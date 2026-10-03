# Split a small workshop kit, then integrate it

## What it shows

Separate file owners work in parallel, exchange pointers by Mail, check each other's outputs, and hand back a usable kit. The parent owns integration.

## Time

10–15 minutes, estimated. Not yet tested on a fresh install.

## Before you start

First, follow the cockpit’s **Your first flight** guide to learn the basic controls ([workshop entry](../README.md#your-first-flight-then-shiritori)); then use the setup below for this exercise.

Start one managed agent in an empty disposable folder with permission to create a new subfolder. Three children will own three different files. This is a synthetic writing task with no downloads or external publication.

## Prompt

```text
Create a small classroom kit for a 20-minute paper-token planning exercise: 12 students, four teams of three, no software installation, network access, or paid materials. The activity should compare two planning strategies using paper tokens, with measurable outcomes and an English debrief. Four phases must sum to exactly 20 minutes.

Use the installed ORRERY /delegate skill to start exactly three real children in parallel. Use available authenticated providers, Sonnet for Claude children, never Haiku, and no --worktree for Codex. All keep the existing ORRERY project identity. Before writing, create a new workshop-kit folder in the current working directory, or the next unused numbered folder. Assign disjoint paths and follow all project file-reservation rules. Nobody edits another owner's file or overwrites existing material.

Child A owns instructions.md: participant directions, team roles, materials, and a four-phase schedule.
Child B owns exercise.md: two planning strategies, token rules, and a scoring example. Send A and B the same four-phase schedule you choose, summing to 20 minutes, so the files agree.
Child C owns checklist.md: derive acceptance criteria independently from this request while A and B write. Include arithmetic, feasibility, understandable rules, score reproducibility, and consistency checks. C does not edit A or B's files.

Embed the full bounded task and require each child's first action to send the parent actual ORRERY Mail with subject "kit ready" and body "ready". Children send completion paths and key findings, not whole files. Once A and B finish, have them send their paths directly to C, cc the parent. C reads both files and sends specific findings to the corresponding owner, cc the parent. A and B fix their own files and send C the revised paths. C verifies those revisions and reports any remaining uncertainty.

No recursive delegation, native subagent substitute, simulated Mail, purchases, downloads, or external posts. If launch, Mail, or reservation fails, report the exact failure and stop the affected work.

You own README.md inside the new kit. Read all three outputs before integrating. Write a short index, explain the activity's purpose, link the files, and report checks you actually performed. Independently verify 12 students = four teams of three, the four phases sum to 20 minutes, the scoring example is reproducible, and all relative links resolve. If the activity is incomplete, label it incomplete rather than passing it based only on child claims.

Finish with the kit path, each file owner, real handoffs, checks, fixes, and open issues. Name the spawned agents for cockpit EXIT after I accept the kit. Do not stop unrelated agents.
```

## What you should see

Three children begin separate bounded tasks. Mail shows early ready messages, A/B → C file pointers, C's findings, and confirmation of revisions. The folder contains README.md, instructions.md, exercise.md, and checklist.md. Read the integrated kit and try its scoring example; traffic alone is not success.

## If it doesn't work

Files disagree: the parent should use the shared phase schedule and ask the owning child to fix its file. No peer handoff: inspect the actual recipients and parent report. A reservation conflict should be reported, not bypassed. Incomplete output must stay labeled incomplete. See [delegate](https://github.com/gyroid-eth/orrery-telemetry/blob/master/skills/delegate/SKILL.md) and [cockpit controls](../guide/cockpit-and-telemetry.md). After acceptance, EXIT the three children.
