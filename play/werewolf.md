# Werewolf — one night and one day

## What it shows

A game master coordinates four separate players. Players exchange private role actions and public statements over Mail, rather than one model narrating every role.

## Time

10–15 minutes, estimated. Not yet tested on a fresh install.

## Before you start

Start one managed CLI agent as GM. Allow four child sessions within your account allowance. Use Sonnet for Claude children, never Haiku; no Codex `--worktree`. Mail privacy here is a game convention: the operator can inspect history. Projecting all Mail reveals roles, so show the GM's daytime summary if you want suspense.

## Prompt

```text
Use the installed ORRERY /delegate skill to run one English-language werewolf game. You are the GM; start exactly four real player children, no further children. Use authenticated available CLIs, Sonnet for Claude children, never Haiku, and no --worktree for Codex. Keep the current directory and existing ORRERY project identity. No project file edits, purchases, external posts, or native subagent substitutes.

Embed the full player task. Each player's first tool action must send you ORRERY Mail with subject "werewolf ready" and body "ready", before waiting for instructions through the documented Mail mechanism. Wait for all four ready messages. If a launch or Mail failure prevents this, report it and stop; never invent players or votes.

Privately assign exactly one werewolf, one seer, and two villagers using the server-assigned agent names. Players communicate through actual ORRERY Mail. The GM sends each role assignment privately to that player; only the GM receives night actions. Role secrecy is a gameplay rule, not protection from the human operator.

Run only Night 1 and Day 1. At night the wolf sends the GM one attack target and the seer one inspection target (not themselves). Collect both before resolving; tell the seer the target's role privately if the seer survives. Announce one eliminated player, without exposing remaining roles. The eliminated player neither speaks nor votes during the day.

For each of two daytime statement rounds, every living player sends a 2–3 sentence English statement to all other living players and cc the GM. Supply the exact recipient list. Wait for all statements in round 1 before starting round 2. No extra statement rounds. Then collect one private ballot from each living player to the GM, with no self-vote. A unique highest vote eliminates that player; a tie means no daytime elimination. Town wins if the wolf is eliminated; otherwise a living wolf at parity wins; if neither condition holds, stop and declare the one-day limit a draw.

Children must use observed messages, not fictitious claims of communication. You validate allowed targets, round counts, and ballots. Report the final roles, night/day events, vote table, outcome, and actual Mail evidence. Limit the game to ten minutes of coordination; if incomplete, report what is missing instead of starting another day. Keep the player's processes available until the human has accepted the result.

End by listing the four spawned agents. Tell the operator to use cockpit EXIT for those players and, when finished, this GM. A Mail message saying "bye" is not a shutdown command. Do not stop unrelated agents.
```

## What you should see

Four children and four `werewolf ready` messages appear. Mail includes private night actions, two rounds of messages among living players, and ballots to the GM. TELEMETRY shows links beyond parent–child communication. The final report reconciles roles, elimination, votes, and the stated win/draw rule.

## If it doesn't work

Check each player's first ready message and terminal. Waiting for a role is normal after ready. If a player is stuck behind authentication, approval, or contact policy, the GM should name the failed step and mark the game incomplete. A narrated game without messages is not a successful run. After accepting the report, use cockpit EXIT for all exercise participants; see [lifecycle controls](../guide/cockpit-and-telemetry.md).
