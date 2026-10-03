# FAQ

## Does ORRERY replace Claude Code or Codex?

ORRERY provides a cockpit, Mail coordination, and visibility around agents. You still need an available CLI and its authentication. Installing ORRERY does not create a model account or grant model access. See [installation](https://github.com/gyroid-eth/orrery/blob/master/docs/en/install.md).

## Do I need both companies' models?

One working CLI is enough for shiritori. The paper exercise deliberately requires a Claude writer and a Codex checker. The add-on can also use two agents of one kind, but that is a same-company check and must be labeled accordingly; choose another exercise if the second provider is unavailable.

## Are these exercises free?

They require no new API-credit purchase or external publication. Existing CLI account usage still applies, and parallel children consume more allowance. The default paper exercise uses a bundled, already converted paper; it does not invoke Mistral OCR. The optional PDF-to-Markdown path has separate services and costs. See [research set](https://github.com/gyroid-eth/orrery/blob/master/docs/en/research-set.md).

## Does local conversion mean my paper stays offline?

Local PDF conversion avoids uploading it to an OCR service. It does not prevent the writer/checker from sending text or images to its model provider. Use the public demo paper. For sensitive work, assess every provider, MCP server, connector, hook, and filesystem boundary; [settings](settings.md) explains the layers.

## Does AGENTS.md or CLAUDE.md protect my `.env`?

A request to avoid a file is behavioral guidance. Claude deny rules and targeted hooks cover particular tools; sandbox read restrictions constrain covered processes. Codex read-only mode prevents writes but does not hide readable files. Use explicit read denies and test with synthetic data. See [settings](settings.md) and its official sources.

## The child appeared but stopped talking. Is it broken?

Check its terminal and first `ready` Mail, then look for a question or approval indicator. A waiting child may be healthy. The parent should use ORRERY's documented notification/await mechanism, not a custom inbox polling loop or direct mailbox database reads. Ask it to report the exact launch/authentication/contact failure if one occurs; do not replace real Mail with a simulated conversation.

## Why not a native subagent tool?

These exercises demonstrate separately managed ORRERY agents and actual ORRERY Mail. Use the installed [delegate skill](https://github.com/gyroid-eth/orrery-telemetry/blob/master/skills/delegate/SKILL.md); a built-in subagent can have a different lifecycle and coordination path.

## Can another model's review guarantee correctness?

No. Independent starting points can reveal different mistakes, but models can share blind spots. Ask for source passages, figure evidence, counterexamples, or executable checks, and inspect the revised result yourself.

## Windows installed something, but Codex is unavailable.

Run the setup and Linux CLIs inside WSL2 Ubuntu. A Windows executable found through `/mnt/` is not the Linux CLI expected by these exercises. Check the installer/doctor output and [upstream WSL guidance](https://github.com/gyroid-eth/orrery/blob/master/docs/en/install.md). Use the browser demo or a neighbor's screen if time is short.

## All my WSL agents stopped responding. Must I reboot Windows?

A [user report in issue #189](https://github.com/gyroid-eth/orrery-telemetry/issues/189) describes WSL2 exhausting memory and swap while many agents were running. No OOM-killer event was logged; the processes remained alive but unresponsive. A long transcription job was the suspected trigger, not proven by per-process memory traces. This is a reported incident, not a prediction that every WSL installation behaves this way.

For prevention, EXIT children after their results are accepted: the report measured roughly 300 MB for an idle agent. Split long inputs into smaller jobs. Review WSL2's `memory` and `swap` settings in `.wslconfig` for your host; larger limits do not replace workload control. See [Microsoft's WSL configuration reference](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#main-wsl-settings).

A general per-job limit example, separate from the issue's reported experiment, is:

```bash
systemd-run --user --scope -p MemoryMax=2G -p MemorySwapMax=512M python3 job.py
```

Replace `python3 job.py` with your existing heavy command and choose limits appropriate to your host/workload. This requires systemd enabled in WSL, a working user manager, and cgroup v2 memory control available to that manager; it is not a command for Windows PowerShell. `--scope` creates a transient scope; `-p` sets unit properties. `MemoryMax` imposes a hard memory limit, with OOM action inside the unit when necessary; `MemorySwapMax` caps its swap use. If the manager or limits cannot be applied, stop and diagnose rather than rerunning the job without limits. The example is **not yet tested on WSL** and is not claimed to be the issue's exact command. Sources: [systemd-run official man-page source](https://github.com/systemd/systemd/blob/main/man/systemd-run.xml), [systemd resource-control official man-page source](https://github.com/systemd/systemd/blob/main/man/systemd.resource-control.xml), [Microsoft: systemd in WSL](https://learn.microsoft.com/en-us/windows/wsl/systemd).

If WSL is unresponsive, Windows itself usually does not need a reboot. In **Windows PowerShell**, run:

```powershell
wsl --shutdown
```

Then reopen Ubuntu and restart your ORRERY services/sessions as needed. This terminates **all running WSL distributions and their processes**, so unsaved work can be lost; Windows applications can stay open. See [Microsoft's shutdown command](https://learn.microsoft.com/en-us/windows/wsl/basic-commands#shutdown). This recovery was not tested as part of these workshop prompts.

## How do I stop the game?

Use cockpit EXIT for the participating managed agents after checking the selection. Mail saying “bye” does not stop a process. See [cockpit and Telemetry](cockpit-and-telemetry.md).

## Have these prompts been tested?

Not yet tested on a fresh install. No run results are asserted here. Record macOS and fresh WSL2 trials using [the template](../tests/README.md), including real messages, output, stalls, and cleanup. Estimated times are workshop targets.
