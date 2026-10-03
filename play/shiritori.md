# Shiritori — the core installation check

## What it shows

One coordinator starts one real child and exchanges ORRERY Mail. Three round trips make launch, identity, and communication visible.

## Time

5 minutes, estimated. Not yet tested on a fresh install.

## Before you start

First, follow the cockpit’s **Your first flight** guide to learn the basic controls ([workshop entry](../README.md#your-first-flight-then-shiritori)); then use the setup below for this exercise.

ORRERY and at least one signed-in CLI must work. Start one managed agent in an empty workshop folder. This game writes no game-output files; delegation may create its required temporary task files. Choose Sonnet for a Claude child; do not use Haiku. Do not add `--worktree` to a Codex child. See the [installed delegate skill](https://github.com/gyroid-eth/orrery-telemetry/blob/master/skills/delegate/SKILL.md).

## Prompt

```text
/delegate Start exactly one child agent, then play Japanese shiritori with it over ORRERY Mail (send_message), three turns each: exactly three round trips.

Read and follow the installed ORRERY delegate skill. Use /delegate to create the child and ORRERY Mail (send_message) for every ready message and game move. Do not use Claude Code's built-in Agent or SendMessage tools. Include these same tool requirements in the child's embedded task. Use an available authenticated CLI: prefer a child from the other provider if available; otherwise use this provider. For a Claude child select Sonnet, never Haiku. Do not use --worktree for a Codex child. Keep the current working directory and existing ORRERY project identity. No native subagent tool, simulated dialogue, recursive delegation, game-output file edits, purchases, or external posting. Temporary task files required by the delegate skill are allowed.

Put the complete rules in the child's embedded task. Its first action must send me ORRERY Mail with subject "shiritori ready" and body "ready", then wait through the documented Mail mechanism. Use the server-assigned names, not invented identities. If launch or Mail fails, report the exact failure and stop.

Rules: use common Japanese words in hiragana. Each word starts with the last hiragana character of the previous word. No repeated words; no word ending in ん. Match the exact ending character, including voicing; avoid small kana and long-vowel marks. Include romaji and a short English meaning so an English-speaking audience can follow.

I am the parent player. My first word is りんご (ringo, apple). Send it to the child as "shiritori round 1". The child replies with a valid word; I validate it and send my next word for round 2; then repeat for round 3. This is six words total: parent/child, parent/child, parent/child. The ready message is not a word move. The child waits for actual messages and replies only once per numbered round. Permit one corrected reply if a move is invalid; report a failed game if it remains invalid.

At the end show a six-row table: round, sender, hiragana, romaji, meaning, and observed Mail ID when available. Check the chain and repetition explicitly. Distinguish actual messages from missing evidence; do not invent IDs. Tell the human which child was created and that cockpit EXIT is the cleanup action after accepting the result. Finish after three round trips, with no new game.
```

## What you should see

A new child appears in the cockpit roster and mini-orrery. The Mail drawer contains `shiritori ready`, then round 1–3 in both directions. TELEMETRY shows the parent/child relationship and traffic. The final table has six valid word moves, beginning with りんご. Check the messages against the table.

## If it doesn't work

No child: inspect the coordinator's launcher error and CLI login. Child exists but is silent: open its terminal and look for approval or a question; check `ready` before assuming failure. No real Mail: treat the installation check as failed, even if a story appears in the terminal. Stop at the reported failure and use the [FAQ](../guide/faq.md) or [upstream install guide](https://github.com/gyroid-eth/orrery/blob/master/docs/en/install.md). After acceptance, use cockpit EXIT for the child.
