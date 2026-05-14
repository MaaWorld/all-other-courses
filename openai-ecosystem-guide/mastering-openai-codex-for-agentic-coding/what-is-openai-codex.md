---
icon: openai
---

# What Is OpenAI Codex?

> Explore OpenAI Codex's unique agentic coding capabilities and how it differs from other AI tools by autonomously executing tasks in a sandboxed environment. Learn how Codex reads repositories, plans and implements changes, runs tests, and reports results, enabling you to delegate development work confidently with a clear review process.

The AI coding tool space has grown rapidly. Inline completion tools suggest the next line as we type. Chat-based assistants answer questions about our code and generate snippets on request. IDE extensions surface context-aware suggestions without us ever leaving the editor. Each of these approaches solves a real problem, and most working developers use at least one of them today.

OpenAI Codex takes a different position in this space. It does not complete our line or answer our question and waits for us to act on the response. Codex accepts a task description and executes it, reading the codebase, writing the files, running the tests, and reporting back what it did. The interaction model is closer to delegating work to a capable collaborator than to prompting a language model and applying the output ourselves. To see what this looks like in practice, consider two developers working on the same task.

### Scenario: From chatbox to shipped feature <a href="#scenario-from-chatbox-to-shipped-feature" id="scenario-from-chatbox-to-shipped-feature"></a>

Consider two developers tasked with adding a POST `/preferences` endpoint to a Flask application that validates and stores user preference data.

The first developer uses ChatGPT. They receive a route handler and validation logic, paste it into `app.py`, and update `models.py`, run the tests, find two failures caused by a naming conflict, return to ChatGPT for a fix, and apply it again. The code eventually works, but the developer was the bridge between the AI and the codebase at every step, reading the output, deciding where it belongs, applying it, and verifying the result.

The second developer opens Codex and writes: _Add a POST /preferences endpoint that validates and stores user preferences. Use the existing User model. Write a pytest test for it._ Codex reads the repository, writes the route, updates the model, generates the test, runs the suite, and reports back with a summary of every change and confirmation that all tests pass. The developer reviews the diff and approves it.

The difference is not in code quality. Both tools can produce correct, well-structured code. The difference is where the execution work sits. With ChatGPT, the developer owns the loop. With Codex, the developer owns the review. To understand what makes this architecture possible, it helps to know where Codex came from.

### The origin of OpenAI Codex <a href="#the-origin-of-openai-codex" id="the-origin-of-openai-codex"></a>

In 2021, OpenAI released a code-completion model also called Codex, a fine-tuned version of GPT-3 trained on billions of lines of publicly available code. That model was the engine behind the original GitHub Copilot, providing the inline completions that many developers came to rely on for finishing functions, generating boilerplate, and filling in repetitive patterns.

As language models advanced in reasoning capacity and context length, OpenAI shifted focus from token-level completion to task-level execution. The result, launched in 2025, is the Codex we have today: a purpose-built agentic coding platform backed by a dedicated line of models that has continued to evolve since launch.

The model family has progressed through successive generations, each improving on the previous in reasoning depth, long-horizon task handling, and code quality:

* `gpt-5-codex`**:** The first purpose-built agentic coding model, tuned for long-running tasks in the Codex environment.
* `gpt-5.1-codex` **and** `gpt-5.1-codex-max`**:** Improved long-horizon reasoning and stronger handling of complex, multi-file work.
* `gpt-5.2-codex`**:** Advanced capabilities for real-world software engineering tasks.
* `gpt-5.3-codex`**:** The industry-leading coding model for complex software engineering; its capabilities now power `gpt-5.4`.
* `gpt-5.3-codex-spark`**:** A research preview model optimized for near-instant, real-time coding iteration.
* `gpt-5.4` and `gpt-5.4-mini`**:** The current recommended models; `gpt-5.4` integrates the coding capabilities of `gpt-5.3-codex` with stronger general reasoning and tool use, while `gpt-5.4-mini` is a fast, efficient option for lighter tasks and subagent workflows.

`gpt-5.4` is the recommended starting point for most Codex work today. That progression from token completion to task ownership shapes how Codex is defined as a product.

### What is OpenAI Codex? <a href="#what-is-openai-codex" id="what-is-openai-codex"></a>

**OpenAI Codex** is a cloud-based, agentic software engineering tool that reads a codebase, plans multi-step work, writes and edits files, runs terminal commands, and produces verifiable outputs inside an isolated sandbox.

Unlike a code generator that produces a snippet and stops, Codex operates as a software engineering agent. It reads the project structure, plans what needs to change, makes those changes across as many files as the task requires, and runs the verification steps that confirm the work is correct. Everything happens inside a controlled sandbox, so we can review exactly what Codex did before any of it affects the wider codebase. The output is not code to paste; it is a completed, tested piece of work ready for our review. The definition tells us what Codex does as a product; understanding how it executes a task safely is the next piece.

### How Codex works: The sandboxed agent model <a href="#how-codex-works-the-sandboxed-agent-model" id="how-codex-works-the-sandboxed-agent-model"></a>

Every Codex task runs inside an isolated sandbox. By default, the network is disabled, and file system access is scoped to the project workspace. This means Codex cannot make outbound network calls, access files outside the project directory, or interact with external systems while it executes. These are not arbitrary restrictions. They are what make it safe to delegate work autonomously. The sandbox draws a clear boundary around what Codex can touch.

When we give Codex a task, it follows a consistent lifecycle:

* **Receive the prompt:** Codex reads the task description along with any context we provide, such as the specific files to focus on or constraints to respect.
* **Read the repository:** It examines the relevant files, directory structure, and configuration to build an understanding of how the codebase is organized and what conventions it follows.
* **Plan the work:** It reasons about which changes are needed, in which files, and in what order.
* **Execute:** It writes and edits files, runs shell commands, executes tests, and observes the results of each action.
* **Report:** It summarizes what it changed, what it ran, and what the outcome was, presenting the full diff for our review before anything is applied.

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="455"><figcaption><p>Codex task lifecycle</p></figcaption></figure>

This lifecycle is what separates Codex from a code generator. A code generator produces output and stops. Codex produces an outcome, confirms that it works, and hands it back for human review. That execution model is also what distinguishes Codex from other tools in the OpenAI platform, most notably ChatGPT.

### Codex vs. ChatGPT: Same platform, different paradigm <a href="#codex-vs-chatgpt-same-platform-different-paradigm" id="codex-vs-chatgpt-same-platform-different-paradigm"></a>

Both Codex and ChatGPT run under the same OpenAI account and appear within the same platform. For developers who use ChatGPT extensively for coding questions, the distinction between the two tools can feel unclear at first. They are genuinely different products built for different purposes.

**ChatGPT** is a conversational assistant. Each exchange is self-contained: we write a prompt, it generates a response, and we decide what to do with that response. It has no persistent connection to our codebase, no ability to run code inside our repository, and no mechanism for verifying that what it generates actually works in our specific project context. Applying the output, integrating it with existing logic, running tests, and fixing what breaks all belong to us.

**OpenAI Codex** is an execution environment. We give it a task that targets our specific codebase, and it operates directly inside that repository: reading files, applying changes, running commands, and producing a result that we can review before accepting. The conversation, if any, is secondary. The primary output is work done.

| **Features**           | **ChatGPT**                                                        | **Codex**                                                                   |
| ---------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| Interaction model      | Conversational, single-turn                                        | Agentic, multi-step                                                         |
| Codebase access        | None; works only from what we provide                              | Reads the full repository                                                   |
| Output                 | A text response containing generated code                          | File changes applied inside a sandbox                                       |
| Verification           | None; we verify manually                                           | Runs tests and commands to verify its own work                              |
| Who applies the result | The developer                                                      | Codex, with our review before acceptance                                    |
| Best for               | Exploring ideas, explaining concepts, generating isolated snippets | Implementing features, fixing bugs, writing tests, refactoring across files |

The practical guide is straightforward. When we want to think through a problem, understand an unfamiliar pattern, or quickly generate a self-contained snippet, ChatGPT is the right tool. When we have a concrete, bounded task to execute inside a real codebase, Codex is the right tool. The two work well together: ChatGPT for exploration and ideation, and Codex for execution. With that distinction clear, the next practical question is where we actually access and work with Codex.

### The three surfaces <a href="#the-three-surfaces" id="the-three-surfaces"></a>

Codex is accessible through three interfaces, each suited to a different working context.

* **The Codex CLI** **(**`@openai/codex`**):** It runs in the terminal. It is open-source, highly configurable, and the most direct way to work with Codex from the command line. The CLI presents an interactive terminal UI where we can read file changes, run approvals, and control exactly what Codex applies to our project. It also supports a non-interactive exec mode called `codex exec` for running tasks inside scripts and automated pipelines without a human at the keyboard.
* **The Codex Desktop App:** It provides a graphical interface for managing Codex work organized as projects and threads. A **thread** is a single task session: one prompt, one workspace, one output, whereas a **project** is a collection of threads tied to a specific repository. The Desktop App includes a review inbox where Codex reports the results of scheduled automations for us to inspect and approve before anything is merged or applied.
* **The IDE extension:** It is available for many IDEs, including VS Code, Cursor, Windsurf, etc. It brings Codex tasks into the editor sidebar, allowing us to delegate work without leaving the editor and to review diffs inline next to the code being changed. Changes proposed by Codex appear as a standard diff inside the editor, using the same review interface we already use for any code change.

<figure><img src="../../.gitbook/assets/image (8).png" alt="" width="424"><figcaption></figcaption></figure>

All three interfaces connect to the same underlying Codex execution engine. The choice of which surface to use is a matter of working context: the CLI for terminal-first workflows and automation, the Desktop App for managing multiple parallel tasks, and the IDE extension for staying inside the editor. Regardless of which surface we use, the boundaries of what Codex can and cannot do by default remain consistent.

### What Codex can and cannot do today <a href="#what-codex-can-and-cannot-do-today" id="what-codex-can-and-cannot-do-today"></a>

Understanding those boundaries shapes how we use Codex effectively.

Codex can read and write files across an entire repository, run shell commands and test suites, autonomously fix failing tests, open pull requests with a summary of the changes made, review pull request diffs against a defined set of guidelines, and execute recurring tasks on a schedule through the automation system. These capabilities make it well-suited to any task that is well-defined, bounded by a clear objective, and verifiable through a test suite or observable output.

By default, Codex cannot access the internet, call external APIs, push changes to a remote repository without our approval, or affect anything outside the sandboxed workspace. These restrictions are intentional and represent the default configuration. They exist because safe defaults matter more than permissive ones. Some of these restrictions can be adjusted through configuration when a specific task genuinely requires it.

One framing that helps calibrate expectations: Codex performs at the level of a strong, task-focused engineer on well-scoped work. It requires clear instructions, a codebase it can read to understand the project conventions, and ideally a test suite it can run to verify its own changes. The clearer and more bounded the task, the better the result. Open-ended or vague tasks produce inconsistent outputs. And regardless of how well a task executes, reviewing Codex’s changes before accepting them is a non-negotiable part of using it responsibly.

> **Scope your tasks:**\
> Tasks with a single, clear objective and a defined verification step consistently produce reliable results. Examples include “add this route,” “fix this failing test,” or “refactor this function to use the new interface.” Tasks defined as broad goals without clear success criteria are unreliable and should be split into smaller, concrete subtasks before delegation.

Agentic coding tools change how software is developed, and OpenAI Codex is one of the more mature implementations in practice. The execution model, the sandbox, and the review workflow are the foundations on which everything else builds. Getting those fundamentals right is what makes the difference between using Codex as a faster way to generate code and using it as a genuine engineering collaborator.
