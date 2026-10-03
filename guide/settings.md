# Settings: instructions, permissions, hooks, and secrets

Instructions shape behavior; permissions and sandbox policies constrain specific capabilities. A hook can reject a covered tool call, but it is not a universal filesystem boundary. Launch directory and version matter.

Official documentation checked on 3 October 2026. The reference CLI versions used for the underlying documentation check were Claude Code 2.1.288 and Codex CLI 0.159.2. This is a public teaching guide for local CLIs, not a claim that every participant has those versions or features. Examples are generic; do not paste secrets into them. Start demonstrations in a disposable repository and use synthetic `.env` contents.

## 1 — Which instruction files are loaded?

Instructions guide behavior; they do not grant or revoke file access. Launch location determines the initial instruction chain.

| Claude Code | Codex |
| --- | --- |
| Managed → user `~/.claude/CLAUDE.md` → ancestor/project instructions. At each directory, `CLAUDE.local.md` follows `CLAUDE.md` | Codex home → project root to launch directory |
| Ancestors are discovered upward, concatenated from root downward. Descendant instructions load when their files are read | Each directory contributes at most one: `AGENTS.override.md`, otherwise `AGENTS.md`, otherwise configured fallback |
| `@path` imports resolve relative to the containing file; maximum four hops | Default combined instruction limit: 32 KiB; configure `project_doc_max_bytes` |

Claude also supports `.claude/CLAUDE.md`. Keep each instruction file concise; 200 lines is a recommendation. Codex home defaults to `~/.codex`, or `CODEX_HOME`. Without a project root, Codex checks only cwd. Its startup discovery stops at cwd. At Codex home, the first non-empty `AGENTS.override.md` replaces `AGENTS.md`; it is not appended to it. More specific project instructions come later in the chain. [Claude memory](https://code.claude.com/docs/en/memory), [OpenAI AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)

```markdown
<!-- Claude: repo/CLAUDE.md -->
Run pytest after changing Python code.
@docs/workflow.md
```

```markdown
<!-- Codex: repo/AGENTS.md -->
Run pytest after changing Python code.
Read docs/workflow.md before changing deployment code.
```

| Launch directory | Claude project chain | Codex project chain |
| --- | --- | --- |
| `repo/` | Root CLAUDE; descendant instructions load on read | Root AGENTS |
| `repo/services/api/` | Root → services → api CLAUDE; ancestors outside the repo can also contribute | Root → services AGENTS → api override |

Global instructions also contribute. Current Claude can automatically load AGENTS when no ancestor/project CLAUDE instructions exist; user settings can select both:

```json
{
  "pluginConfigs": {
    "agents-md@builtin": {
      "options": { "instructionFiles": "claude-md-and-agents-md" }
    }
  }
}
```

This requires supported versions, starting with 2.1.277. Codex-compatible AGENTS does not imply identical loading behavior. [Claude AGENTS support](https://code.claude.com/docs/en/memory#agentsmd)

## 2 — Configure permissions and execution boundaries

### Claude Code

Settings precedence, highest first: managed → CLI → local → project → user. Permission lists and hooks merge. [Settings precedence](https://code.claude.com/docs/en/settings#settings-precedence)

| Scope | File |
| --- | --- |
| User | `~/.claude/settings.json` |
| Project | `.claude/settings.json` in the primary working directory |
| Local | `.claude/settings.local.json`, normally at Git root on current versions |
| Managed | macOS: `/Library/Application Support/ClaudeCode/managed-settings.json`; Linux: `/etc/claude-code/managed-settings.json`; Windows: `C:\Program Files\ClaudeCode\managed-settings.json` |

Managed policy also supports MDM and server delivery. Start the demonstration at repository root: ancestor CLAUDE loading does not mean ancestor shared settings load. [Managed deployment](https://code.claude.com/docs/en/managed-settings), [Settings locations](https://code.claude.com/docs/en/settings#where-claude-code-keeps-the-local-file-in-a-git-repository)

```json
{
  "permissions": {
    "defaultMode": "default",
    "allow": ["Bash(pytest *)"],
    "ask": ["Bash(git push *)"],
    "deny": ["Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)"]
  }
}
```

Deny wins over ask, which wins over allow. Relative paths use cwd; absolute file rules use `//`, home rules use `~/`. File rules cover built-in tools and recognized shell file operations, not arbitrary indirect reads by scripts. [Permissions](https://code.claude.com/docs/en/permissions)

Modes: `default` for manual approval; `acceptEdits` for automatic edits; `plan` for investigation; `auto` for classifier review; `dontAsk` to reject actions requiring prompts; `bypassPermissions` for externally isolated environments. Auto is the built-in interactive terminal/VS Code default from 2.1.283, subject to availability. Configure auto in user/managed settings or CLI, not project/local settings. [Permission modes](https://code.claude.com/docs/en/permission-modes)

```bash
claude --permission-mode default
claude --permission-mode auto
```

### Codex

User configuration lives in `~/.codex/config.toml`; trusted project configuration layers run from root to cwd. [Config basics](https://learn.chatgpt.com/docs/config-file/config-basic)

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
[sandbox_workspace_write]
network_access = false
```

```bash
codex --sandbox workspace-write --ask-for-approval on-request
codex --sandbox read-only --ask-for-approval never
```

Approval determines when to ask; sandbox determines command access. `never` preserves restrictions. `read-only` prevents writes; it does not hide secrets. `danger-full-access` removes sandbox restrictions. CLI string choices are `on-request` and `never`; config also supports granular approval categories. `untrusted` is retired. Auto-review routes requests to a reviewer. [Approvals and security](https://learn.chatgpt.com/docs/agent-approvals-security)

For a reusable config profile, put these keys in `~/.codex/lecture.config.toml` and run `codex --profile lecture`. From 0.134.0, use separate files rather than `[profiles.lecture]`. Config profiles differ from filesystem permission profiles. [Profiles](https://learn.chatgpt.com/docs/config-file/config-advanced#profiles)

## 3 — Hooks run code at defined events

| Event | Purpose |
| --- | --- |
| SessionStart | Add startup/resume context |
| PreToolUse | Inspect and potentially deny a call before execution |
| PostToolUse | Validate completed work; cannot undo the original action |
| Stop | Check whether a turn may finish |
| SessionEnd | Cleanup |

### Claude: block an explicit `.env` Read

Add to project settings:

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "^Read$",
      "hooks": [{
        "type": "command",
        "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/block-env.py\""
      }]
    }]
  }
}
```

Save this as `.claude/hooks/block-env.py`:

```python
import json
import sys
from pathlib import Path
event = json.load(sys.stdin)
path = Path(event["tool_input"]["file_path"])
if not path.is_absolute():
    path = Path(event["cwd"]) / path
if any(p.name == ".env" or p.name.startswith(".env.")
       for p in (path, path.resolve())):
    print("Blocked: environment files are private.", file=sys.stderr)
    sys.exit(2)
```

Matchers select tools; stdin supplies JSON arguments. Exit 2 blocks PreToolUse. Ordinary failures or timeouts can leave the call unblocked. This example covers Read, not Bash, Grep or MCP. [Hook tutorial](https://code.claude.com/docs/en/hooks-guide)

Alternatively, return this stdout JSON with exit 0:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "Environment files are private."
  }
}
```

Use stderr for diagnostics. [Hook reference](https://code.claude.com/docs/en/hooks)

### Codex: supported hooks and ORRERY

Codex supports user/project `hooks.json`, inline config hooks and plugin hooks. Review and trust non-managed definitions with `/hooks`. Changes require renewed trust. [Codex hooks](https://learn.chatgpt.com/docs/hooks)

`~/.codex/hooks.json` example:

```json
{
  "hooks": {
    "SessionStart": [{
      "matcher": "startup|resume",
      "hooks": [{
        "type": "command",
        "command": "python3 ~/.codex/hooks/session-context.py"
      }]
    }]
  }
}
```

`~/.codex/hooks/session-context.py`:

```python
import json
print(json.dumps({"hookSpecificOutput": {
    "hookEventName": "SessionStart",
    "additionalContext": "Use synthetic lecture data only."
}}))
```

PreToolUse supports Bash, apply_patch, MCP and local function tools. Denial JSON or exit 2 can block supported calls. Hosted tools are outside this path; write_stdin does not repeat PreToolUse. Treat hooks as targeted checks. [Tool coverage](https://learn.chatgpt.com/docs/hooks#tool-coverage)

ORRERY's Claude integration includes a PreToolUse reservation check for Edit/Write. The public AgentStack Codex plugin contributes lifecycle hooks; plugin hooks live under Codex's plugin cache, and hook trust/state is managed by Codex. Do not infer that Claude's reservation guard also covers Codex writes: follow the installed coordination instructions and reserve files explicitly where required. The exact installed plugin path and event coverage depend on the release; inspect them rather than assuming a versioned cache path. [ORRERY reservation hook](https://github.com/gyroid-eth/orrery-telemetry/blob/master/hooks/check-file-reservation.sh), [ORRERY Codex plugin hooks](https://github.com/gyroid-eth/orrery-telemetry/blob/master/integrations/codex_app/plugin/hooks/hooks.json), [Codex hook locations](https://learn.chatgpt.com/docs/hooks#where-codex-looks-for-hooks)

## 4 — Protect secrets in layers

| Layer | What it provides |
| --- | --- |
| Written instructions | Behavioral guidance; no access boundary |
| Deny rules | Tool/path checks in Claude; explicit filesystem denies for Codex sandboxed commands |
| PreToolUse hooks | Rejection of matched calls; coverage and failure behavior matter |
| Sandbox | OS enforcement for covered execution, with explicit read restrictions |
| Separate OS user / container / VM | Broader isolation when secrets and credentials are inaccessible |

### Claude: cover both file tools and shell commands

```json
{
  "permissions": {
    "deny": ["Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)"]
  },
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "allowUnsandboxedCommands": false,
    "filesystem": {
      "denyRead": ["./.env", "./.env.*", "./secrets/**"]
    }
  }
}
```

Default sandbox reads are broad. denyRead limits sandboxed shell processes; Read needs file permissions. Current Claude also merges Read denies into sandbox policy. Hooks, MCP servers and built-in file tools run outside the Bash sandbox. [Sandbox boundaries](https://code.claude.com/docs/en/sandboxing)

### Codex: explicitly deny reads

Use this beta permission profile instead of older sandbox settings. Remove `sandbox_mode`, `sandbox_workspace_write`, and `--sandbox` so they do not select the older policy:

```toml
approval_policy = "never"
default_permissions = "lecture"
[permissions.lecture]
extends = ":workspace"
[permissions.lecture.filesystem]
":root" = "deny"
":minimal" = "read"
":tmpdir" = "deny"
":slash_tmp" = "deny"
[permissions.lecture.filesystem.":workspace_roots"]
".env" = "deny"
".env.*" = "deny"
"secrets" = "deny"
```

Inherited workspace access remains; narrower denies protect secrets. Outside reads are denied except minimal runtime paths. Inherited temp access is also denied; builds needing temporary directories require adjustment. This governs local sandboxed commands; MCP, connectors and other capabilities need separate controls. [Permission profiles](https://learn.chatgpt.com/docs/permissions)

A dedicated user helps only when that user lacks access to the secrets, privileged escalation and shared credentials. `chmod 600` does not hide a file from an agent running as its owner. This is an OS-design inference, not a tested deployment recipe. Official guidance also discusses VM/container isolation. [Claude security](https://code.claude.com/docs/en/security), [Codex isolation](https://learn.chatgpt.com/docs/agent-approvals-security)

Demonstrate with synthetic secrets only. Test a file-tool read, a direct shell read and an indirect Python read. Observe actual rejection rather than trusting the agent's statement that a rule is loaded.


## Verification and limits

The source guide's JSON/TOML examples were syntax-checked; its Python Read hook was exercised with synthetic relative/absolute paths, `.env` variants, a symlink, and an ordinary source file. The copied examples are documentation, not an installed policy. They still need end-to-end checks on the target CLI and OS.

Not established by those static checks:

- The initial instruction chain and actual hook invocation in a fresh CLI session.
- Account/provider availability of Claude auto mode or activation of its AGENTS builtin setting.
- Identical `@import` behavior in Codex; the inspected docs do not establish Claude-style imports.
- All OS/IDE/cloud/SDK combinations, MCP/connector isolation, or a separate OS-user deployment.
- Whether an ORRERY reservation check runs on a particular installed Codex release.
- Removal of a secret already provided to a model through another channel.

Before the workshop, recheck versions and linked official docs. Test a file-tool read, a direct shell read, and an indirect Python read against synthetic files; inspect actual denial. Do not turn a 3 October documentation check into a guarantee for every 7 October environment.
