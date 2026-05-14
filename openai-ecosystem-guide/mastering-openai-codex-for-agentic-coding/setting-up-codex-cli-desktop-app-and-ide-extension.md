---
icon: openai
---

# Setting Up Codex: CLI, Desktop App, and IDE Extension



> Explore how to install and configure OpenAI Codex across its three primary interfaces: CLI, Desktop App, and IDE extension. Understand authentication methods, approval and sandbox modes, and how to manage configuration with config.toml to enable a controlled, effective coding agent workflow.

OpenAI Codex is accessible through three surfaces: the CLI, the Desktop App, and the IDE extension. All three connect to the same underlying execution engine and share the same configuration defaults set in `config.toml`. Choosing which surface to use depends on the working context:

* The CLI suits terminal-first workflows and automation.
* The Desktop App handles multiple parallel tasks through a visual interface.
* The IDE extension brings Codex tasks into the editor without switching context.

This lesson covers what each surface is, how to install it, and how to configure the controls that govern its behavior. Getting these in place is what turns Codex from an installed package into a reliable part of the development workflow.

### Prerequisites and access <a href="#prerequisites-and-access" id="prerequisites-and-access"></a>

Three things need to be in place before installing any Codex surface:

1. OpenAI API key, obtained from the OpenAI platform and billed per usage, or an active ChatGPT Plus, Pro, Team, or Enterprise subscription.
2. Node.js 22 or later is required to install the CLI on macOS and Linux.
3. Git is required for Codex to read the repository structure before executing any task.

### The Codex CLI <a href="#the-codex-cli" id="the-codex-cli"></a>

The **Codex CLI** (`@openai/codex`) is the most configurable surface for working with Codex. It is open-source, runs in any terminal environment, and supports both an interactive terminal UI for development work and a non-interactive exec mode for scripting and automation.

#### Installation <a href="#installation" id="installation"></a>

The CLI installs as a global `npm` package. On macOS, Homebrew is also an option. On Windows, the CLI is installed through `winget` or the Microsoft Store.

```
# macOS and Linux via npm
npm install -g @openai/codex

# macOS via Homebrew
brew install codex

# Windows via winget
winget install Codex -s msstore
```

Once installation is done, we can verify it with the command:

```
codex --version
```

After verification is done, we can start Codex CLI using thiscommand:

```
codex
```

#### Authentication <a href="#authentication" id="authentication"></a>

The CLI supports two authentication methods.

* **API key authentication:** This uses the `OPENAI_API_KEY` environment variable. Setting it in the shell profile means the CLI picks it up automatically for every request. This is the recommended approach for scripting, automation, and CI environments.

```
# Add to ~/.zshrc or ~/.bashrc
export OPENAI_API_KEY="your-key-here"
```

* **ChatGPT account authentication:** This uses an OAuth flow initiated with `codex login`. This is the default path when no API key is present and works with any ChatGPT Plus, Pro, Team, or Enterprise subscription.

Credentials from either method are cached at `~/.codex/auth.json`. For headless or server environments where a browser is unavailable, `codex login --device-auth` provides a device code flow that does not require one.

#### CLI commands <a href="#cli-commands" id="cli-commands"></a>

The CLI has two primary operating modes.

* **Interactive terminal user interface (TUI) mode:** This is launched by running `codex` in a project directory. It presents a terminal interface where we type a prompt, watch Codex work through the task, and approve or reject changes before they are applied.
* **Non-interactive exec mode:** This is used via `codex exec`, which runs a task entirely without the UI and is designed for scripts, CI pipelines, and any context where no human is at the keyboard.

Beyond these two modes, the CLI includes utility commands for authentication, session management, and configuration. Some of them are given in the table below:

| **Command**                                 | **What It Does**                                                                | **Example**                                              |
| ------------------------------------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------- |
| `codex`                                     | Launches the interactive TUI in the current project directory.                  | `codex`                                                  |
| `codex "<prompt>"`                          | Launches the TUI with an initial prompt pre-filled.                             | `codex "explain the auth module"`                        |
| `codex exec "<prompt>"`                     | Runs a task non-interactively without the TUI, suitable for scripts and CI.     | `codex exec "fix the failing test in test_auth.py"`      |
| `codex exec resume [SESSION_ID]`            | Resumes a previous non-interactive session; use --last for the most recent one. | `codex exec resume --last "now write the tests"`         |
| `codex resume`                              | Resumes the most recent interactive session.                                    | `codex resume`                                           |
| `codex login`                               | Runs the explicit login flow and creates a codex-login.log entry.               | `codex login`                                            |
| `codex mcp`                                 | Opens the interface for configuring Model Context Protocol servers.             | `codex mcp add context7 -- npx -y @upstash/context7-mcp` |
| `codex features list`                       | Lists all available feature flags and their current state.                      | `codex features list`                                    |
| `codex features enable / disable <feature>` | Enables or disables a named feature flag.                                       | `codex features enable web-search`                       |

These commands cover everything we run from the terminal before and outside of a session. Once a session is active, a separate set of commands called slash commands becomes available.

#### Slash commands <a href="#slash-commands" id="slash-commands"></a>

Slash commands are typed inside an active Codex session, either in the interactive TUI or in the IDE extension panel, and they control behavior within that session without exiting it. Some of the commads are given in the table below:

| **Slash Command** | **What It Does**                                                                                       | **Example**                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| `/model`          | Opens the model selector to switch models for the current session.                                     | Type `/model` and select `gpt-5.4-mini` from the list.                                |
| `/plan-mode`      | Toggles plan mode, where Codex proposes a plan and waits for approval before acting.                   | Type `/plan-mode` before a large refactor to review the plan first.                   |
| `/review`         | Triggers a diff review of all pending changes before they are applied.                                 | Type `/review` after Codex finishes writing to inspect the diff.                      |
| `/status`         | Shows the current session state, active sandbox mode, and approval policy.                             | Type `/status` to confirm which sandbox mode is active mid-session.                   |
| `/mcp`            | Opens the interface for managing Model Context Protocol server connections.                            | Type `/mcp` to check which servers are connected to the current session.              |
| `/feedback`       | Submits feedback on the current session directly from within the TUI.                                  | Type `/feedback` after an unexpected result to report it without leaving the session. |
| `/auto-context`   | Enables automatic context gathering so Codex selects relevant files without explicit @file references. | Type `/auto-context` at the start of a task spanning many files.                      |
| `/local`          | Switches the session to run tasks against the local working tree.                                      | Type `/local` to switch back from cloud mode during the same session.                 |
| `/cloud`          | Switches the session to run tasks in a configured cloud environment.                                   | Type `/cloud` to move a long-running task to the remote environment.                  |

While slash commands adjust behavior from inside an active session, flags give us control over how a session is configured before it begins.

#### Key flags <a href="#key-flags" id="key-flags"></a>

Flags are the primary way to adjust Codex’s behavior for a specific run without changing the persistent defaults in `config.toml`. They cover model selection, output formatting, directory access, and session control. All flags apply globally across commands, and passing them on the command line overrides the values set in `config.toml` for that run only.

| **Flags**                   | **Values**     | **Purpose**                                                                           | **Example**                                                    |
| --------------------------- | -------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| `--model, -m`               | model name     | Overrides the default model for this run.                                             | `codex -m gpt-5.4-mini "explain this function"`                |
| `--full-auto`               | (none)         | Convenience shortcut: sets workspace-write sandbox and on-request approvals together. | `codex --full-auto "refactor the payments module"`             |
| `--output-last-message, -o` | file path      | Writes the final assistant message to a file instead of stdout.                       | `codex exec -o summary.txt "summarise recent changes"`         |
| `--json`                    | (none)         | Outputs newline-delimited JSON events for parsing in scripts.                         | `codex exec --json "check for lint errors" \\\| jq '.message'` |
| `--add-dir`                 | directory path | Grants write access to an additional directory alongside the workspace.               | `codex --add-dir ../shared "update the shared types"`          |
| `--cd, -C`                  | directory path | Sets the working directory before processing begins.                                  | `codex --cd apps/api "fix the gateway timeout"`                |
| `--search`                  | (none)         | Enables live web search during the task.                                              | `codex --search "find the latest FastAPI lifespan syntax"`     |
| `-i`                        | image path     | Attaches an image file to the initial prompt.                                         | `codex -i mockup.png "implement this component"`               |

Two flags `--ask-for-approval` and `--sandbox` not listed here carry more weight than the rest. Each accepts a set of named values that directly determine how much autonomy Codex has over the codebase.

#### Approval and sandbox modes <a href="#approval-and-sandbox-modes" id="approval-and-sandbox-modes"></a>

Because Codex operates autonomously, it needs a clearly defined boundary around what it is allowed to do and when it must stop and check. Without these boundaries, an autonomous agent writing files and running commands could cause unintended changes that are difficult to reverse. Approval and sandbox modes are the two controls that set those boundaries, and they work as a pair.

* **Approval mode:** It controls when Codex stops and waits for our confirmation before taking an action. It governs the intent. Approval modes are set with the `--ask-for-approval` flag or the `approval_policy` field in `config.toml`:
  * `on-request`**:** Codex operates within the sandbox and prompts only when it would need to cross a boundary. This is the recommended default for everyday development work.
  * `untrusted`**:** Codex requests confirmation before any potentially destructive or non-trusted command, regardless of whether a sandbox boundary would be crossed.
  * `never`**:** Codex runs without any approval prompts. This is appropriate for CI pipelines and fully automated environments.
* **Sandbox mode:** It controls what Codex is physically allowed to access during a task. It governs the access. Sandbox modes are set with the `--sandbox` flag or the `sandbox_mode` field in `config.toml`:
  * `workspace-write`**:** The default mode. Codex can read and write files within the project workspace. Network access is disabled by default but can be enabled through configuration without switching to a less restrictive mode. The `.git`, `.agents`, and `.codex` directories are protected as read-only even within this mode.
  * `read-only`**:** Codex can read files but cannot write or execute commands. Useful for code review and analysis tasks that should not modify the codebase.
  * `danger-full-access`**:** Removes all sandbox restrictions, including network access. Should only be used when external system access is explicitly required for the task.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Both settings can be passed as command-line flags for a single run or persisted as defaults in `config.toml`. Together, they define exactly how much autonomy Codex has and what it is permitted to affect. Repeating these on every command works for one-off situations, but for consistent day-to-day work a better approach is to set them once as persistent defaults.

#### Configuring defaults: `Config.toml` file <a href="#configuring-defaults-configtoml-file" id="configuring-defaults-configtoml-file"></a>

Most of the time, we want the same model, the same approval policy, and the same sandbox mode for everything we do in a project or across all projects. That is what `config.toml` is for. Rather than repeating flags on every run, we set the defaults once and let the CLI, the Desktop App, and the IDE extension all pick them up automatically.

Codex reads configuration from two locations:

* The global config lives at `~/.codex/config.toml` and applies to every project on the machine.
* The project config lives at `.codex/config.toml` in the repository root and applies only to that repository, overriding global values where they conflict.

Project-level config is the right choice for repository-specific defaults that should not affect other projects. A minimal `config.toml` setting the three most commonly used defaults:

```
# ~/.codex/config.toml

model   = "gpt-5.4"          # model to use for all tasks
approval_policy = "on-request"        # on-request | untrusted | never
sandbox_mode    = "workspace-write"   # workspace-write | read-only | danger-full-access
```

For workflows that require different configurations in different contexts, Codex supports named profiles. A **profile** is a named section in `config.toml` that can be activated with the `--profile <name>` flag at runtime:

```
# A strict read-only review profile
[profiles.review]
approval_policy = "never"
sandbox_mode    = "read-only"
```

With the CLI fully configured, the Desktop App and IDE extension pick up those same `config.toml` defaults and layer their own interfaces on top for managing tasks visually and from inside the editor.

### The Codex Desktop app <a href="#the-codex-desktop-app" id="the-codex-desktop-app"></a>

The Codex Desktop App is available for macOS (Apple Silicon) and Windows (via the Microsoft Store). After downloading the app, authentication can be done through two methods:

* **ChatGPT account:** Sign in using an existing ChatGPT Plus, Pro, Team, or Enterprise account through the OAuth flow.
* **API key:** Sign in by directly entering an OpenAI API key.

<figure><img src="../../.gitbook/assets/image (11).png" alt="" width="563"><figcaption></figcaption></figure>

Once authenticated, the interface organizes Codex work around two core concepts: projects and threads.

#### Projects and threads in Codex <a href="#projects-and-threads-in-codex" id="projects-and-threads-in-codex"></a>

A **project** is a connection between a local repository and Codex where as a **thread** is a single task session within that project. Projects group all threads for a specific repository, making it easy to track parallel work and return to previous sessions. Each thread represents one prompt, one workspace, and one result. Threads can run in different execution modes depending on how isolated the work needs to be from the rest of the codebase:

* **Local:** Codex works directly in the project directory, using the current working tree.
* **Worktree:** Codex creates an isolated Git branch for the task, keeping parallel changes separate from the main working tree.
* **Cloud:** Codex runs the task remotely in a configured cloud environment.

The Desktop App includes built-in Git tools for reviewing diffs, adding inline comments, staging and reverting chunks, committing, pushing, and creating pull requests, making it possible to take a task from a prompt to a merged pull request without leaving the app. A review inbox surfaces the results of scheduled automations for inspection before they are applied. The Desktop App supports the subset of CLI commands, relevant to its interface, which includes `/plan-mode`, `/review`, `/status`, `/mcp`, and `/feedback`.

For teams who prefer staying inside the editor entirely, the IDE extension brings the same capabilities directly into the coding environment.

### The IDE extension <a href="#the-ide-extension" id="the-ide-extension"></a>

The Codex IDE extension is available for VS Code, Cursor, Windsurf, VS Code Insiders, and JetBrains IDEs including IntelliJ IDEA, PyCharm, Rider, and WebStorm. To get started:

* **Open the extension marketplace:** Search for `openai.chatgpt` in the editor’s extension panel and install it.
* **Sign in:** After installation, the extension prompts for sign-in. Use either a ChatGPT account (Plus, Pro, Team, or Enterprise) or an OpenAI API key, the same credentials used for the CLI.

<figure><img src="../../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

Once signed in, the extension adds a sidebar panel to the editor. Tasks are written in that panel with support for `@file` references to include specific files as context. Proposed changes appear as a standard inline diff inside the editor, using the same review interface as any other code change.

#### Approval modes <a href="#approval-modes" id="approval-modes"></a>

Because the extension runs Codex tasks directly inside the editor, it needs the same kind of boundary controls as the CLI. The extension exposes these through three named approval modes:

* **Chat:** Codex operates in read-only mode and makes suggestions without writing to files.
* **Agent:** The default mode. Codex writes files and executes commands, with approval prompts before each action.
* **Agent full access:** Codex runs without approval prompts.

A model selector and a reasoning effort control (low, medium, or high) are available below the chat input, making it possible to adjust both the model and its reasoning depth without leaving the editor. Like the Desktop App, the extension supports a subset of the slash commands covered in the CLI section: `/local`, `/cloud`, `/review`, `/auto-context`, and `/status`. The extension reads from the same `config.toml` as the CLI, so any defaults configured there apply in the editor automatically.

With all three surfaces installed and configured, Codex operates consistently across every working context using the same defaults. The settings in `config.toml` are shared by the CLI, the Desktop App, and the IDE extension, so configuring once is enough for all three. Understanding which surface fits which workflow, and which approval and sandbox settings match the task at hand, is what makes Codex a controlled and effective collaborator rather than simply an available one.
