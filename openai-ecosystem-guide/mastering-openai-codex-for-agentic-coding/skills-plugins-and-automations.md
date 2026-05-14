---
icon: openai
---

# Skills, Plugins, and Automations

> Explore how to extend OpenAI Codex with Skills, Plugins, and Automations that package repeatable workflows, connect external tools, and automate recurring tasks. Understand how to create and invoke skills, install plugins, and schedule automations to streamline development processes and reduce manual overhead.

Codex handles individual tasks well. Give it a prompt, and it reads the codebase, plans the work, writes the files, and reports back. That loop works for tasks we think of in the moment: fix this bug, add this endpoint, refactor this module.

But development work also includes workflows that do not change from run to run. Scanning for technical debt before a sprint. Generating a changelog before a release. Checking that every new file follows the project’s naming conventions. They are repeatable processes. Running them manually means writing the same multi-step prompt each time, hoping the phrasing stays consistent, and reviewing output that varies slightly depending on how we worded it that day.

The real cost is not the time spent on any single run. It is the compounding overhead of repeating the same prompt, the same review, the same manual invocation, week after week. Skills, Plugins, and Automations are the layer that eliminates this overhead.

* A Skill packages a repeatable workflow so it can be invoked by name.
* A Plugin bundles pre-built integrations so Codex can connect to external tools.
* An Automation schedules a skill to run on a set cadence without any input from us.

Together, they turn Codex from a capable assistant into a persistent part of the development workflow.

### What are Skills? <a href="#what-are-skills" id="what-are-skills"></a>

A **Skill** is a directory containing a `SKILL.md` file that teaches Codex how to perform a specific, repeatable workflow. Where `AGENTS.md` gives Codex rules that apply to every task, a skill packages a named, multi-step process that we invoke when we need it.

<figure><img src="../../.gitbook/assets/image (14).png" alt="" width="563"><figcaption></figcaption></figure>

Skills are built around a concept called progressive disclosure. Codex loads only the skill’s metadata at startup, which allows it to discover all available skills without loading their full content into the context window. The full `SKILL.md` is read only when the skill is actually invoked, and any scripts it references are run only when explicitly called. This keeps context usage low regardless of how many skills exist in the project.

A well-defined skill is a reusable unit of work with a name, a precise description, and a reliable outcome. Once written, it can be invoked repeatedly, shared with a team, and scheduled to run automatically.

#### Anatomy of a skill: The `SKILL.md` file <a href="#anatomy-of-a-skill-the-skillmd-file" id="anatomy-of-a-skill-the-skillmd-file"></a>

Every `SKILL.md` file has two parts:

* **YAML frontmatter:** The YAML frontmatter block is present at the top, containing two fields:
  * `name`: This is the unique identifier used to invoke the skill.
  * `description`: This is a one-line explanation of what the skill does and when it should be used. The description field is the most important line in the file. It determines whether Codex can recognise a matching task and invoke the skill automatically, without an explicit call from us. A vague description produces unreliable automatic invocation; a precise one that names the trigger condition works consistently.
* **The instructions body:** It is present below the frontmatter and contains the step-by-step workflow Codex follows when the skill runs. Each step should be specific enough that it cannot be misinterpreted.

```
---
name: generate-changelog
description: Generates a CHANGELOG.md entry from git commits since the last tag. Use when preparing a release or summarising recent changes for stakeholders.
---

## Instructions
1. Run `git log <last-tag>..HEAD --oneline` to get all commits since the last release.
2. Group commits by type: Features, Bug Fixes, Chores.
3. Write a new entry at the top of CHANGELOG.md using Keep a Changelog format.
4. Use today's date and the version passed as an argument, or prompt for it if not provided.
5. Leave the rest of CHANGELOG.md unchanged.

// The SKILL.md file
```

Once the structure is clear, the next decision is where to place the skill file.

#### Where skills live <a href="#where-skills-live" id="where-skills-live"></a>

Skills can be stored at two levels, and the choice determines who can use them.

* **Global skills:** `$HOME/.agents/skills/` makes a skill available in every project on the machine. This is the right location for personal productivity workflows that do not belong to any single codebase.
* **Repo skills:** `.agents/skills/` inside the repository root scopes the skill to that project. Repo skills are checked into source control and shared with the team. Any developer who clones the repository gets the full skill library automatically.

Skills in subdirectories are scanned recursively, so a monorepo can organise skills by service or package without any central registration. With the skill file in place, invoking it is straightforward.

#### Invoking skills <a href="#invoking-skills" id="invoking-skills"></a>

Skills support two invocation modes that serve different situations:

* **Explicit invocation** uses the `$skill-name` syntax in the thread composer or prompt. For example, typing `$generate-changelog` tells Codex to load and run that specific skill, regardless of how the prompt is phrased.
* **Implicit invocation** requires no special syntax. Codex reads the metadata of all available skills at startup and matches them against the current task description. When a prompt aligns with a skill’s description closely enough, Codex invokes the skill automatically. The quality of the `description` field determines how reliably this works: a description that names the trigger condition explicitly performs far better than one that vaguely describes the output.

For workflows we want to invoke predictably, explicit invocation is the safer choice. Implicit invocation becomes useful once the description is well-tuned and the skill’s output is proven consistent. Skills can also reference scripts for steps that benefit from precise, executable logic.

#### Skills and the scripts folder <a href="#skills-and-the-scripts-folder" id="skills-and-the-scripts-folder"></a>

A skill directory can include a `scripts/` subfolder containing shell scripts or Python scripts. The skill’s instructions can reference and run these scripts as steps in the workflow.

The decision of when to use scripts is straightforward. If a workflow step involves data processing that is easier to express in code than in natural language, it belongs in a script. Parsing structured output from a command, transforming a JSON payload, or applying a regex across a large file are all good candidates. Simple steps that read files or write text are better left as natural language instructions.

Skills and scripts together form a complete unit. The instructions define the workflow at a high level, and the scripts handle the steps that require precision.

### What are Plugins? <a href="#what-are-plugins" id="what-are-plugins"></a>

A **Plugin** is an installable package that bundles skills, app integrations, and MCP server configurations into a single distributable unit. Where a skill is something we author for our own workflows, a plugin is something we install when a pre-built integration already exists for a tool we want to connect.

Plugins are installed through the Plugin Directory, available in both the Codex app and the CLI. After installation, bundled skills are immediately available. App integrations may require authentication with the connected service before they can be used.

A plugin can contain three types of content:

* **Skills:** Ready-to-use workflow instructions that become available immediately after install.
* **App integrations:** Connections to external services such as GitHub, Slack, Jira, Linear, Figma, and Gmail that Codex can read from and act on as part of a task.
* **MCP servers:** Configurations for external tool connections that give Codex access to systems outside the local workspace.

Once installed, a plugin can be invoked using `@plugin-name` in the thread composer, or Codex can match it implicitly from the task description. The decision between building a skill and installing a plugin is clear. Install a plugin when an integration for the tool already exists where as build a skill when the workflow is specific to the project and no pre-built option covers it.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Plugins extend what Codex can reach. Automations extend when and how often it acts.

### What are Automations? <a href="#what-are-automations" id="what-are-automations"></a>

An **Automation** is a scheduled, recurring task that Codex runs in the background on a set cadence, without requiring a prompt from us at run time. Where a skill defines a workflow we invoke manually, an automation defines when that workflow runs on its own.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Automations in Codex</p></figcaption></figure>

For an automation to run, the Codex app must be open, and the project must be available on disk. Each automation executes in the project directory or a dedicated background worktree to isolate its changes from active sessions. Results appear in the Triage section of the Automations pane in the Codex app. Runs with findings stay in the inbox for review. Runs without findings are archived automatically, so the inbox surfaces only items that require attention. Automations become most powerful when combined with skills that have already proven reliable.

### Combining automations with skills <a href="#combining-automations-with-skills" id="combining-automations-with-skills"></a>

An automation invokes a skill by including `$skill-name` in the automation prompt. The skill defines the workflow; the automation defines the schedule. Keeping these two separate has a practical benefit: updating the skill updates every automation that invokes it, without touching any schedule configuration.

The key principle is sequencing. A workflow that still requires steering or produces inconsistent output is not ready to automate. The right order is:

1. Create the skill first
2. Run it manually until the output is reliable
3. Schedule it as an automation.

Automating an unreliable workflow only makes the problem recur on a fixed schedule.

### Choosing between the four tools <a href="#choosing-between-the-four-tools" id="choosing-between-the-four-tools"></a>

`AGENTS.md`, Skills, Plugins, and Automations are complementary layers, each serving a different purpose. Understanding which layer fits which situation makes it easy to build the right solution without over-engineering.

| **Tool**       | **Use When**                                                         |
| -------------- | -------------------------------------------------------------------- |
| `AGENTS.md`    | We want Codex to follow project rules on every task automatically.   |
| **Skill**      | We have a repeatable workflow to invoke by name or description.      |
| **Plugin**     | A pre-built integration exists for the tool we want to connect.      |
| **Automation** | A workflow is reliable enough to run on a schedule without a prompt. |

The natural build order follows the table from top to bottom. Write `AGENTS.md` first to establish project conventions. Create skills for workflows that repeat. Install plugins for external integrations. Automate the skills that prove consistent enough to run unattended.

<figure><img src="../../.gitbook/assets/image (17).png" alt="" width="462"><figcaption></figcaption></figure>

### Try it yourself <a href="#try-it-yourself" id="try-it-yourself"></a>

This exercise builds a `todo-tracker` skill from scratch and plans an automation for it. Open any existing project, or create a small one with a few source files that contain `TODO` and `FIXME` comments scattered through the code.

1. **Design the skill:** Write the `name` and `description` fields for a `todo-tracker` skill. Pay close attention to the description. It should be specific enough for Codex to invoke the skill automatically when someone asks to “audit technical debt” or “find all open TODOs.” A weak description will not trigger implicit invocation reliably.
2. **Write the instructions:** Write 4–5 instruction steps that tell Codex how to scan for `TODO` and `FIXME` comments, what to collect from each match (file path, line number, comment text), how to group the results, and where to write the output file.
3. **Plan the automation:** Write the automation prompt that invokes `$todo-tracker` on a weekly schedule. Decide what day and time makes sense for the workflow, and consider whether the automation should do anything beyond a plain scan, such as comparing against a previous run or filtering by comment age.
4. **Take it further:** Create the skill file at `.agents/skills/todo-tracker/SKILL.md` in the project, open Codex, and invoke it with `$todo-tracker`. Review the generated `TODOS.md` and refine the instructions based on what the output got wrong or missed.

Skills, Plugins, and Automations reward incremental investment. One well-written skill saves time on every subsequent run, and a well-tuned skill becomes the foundation for an automation that operates without any input at all. Start with the most frequently repeated workflow to understand how these improvements compound over time.
