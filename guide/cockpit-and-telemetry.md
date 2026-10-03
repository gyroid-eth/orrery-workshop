# Cockpit and Telemetry

**Cockpit is where you work with agents. Telemetry shows their status, relationships, and history.** Both expose some lifecycle controls; the Telemetry view inside the cockpit remains a distinct interface.

| Surface | Use it for |
| --- | --- |
| Cockpit roster and terminal/composer | Select an agent, give it a task, read output, answer questions |
| Cockpit NEW AGENT | Choose provider/model and primary working directory; launch a CLI agent |
| Cockpit Mail rail and drawer | Read actual ORRERY Mail threads; the rail is display-only |
| Cockpit mini-orrery | See parent–child lineage and select the corresponding agent |
| TELEMETRY → DECK | Read states, model/task information, and agents needing your attention |
| TELEMETRY → NETWORK | Inspect relationships and message traffic among agents |
| Telemetry history and lifecycle controls | Inspect previous work; RESUME an eligible session or request EXIT |

A Mail card opens a conversation, not the sender's terminal. Select a roster tile or lineage node to work with that agent. Message activity is evidence of communication, not evidence that a claim is correct. Read both the messages and the checked output.

## During an exercise

1. Use NEW AGENT to start one coordinator in your workshop folder. For the paper exercise, select the demo vault folder printed by the research-set installer.
2. Paste the whole Prompt block into its input and submit it. A literal `/delegate` line is a skill request followed by the task; in a client that does not expose slash commands, ask it to read and follow the installed delegate skill.
3. Watch for a child in the roster/lineage and its first `ready` Mail. In the Mail drawer, inspect sender, recipient, subject, and content.
4. Open TELEMETRY to see states and relationships. A question or approval indicator means a human answer may be needed; silence alone does not prove a crash.
5. Read the coordinator's final result and verify the exercise's expected evidence.
6. When you have accepted the result, use **EXIT** for the agents created by your exercise. Re-check the targets; cockpit Select mode supports bulk EXIT with confirmation. Do not stop unrelated sessions.

Sending “bye” by Mail is a message, not a process shutdown. Closing a cockpit session tab can merely detach it. EXIT requests the process to exit; verify it has stopped. RESUME depends on retained session history and supported runtime/provider behavior, so it is not a universal undo button.

These instructions target managed CLI agents. Native application sessions can have different terminal and lifecycle support.

Sources: [ORRERY cockpit usage](https://github.com/gyroid-eth/orrery/blob/master/docs/en/usage.md), [ORRERY Telemetry](https://github.com/gyroid-eth/orrery-telemetry), [delegation workflow](https://github.com/gyroid-eth/orrery-telemetry/blob/master/skills/delegate/SKILL.md). The browser [demo](https://agentstack-demo.pages.dev/) lets you inspect the interface without a local installation.
