# Getting started with the terminal

[Back to guides](https://docs.getdbt.com/guides.md)

dbt

CLI

Beginner

Beginner

[Menu ]()



## What is the terminal?[​](#what-is-the-terminal "Direct link to What is the terminal?")

The terminal (also called the command line, shell, or CLI) is a text-based interface for running commands on your computer. Many dbt tools — including Fusion, dbt Core, and the dbt Wizard CLI — run from the terminal.

You don't need to be a terminal expert to use dbt. This guide covers the basics.

## Open the terminal[​](#open-the-terminal "Direct link to Open the terminal")

* macOS
* Windows
* Linux

**Option 1: Spotlight** Press `⌘ Space`, type `Terminal`, and press Enter.

**Option 2: Applications folder** Go to **Applications** → **Utilities** → **Terminal**.

**Option 3: VS Code integrated terminal** In VS Code, press `` ⌃` `` (Control + backtick) to open a terminal panel directly in your editor.

**Option 1: Command Prompt or PowerShell** Press `Win + R`, type `cmd` or `powershell`, and press Enter.

**Option 2: Windows Terminal** Search for "Terminal" in the Start menu. Windows Terminal supports Command Prompt, PowerShell, and WSL side by side.

**Option 3: VS Code integrated terminal** In VS Code, press `` Ctrl+` `` to open a terminal panel directly in your editor.

Using dbt on Windows?

dbt Core runs natively on Windows via PowerShell. For the best experience, consider using [WSL (Windows Subsystem for Linux)](https://learn.microsoft.com/en-us/windows/wsl/install).

Open your distribution's terminal emulator — usually found in the applications menu, or press `Ctrl + Alt + T` on most desktop environments.

## Navigate your file system[​](#navigate-your-file-system "Direct link to Navigate your file system")

When the terminal opens, you're in a directory (folder). These commands help you move around:

| Command       | What it does                         | Example                |
| ------------- | ------------------------------------ | ---------------------- |
| `pwd`         | Print the current directory          | `pwd` → `/Users/alice` |
| `ls`          | List files and folders (macOS/Linux) | `ls`                   |
| `dir`         | List files and folders (Windows)     | `dir`                  |
| `cd <folder>` | Change into a folder                 | `cd my-dbt-project`    |
| `cd ..`       | Go up one folder                     | `cd ..`                |
| `cd ~`        | Go to your home directory            | `cd ~`                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Tips:**

* Press **Tab** to autocomplete folder and file names — saves a lot of typing.
* Press **↑ / ↓** to scroll through previous commands.
* Press **Ctrl+C** to cancel a running command.

## Navigate to your dbt project[​](#navigate-to-your-dbt-project "Direct link to Navigate to your dbt project")

Your dbt project is a folder on your computer containing a `dbt_project.yml` file. Before running any dbt commands, navigate into that folder:

```bash
cd ~/path/to/your-dbt-project
```

Verify you're in the right place:

```bash
ls          # macOS/Linux — you should see dbt_project.yml
dir         # Windows
```

## Run your first dbt command[​](#run-your-first-dbt-command "Direct link to Run your first dbt command")

After installing dbt, you can run your first dbt command to verify it's installed and working. Make sure you're in your project folder before running any dbt commands:

```bash
cd ~/path/to/your-dbt-project
```

Then run your first dbt command:

```bash
dbt --version
```

You should see output similar to the following:

```bash
dbt-fusion 2.0.0-preview.45
```

.... and that's it! Congrats, you're ready to start using dbt in the terminal! 🎉

## Common issues[​](#common-issues "Direct link to Common issues")

**`command not found`** The tool isn't installed or isn't on your PATH. Double-check the install instructions for [dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md) or [dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md).

**`Permission denied`** You may need to run the command with elevated permissions, or check that the file is executable.

**Nothing happens after I type** Make sure you pressed **Enter** after the command.

## Next steps[​](#next-steps "Direct link to Next steps")

* [dbt Wizard quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md) — use the dbt Wizard CLI in your terminal
* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md) — set up dbt Fusion engine or dbt Core locally
* [dbt commands reference](https://docs.getdbt.com/reference/dbt-commands.md) — all available dbt CLI commands
