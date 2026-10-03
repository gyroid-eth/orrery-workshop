# One paper → a checked reading note

## What it shows

A Claude writer drafts a note; a Codex checker compares it with the paper's actual text and figures. Mail carries findings, fixes, and confirmation of the revised draft.

## Time

10–15 minutes, estimated for a bundled preconverted paper. Not yet tested on a fresh install.

## Before you start

First, follow the cockpit’s **Your first flight** guide to learn the basic controls ([workshop entry](../README.md#your-first-flight-then-shiritori)); then use the setup below for this exercise.

Install ORRERY, sign in to both Claude Code and Codex inside your execution environment, then install the [research set](https://github.com/gyroid-eth/orrery/blob/master/docs/en/research-set.md):

```bash
curl -fsSL https://raw.githubusercontent.com/gyroid-eth/orrery/master/scripts/research-set.sh | bash
```

It installs the [digest-paper add-on](https://github.com/gyroid-eth/orrery-digest-paper) and unpacks the public [demo vault](https://github.com/gyroid-eth/orrery-demo-vault). Read the printed paths and team availability. Open that vault in Obsidian, then start a cockpit CLI agent with that same vault as its **primary working directory** (the WSL path on Windows). Use the bundled Markdown and figures; this exercise does not require Mistral credit or a PDF conversion. Leave the supplied reference notes intact.

## Prompt

```text
/digest-paper Create one English reading note from a bundled, already converted paper, using a Claude writer and a Codex checker over real ORRERY Mail.

The primary working directory of this session is the demo vault I opened in Obsidian; use that exact directory as vault_root, and report it before starting. If this directory does not contain the demo vault's 20_MDPapers and 10_Reference folders, stop and explain the setup mismatch instead of guessing a vault elsewhere.

Read and follow the research-set digest-paper add-on at ~/.agentstack/addons/digest-paper/src/skills/digest-paper/SKILL.md. Use that explicit file even if a different digest-paper skill already exists. Find a bundled paper Markdown under 20_MDPapers whose linked figure images are present; use the first suitable path in lexical order and report the chosen source. This authorizes that single bundled paper only, no internet paper download or OCR. If no suitable converted input exists, stop and report it.

Check the add-on's team availability. Require cross-vendor pairing: Claude writes and Codex reviews, whichever role this coordinator takes. If both cannot run, stop and explain; do not silently use a same-vendor pair. Start only the counterpart required by the installed skill, using its actual ORRERY delegation and Mail workflow. Do not use Haiku or add --worktree to a Codex child.

Use a new output folder inside this vault: 10_Reference/Notes/workshop-r2, adding the next unused numeric suffix if needed. Do not overwrite the bundled reference note or previous runs. No new API-credit purchase, Mistral OCR, external publication, or reading unrelated private files. Ordinary signed-in CLI usage is allowed.

The writer must read the paper and open each figure it discusses. The checker must independently read source passages and open the relevant figures, send specific findings over ORRERY Mail, and confirm the revised draft rather than merely accepting the writer's claims. Follow the add-on's evidence, bundle validation, and publish steps. A tool result or attractive prose alone does not prove the science is right.

Finish with the published note path, actual writer/checker identities and providers, review status, source/figure evidence checked, fixes made, and remaining uncertainty. If a figure or provider is unavailable, say so and do not claim it was checked. Keep the child until I have inspected and accepted the note; tell me which agent to EXIT afterward.
```

## What you should see

Two agents with different providers, a source-backed draft, and Mail containing findings plus confirmation after revision. Open the finished note in Obsidian: links and figure embeds should resolve. Its review metadata should identify cross-vendor pairing and the actual status. Compare at least one claim and figure with the source yourself.

## If it doesn't work

Check the research-set output, Linux CLI availability on WSL, exact vault working directory, and image paths. The [add-on skill](https://github.com/gyroid-eth/orrery-digest-paper/blob/main/skills/digest-paper/SKILL.md) describes writer/checker roles and failures. If only one provider works, choose another exercise; an explicitly labeled same-vendor fallback is a different trial. Do not claim independent checking when the counterpart never started. Local conversion, if you later choose it, avoids an OCR upload but does not make model inference offline. After acceptance, EXIT the spawned child.
