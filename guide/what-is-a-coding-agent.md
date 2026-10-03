# What is a coding agent?

A coding agent combines a model with a harness that can inspect a workspace, propose changes, run tools, and check results. You give it a goal; it chooses intermediate steps within the tools and permissions you provide. A chat product can also have tools: inspect the actual capabilities rather than assuming a chat/agent label guarantees access.

## Six things to recognize

| Concept | Example | Question to ask |
| --- | --- | --- |
| Agent loop | Read a file → edit → run a check → revise | Did the result pass a check that matters? |
| Instructions | CLAUDE.md or AGENTS.md says how to work | Which files were loaded from this launch directory? |
| Permissions | A tool call needs approval or is denied | Is this a prompt policy or an enforced access boundary? |
| Skills | `/delegate` describes how to launch and coordinate a child | Is the named skill actually installed? |
| Hooks | A program inspects an event such as PreToolUse | Which calls does the hook cover, and how does it fail? |
| ORRERY | Separate agents exchange Mail while you watch their work | Is there real traffic, a checked artifact, and a clear owner? |

Instructions, skills, and hooks are distinct. An instruction is guidance. A skill is a reusable workflow the agent must follow. A hook is executable code attached to an event. None of these alone guarantees a correct result or complete secret isolation; see [settings](settings.md).

## A small collaboration

In [shiritori](../play/shiritori.md), your agent uses ORRERY's installed `/delegate` skill to launch one separate child. They use ORRERY Mail for three round trips. You inspect the messages and the six-word result. Text that merely describes two characters talking does not demonstrate delegation.

The same pattern supports useful work: [a writer and checker](../play/paper-to-note.md), [independent reviews](../play/two-reviewers.md), or [separate file owners](../play/split-the-work.md). The coordinating agent still needs to read the results, resolve disagreements, and verify the final artifact.

See the official introductions to [Claude Code](https://code.claude.com/docs/en/overview) and [Codex CLI](https://learn.chatgpt.com/docs/codex/cli), and the public [ORRERY delegate skill](https://github.com/gyroid-eth/orrery-telemetry/blob/master/skills/delegate/SKILL.md). The play prompts are drafts awaiting fresh-install trials.
