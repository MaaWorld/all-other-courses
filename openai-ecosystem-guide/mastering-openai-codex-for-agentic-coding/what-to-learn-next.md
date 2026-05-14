---
icon: openai
---

# What to Learn Next

> Completing a course on agentic development is not just learning a new tool. It is a shift in how we think about writing software. From configuring a sandboxed agent to shipping a full Flask app without writing a single line manually, we have covered a lot of ground together. Congratulations!

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

### What we covered <a href="#what-we-covered" id="what-we-covered"></a>

Let's recap what we have learned:

* **What Codex is:** A cloud-based agentic coding tool that reads a repository, plans a task, executes it inside a sandbox, and reports back. No manual steering required at each step.
* **Setting up the three surfaces:** CLI, Desktop App, and IDE extension, all sharing the same `config.toml` for consistent behavior across every working context.
* **Teaching Codex with** `AGENTS.md`**:** A persistent instruction file that Codex reads before every task, covering project conventions, test commands, structure hints, and review guidelines.
* **Skills, Plugins, and Automations:** Packaging repeatable workflows as named skills, installing pre-built integrations as plugins, and scheduling reliable skills as automations that run without any prompt at run time.
* **MCP:** Connecting Codex to external tools and data sources via STDIO and HTTP servers, so a single task can reach GitHub issues, live documentation, or design files without manual copy-paste.
* **Team workflows:** Running parallel tasks in isolated Git worktrees, using subagents for concurrent work within a single thread, and connecting Codex to GitHub so it can respond to issues, review pull requests, and run in CI via `codex-action`.
* **Building the pixel art canvas app:** Planning and executing a complete Flask app across four Codex tasks, from scaffold to gallery and PNG export, with a test suite generated alongside the code throughout the build.

### Where Codex is heading <a href="#where-codex-is-heading" id="where-codex-is-heading"></a>

The platform is still evolving rapidly. Here are a few directions worth watching:

* **Longer autonomous task horizons:** Codex is progressively handling more complex, multi-step tasks with fewer interruptions. Tasks that currently need a few prompts may soon complete in a single delegation.
* **A growing ecosystem of Skills and Plugins:** The Skills and Plugin Directory is expanding. Pre-built integrations for tools like Linear, Slack, Jira, and Figma reduce the configuration work needed to connect Codex to existing workflows.
* **Deeper CI and GitHub integration:** The `codex-action` GitHub Action is still early. Scheduled automation runs, failure analysis, and automated fix PRs are the direction the GitHub integration is heading.
* **Codex as a component in multi-agent systems:** With `codex mcp-server`, Codex can already be exposed as an MCP tool for other agents. As orchestration frameworks mature, Codex will increasingly operate as a specialised coding component inside larger automated pipelines rather than as a standalone tool.

The official [OpenAI changelog](https://developers.openai.com/changelog) and the [Codex GitHub repository](https://github.com/openai/codex) are the most reliable places to follow what ships.

### Where to go from here <a href="#where-to-go-from-here" id="where-to-go-from-here"></a>

Codex is one tool in a broader ecosystem of AI coding assistants. Each one takes a different approach to the same problem of making software development faster and more automated. Exploring more than one of them builds a clearer picture of how agentic development is evolving across the industry.

* **Gemini Code Assist:** Google’s AI coding assistant is built into the IDE and deeply integrated with Google Cloud. Take this course to see how a different model family approaches context-aware code generation and how the workflow differs when AI assistance lives inside the editor rather than alongside it.
* **Claude Code:** Anthropic’s terminal-based agentic coding tool, built around long-context reasoning and a transparent task execution model. Take this course to compare a different philosophy of agentic coding, with a focus on auditability and reasoning depth over speed.
* **Cursor AI:** An AI-first code editor built on VS Code with deep codebase-wide context and powerful inline editing. Take this course to explore what AI assistance looks like when it is baked into the editor itself rather than layered on top as an extension.
* **Windsurf:** Codeium’s AI-powered IDE with a flow-based agent called Cascade that stays aware of developer actions in real time. Take this course to see how continuous editor-state awareness changes the dynamic between a developer and an AI agent during active coding sessions.
* **Building with OpenAI:** A hands-on course covering the OpenAI API, Assistants, tools, and the full platform powering Codex. Take this course to go deeper on the models and APIs behind what we used throughout this course and build AI-powered applications from scratch.

Codex is most useful when it becomes a genuine part of the workflow rather than something we reach for occasionally. The habits that matter most, such as writing clear `AGENTS.md` files, scoping tasks tightly, and reviewing output carefully, compound over time. The more deliberate the delegation, the more reliable the result.
