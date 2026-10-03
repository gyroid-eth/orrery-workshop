# ORRERY workshop

Coding agents, visible teamwork, and small experiments for the Osaka University System Science Seminar, 7 October 2026. The session is in English, lasts 60 minutes, and supports up to 136 participants. [日本語](README.ja.md)

Install, follow **Your first flight** in the cockpit, then run **shiritori** to check real delegation and Mail. Afterwards, try a paper reading note or choose another exercise. These are workshop drafts: every exercise prompt is **Not yet tested on a fresh install**. Times below are estimates, not measured results.

## The session

| Minutes | Activity |
| --- | --- |
| 0–15 | What a coding agent is; instructions, permissions, and ORRERY |
| 15–25 | Install, sign in, and follow Your first flight |
| 25–30 | Everyone runs [shiritori](play/shiritori.md) |
| 30–45 | [One paper → a checked note](play/paper-to-note.md) |
| 45–60 | Choose an exercise, compare results, and ask questions |

## Install

Use a terminal on macOS. On Windows, use **Ubuntu inside WSL2**, rather than PowerShell. Follow the [upstream installation guide](https://github.com/gyroid-eth/orrery/blob/master/docs/en/install.md) for prerequisites and supported environments. Have at least one supported Claude Code or Codex CLI installed and signed in; having both enables the paper exercise's cross-company check. WSL needs Linux CLI installations inside Ubuntu.

```bash
curl -fsSL https://raw.githubusercontent.com/gyroid-eth/orrery/master/scripts/get.sh | bash
```

This downloads and runs the upstream installer. It checks the environment, presents its plan for confirmation, installs ORRERY and ORRERY Telemetry, checks Mail, and opens the cockpit. Read its output and resolve any failed checks before continuing. CLI sign-in and account access are separate from installing ORRERY. The [Telemetry install guide](https://github.com/gyroid-eth/orrery-telemetry/blob/master/docs/install.en.md) has additional prerequisites and diagnostics.

## Your first flight, then shiritori

Follow **Your first flight**, the seven-item guide that appears on the right on your first visit. For its first step, use **NEW AGENT**, an available provider/model, and an empty disposable workshop folder. Choose that agent and send a short greeting below its terminal. Continue with the instructions on screen; the Mail-reading item has a `Mark as read` button. Reopen the guide from **Settings → Getting started → Your first flight** if it is closed.

Next, paste the [shiritori prompt](play/shiritori.md) into that same agent's terminal/composer. A completed first-flight checklist introduces the controls; the three real Mail round trips in shiritori check delegation and communication. Use the research set's demo vault as the working directory for the paper exercise instead.

During free exploration, open **Settings → Getting started → Show help map** to look around the annotated controls, then **Full tour** for sixteen steps covering a child game, pane layout, and Telemetry EXIT / RESUME and replay. Full tour includes the same shiritori prompt; it observes a fresh game, so plan another three round trips if you take the tour after the classroom game. You can leave the tour for later. Follow the [upstream quick start](https://github.com/gyroid-eth/orrery/blob/master/README.en.md#quick-start) for the current entry points.

No exercise requires buying API credit or publishing to an external service. Agents still consume your existing CLI account allowance; running more children consumes more of it. Use public or synthetic inputs. File content may be sent to the selected model provider.

## If installation is unavailable

Open the [browser demo](https://agentstack-demo.pages.dev/) or follow a neighbor's screen. The demo illustrates the interface; it does not prove your local CLI, Mail, or delegation works. You can still inspect instructions, predict a game turn, compare reviewer findings, and discuss which evidence shows real cooperation.

## Read and play

| Exercise | Estimated time | What to look for |
| --- | --- | --- |
| [Shiritori](play/shiritori.md) — core installation check | 5 min | One real child and three Mail round trips |
| [Werewolf](play/werewolf.md) | 10–15 min | A GM and four players exchanging private and public messages |
| [Paper to note](play/paper-to-note.md) | 10–15 min | Claude writes; Codex checks source text and figures |
| [Two reviewers](play/two-reviewers.md) | 5–10 min | Independent findings followed by a peer comparison |
| [Split the work](play/split-the-work.md) | 10–15 min | Separate file ownership, parallel work, and an integrated result |

Read [what a coding agent is](guide/what-is-a-coding-agent.md), [cockpit vs Telemetry](guide/cockpit-and-telemetry.md), [settings and secret protection](guide/settings.md), and the [FAQ](guide/faq.md). Record actual runs using the [trial template](tests/README.md).

## Scope and sources

Material checked against public upstream documentation on 3 October 2026. Installation and skill behavior can change; the linked upstream docs are the source for current setup. Local CLI settings are separate from hosted/cloud agents. These prompts target ORRERY-managed CLI sessions, not native application sessions without a terminal.

This repository uses the same [PolyForm Perimeter 1.0.1 license](LICENSE) as [ORRERY Telemetry](https://github.com/gyroid-eth/orrery-telemetry). Public availability does not remove the license's restrictions.
