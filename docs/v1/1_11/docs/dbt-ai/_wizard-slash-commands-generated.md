# \_wizard-slash-commands-generated

## Slash commands[​](#slash-commands "Direct link to Slash commands")

Type `/` in the composer to open the command picker. Use arrow keys to navigate or keep typing to filter. Press **Tab** to queue a command while a task is running — it executes at the end of the current turn.

### All slash commands[​](#all-slash-commands "Direct link to All slash commands")

| Command                                             | Alias    | Purpose                                                                | Available during task |
| --------------------------------------------------- | -------- | ---------------------------------------------------------------------- | --------------------- |
| [`/model`](#model-and-ai)                           | —        | Choose what model and reasoning effort to use                          | ✗                     |
| [`/providers`](#model-and-ai)                       | —        | Configure model providers and BYOK credentials                         | ✓                     |
| [`/ide`](#review-and-context)                       | —        | Include current selection, open files, and other context from your IDE | ✓                     |
| [`/permissions`](#permissions-and-safety)           | —        | Choose what Wizard is allowed to do                                    | ✗                     |
| [`/keymap`](#customization)                         | —        | Remap TUI shortcuts                                                    | ✗                     |
| [`/vim`](#customization)                            | —        | Toggle Vim mode for the composer                                       | ✗                     |
| [`/setup-default-sandbox`](#permissions-and-safety) | —        | Set up elevated agent sandbox                                          | ✗                     |
| [`/sandbox-add-read-dir`](#permissions-and-safety)  | —        | Let sandbox read a directory: /sandbox-add-read-dir \<absolute\_path>  | ✗                     |
| [`/experimental`](#customization)                   | —        | Toggle experimental features                                           | ✗                     |
| [`/approve`](#customization)                        | —        | Approve one retry of a recent auto-review denial                       | ✓                     |
| [`/memories`](#memory)                              | —        | Configure memory use and generation                                    | ✗                     |
| [`/skills`](#skills-and-extensions)                 | —        | Use skills to improve how Wizard performs specific tasks               | ✓                     |
| [`/hooks`](#skills-and-extensions)                  | —        | View and manage lifecycle hooks                                        | ✓                     |
| [`/review`](#review-and-context)                    | —        | Review my current changes and find issues                              | ✗                     |
| [`/rename`](#session-management)                    | —        | Rename the current thread                                              | ✓                     |
| [`/new`](#session-management)                       | —        | Start a new chat during a conversation                                 | ✗                     |
| [`/resume`](#session-management)                    | —        | Resume a saved chat                                                    | ✗                     |
| [`/fork`](#session-management)                      | —        | Fork the current chat                                                  | ✗                     |
| [`/init`](#session-management)                      | —        | Create an AGENTS.md file with instructions for Wizard                  | ✗                     |
| [`/compact`](#session-management)                   | —        | Summarize conversation to prevent hitting the context limit            | ✗                     |
| [`/plan`](#long-running-tasks)                      | —        | Switch to Plan mode                                                    | ✗                     |
| [`/goal`](#long-running-tasks)                      | —        | Set or view the goal for a long-running task                           | ✓                     |
| [`/agent`](#long-running-tasks)                     | —        | Switch the active agent thread                                         | ✓                     |
| [`/side`](#long-running-tasks)                      | —        | Start a side conversation in an ephemeral fork                         | ✓                     |
| [`/btw`](#long-running-tasks)                       | —        | Start a side conversation in an ephemeral fork                         | ✓                     |
| [`/copy`](#review-and-context)                      | —        | Copy last response as markdown                                         | ✓                     |
| [`/raw`](#review-and-context)                       | —        | Toggle raw scrollback mode for copy-friendly terminal selection        | ✓                     |
| [`/diff`](#review-and-context)                      | —        | Show git diff (including untracked files)                              | ✓                     |
| [`/mention`](#review-and-context)                   | —        | Mention a file                                                         | ✓                     |
| [`/status`](#session-info)                          | —        | Show current session configuration and token usage                     | ✓                     |
| [`/config`](#session-info)                          | —        | View and manage configuration                                          | ✓                     |
| [`/debug-config`](#session-info)                    | —        | Show config layers and requirement sources for debugging               | ✓                     |
| [`/title`](#customization)                          | —        | Configure which items appear in the terminal title                     | ✓                     |
| [`/statusline`](#customization)                     | —        | Configure which items appear in the status line                        | ✓                     |
| [`/theme`](#customization)                          | —        | Choose a syntax highlighting theme                                     | ✗                     |
| [`/pets`](#customization)                           | `/pet`   | Choose or hide the terminal pet                                        | ✗                     |
| [`/overview`](#review-and-context)                  | —        | Render the dbt project overview card                                   | ✓                     |
| [`/mcp`](#skills-and-extensions)                    | —        | List configured MCP tools; use /mcp verbose for details                | ✓                     |
| [`/apps`](#skills-and-extensions)                   | —        | Manage apps                                                            | ✓                     |
| [`/plugins`](#skills-and-extensions)                | —        | Browse plugins                                                         | ✓                     |
| [`/quit`](#exit)                                    | —        | Exit Wizard                                                            | ✓                     |
| [`/exit`](#exit)                                    | —        | Exit Wizard                                                            | ✓                     |
| [`/feedback`](#session-info)                        | —        | Send feedback to maintainers                                           | ✓                     |
| [`/ps`](#background-terminals)                      | —        | List background terminals                                              | ✓                     |
| [`/stop`](#background-terminals)                    | `/clean` | Stop all background terminals                                          | ✓                     |
| [`/clear`](#session-management)                     | —        | Clear the terminal and start a new chat                                | ✗                     |
| [`/personality`](#model-and-ai)                     | —        | Choose a communication style for Wizard                                | ✗                     |
| [`/realtime`](#realtime-experimental)               | —        | Toggle realtime voice mode (experimental)                              | ✓                     |
| [`/settings`](#realtime-experimental)               | —        | Configure realtime microphone/speaker                                  | ✓                     |
| [`/subagents`](#long-running-tasks)                 | —        | Switch the active agent thread                                         | ✓                     |

***

### Model and AI[​](#model-and-ai "Direct link to Model and AI")

Control the AI model, provider, speed, and response style.

| Command        | Inline args | Description                                    |
| -------------- | ----------- | ---------------------------------------------- |
| `/model`       | —           | Choose what model and reasoning effort to use  |
| `/providers`   | —           | Configure model providers and BYOK credentials |
| `/personality` | —           | Choose a communication style for Wizard        |

***

### Session management[​](#session-management "Direct link to Session management")

Start, resume, branch, and clean up sessions.

| Command    | Inline args | Description                                                 |
| ---------- | ----------- | ----------------------------------------------------------- |
| `/new`     | —           | Start a new chat during a conversation                      |
| `/clear`   | —           | Clear the terminal and start a new chat                     |
| `/resume`  | Yes         | Resume a saved chat                                         |
| `/fork`    | —           | Fork the current chat                                       |
| `/compact` | —           | Summarize conversation to prevent hitting the context limit |
| `/rename`  | Yes         | Rename the current thread                                   |
| `/init`    | —           | Create an AGENTS.md file with instructions for Wizard       |

***

### Review and context[​](#review-and-context "Direct link to Review and context")

Pull information into the session or trigger a code review.

| Command     | Inline args | Description                                                            |
| ----------- | ----------- | ---------------------------------------------------------------------- |
| `/review`   | Yes         | Review my current changes and find issues                              |
| `/diff`     | —           | Show git diff (including untracked files)                              |
| `/mention`  | —           | Mention a file                                                         |
| `/copy`     | —           | Copy last response as markdown                                         |
| `/raw`      | Yes         | Toggle raw scrollback mode for copy-friendly terminal selection        |
| `/overview` | —           | Render the dbt project overview card                                   |
| `/ide`      | Yes         | Include current selection, open files, and other context from your IDE |

***

### Permissions and safety[​](#permissions-and-safety "Direct link to Permissions and safety")

Control what Wizard is allowed to execute.

| Command                  | Inline args | Description                                                           |
| ------------------------ | ----------- | --------------------------------------------------------------------- |
| `/permissions`           | —           | Choose what Wizard is allowed to do                                   |
| `/setup-default-sandbox` | —           | Set up elevated agent sandbox                                         |
| `/sandbox-add-read-dir`  | Yes         | Let sandbox read a directory: /sandbox-add-read-dir \<absolute\_path> |

***

### Customization[​](#customization "Direct link to Customization")

Appearance, keybindings, and UI preferences.

| Command               | Inline args | Description                                        |
| --------------------- | ----------- | -------------------------------------------------- |
| `/theme`              | —           | Choose a syntax highlighting theme                 |
| `/keymap`             | Yes         | Remap TUI shortcuts                                |
| `/vim`                | —           | Toggle Vim mode for the composer                   |
| `/statusline`         | —           | Configure which items appear in the status line    |
| `/title`              | —           | Configure which items appear in the terminal title |
| `/experimental`       | —           | Toggle experimental features                       |
| `/approve`            | —           | Approve one retry of a recent auto-review denial   |
| `/pets` (alias: /pet) | Yes         | Choose or hide the terminal pet                    |

***

### Skills and extensions[​](#skills-and-extensions "Direct link to Skills and extensions")

Manage capabilities Wizard can use during a session.

| Command    | Inline args | Description                                              |
| ---------- | ----------- | -------------------------------------------------------- |
| `/skills`  | —           | Use skills to improve how Wizard performs specific tasks |
| `/hooks`   | —           | View and manage lifecycle hooks                          |
| `/mcp`     | Yes         | List configured MCP tools; use /mcp verbose for details  |
| `/apps`    | —           | Manage apps                                              |
| `/plugins` | —           | Browse plugins                                           |

***

### Long-running tasks[​](#long-running-tasks "Direct link to Long-running tasks")

Manage multi-turn goals, parallel agents, and branched conversations.

| Command      | Inline args | Description                                    |
| ------------ | ----------- | ---------------------------------------------- |
| `/plan`      | Yes         | Switch to Plan mode                            |
| `/goal`      | Yes         | Set or view the goal for a long-running task   |
| `/agent`     | —           | Switch the active agent thread                 |
| `/subagents` | —           | Switch the active agent thread                 |
| `/side`      | Yes         | Start a side conversation in an ephemeral fork |
| `/btw`       | Yes         | Start a side conversation in an ephemeral fork |

***

### Background terminals[​](#background-terminals "Direct link to Background terminals")

Inspect and control shell processes Wizard has running in the background.

| Command                 | Inline args | Description                   |
| ----------------------- | ----------- | ----------------------------- |
| `/ps`                   | —           | List background terminals     |
| `/stop` (alias: /clean) | —           | Stop all background terminals |

***

### Memory[​](#memory "Direct link to Memory")

Control how Wizard stores and uses memory across sessions.

| Command     | Inline args | Description                         |
| ----------- | ----------- | ----------------------------------- |
| `/memories` | —           | Configure memory use and generation |

***

### Session info[​](#session-info "Direct link to Session info")

Inspect the current session state without changing anything.

| Command         | Inline args | Description                                              |
| --------------- | ----------- | -------------------------------------------------------- |
| `/status`       | —           | Show current session configuration and token usage       |
| `/config`       | —           | View and manage configuration                            |
| `/debug-config` | —           | Show config layers and requirement sources for debugging |
| `/feedback`     | —           | Send feedback to maintainers                             |

***

### Exit[​](#exit "Direct link to Exit")

| Command | Inline args | Description |
| ------- | ----------- | ----------- |
| `/quit` | —           | Exit Wizard |
| `/exit` | —           | Exit Wizard |

***

### Realtime (experimental)[​](#realtime-experimental "Direct link to Realtime (experimental)")

| Command     | Inline args | Description                               |
| ----------- | ----------- | ----------------------------------------- |
| `/realtime` | —           | Toggle realtime voice mode (experimental) |
| `/settings` | —           | Configure realtime microphone/speaker     |

***
