---
icon: claude
---

# Getting Started with Claude Code

> Explore the fundamentals of Claude Code, an AI-powered coding assistant that combines text-based reasoning with direct codebase interaction. Understand its architecture, benefits, and limitations to effectively integrate it into your development environment and workflows.

While new AI coding tools frequently make ambitious claims, the attention surrounding Claude Code stands out, and with good reason. Throughout this course, we will dissect the facts and logic behind that.

The core of that logic and the reason for the hype lies in its fundamental design. Before executing any commands, we need to be crystal clear on what a coding assistant is and, more importantly, what it isn’t. It is not an all-knowing engine that simply ‘writes code.’ Thinking of it that way fundamentally misrepresents the technology and limits what you can accomplish with it.

### What is a coding assistant? <a href="#what-is-a-coding-assistant" id="what-is-a-coding-assistant"></a>

A **coding assistant** is an AI-powered tool that helps developers write, understand, and modify code by combining reasoning (like a language model) with the ability to interact directly with a developer’s environment: reading files, running commands, and editing code. Unlike a standard language model, which only processes text, a coding assistant has “hands” and a “brain,” allowing it to participate directly in the coding workflow.

At its core, it’s a system designed to replicate the workflow of a human developer. Consider how you would solve a programming problem, such as fixing a bug from an error message. What are the actual steps you would take?

You’d likely follow a three-part process, outlined below.

1. **Understand:** First, examine the problem. Ask, “What does this error actually mean? Which files are involved?” Read the relevant parts of the codebase to get your bearings.
2. **Plan:** Next, evaluate and decide on a strategy based on the context. For instance, “Okay, I need to change this line in `main.py`, then run the test suite to see whether the fix worked.”
3. **Act:** Finally, execute the plan by typing the changes into the file, saving it, and running commands in your terminal.

Now, look closely at those three steps. The middle step, formulating a plan, is entirely cognitive; it depends entirely on your ability to think. A language model is adept at that. But the first and last steps, gathering context and taking action, require interacting with the world outside its text-based mind. You have to read files and run commands.

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="563"><figcaption></figcaption></figure>

Here we arrive at the central, logical problem. A language model performs a singular function by design: it processes input text and generates corresponding output text. That’s it; it cannot, on its own, read a file from your computer, nor can it run a command. If you ask a basic language model in a web chat, “Read the file `main.py` on my desktop,” it will correctly tell you it can’t. It functions like a brain in isolation: capable of processing and generating thought, yet incapable of physically interacting with the external world.

### How does Claude Code work? <a href="#how-does-claude-code-work" id="how-does-claude-code-work"></a>

**Claude Code** is built by Anthropic, the AI research company behind the Claude family of models. You’ll see the name “Claude” elsewhere (chat, API, etc.); Claude Code is the terminal-based, agentic coding experience that lets Claude read files, run commands, and edit code directly on your machine.

So, how does a coding assistant like Claude Code bridge this gap between a model’s ability to think in text and a developer’s need to act in the real coding environment? Language models can generate and reason about text, but they can’t, on their own, perform real-world actions.

The solution is a system called tool use. **Tool use** is an instruction layer between you and the model, acting as the model’s hands.

When you give a command to a properly equipped assistant, it doesn’t just pass your words to the model. It quietly adds a set of instructions, a hidden preamble that essentially says: “You are a helpful assistant. If you need to perform actions like listing or reading files, respond in a specific format, such as `ListFiles: .` or `ReadFile: path/to/file`.”

The model then uses these instructions to reason its way to the answer. As specified below, the loop is no longer a single step but a chain of logic.

1. You ask: “What is the database connection string for this project?”
2. The assistant adds instructions and sends your request to the model.
3. The model requests a tool. It thinks, “I can’t answer without seeing the project files. I should look for a configuration file,” and responds with `ListFiles: .`.
4. The assistant executes the command, gets a list of files (for example, `main.py`, `README.md`, `config.yaml`), and sends this list back to the model.
5. The model reasons again. It sees `config.yaml` and thinks, “That is the most likely place for a database string. Now I need to read its contents,” and responds with `ReadFile: config.yaml`.
6. The assistant executes the second command, reads the content of `config.yaml`, and sends it back to the model.
7. The model provides the final answer. Now equipped with the actual configuration data, it can locate the connection string and give you the precise answer.

<figure><img src="../../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

This loop is the secret. The model generates text-based requests for action, and the assistant program turns those requests into reality.

Claude Code supports multiple Claude models; availability depends on your plan and organization settings. The three models that can be used here are specified below.

* **Opus 4.6:** Most capable for deep planning and tough bugs; comes at the highest cost.
* **Sonnet 4.6:** Balanced in quality, speed, and cost; the sensible default for most learners.
* **Haiku 4.5:** Fastest for quick answers related to codebase.

You can check the current model with the `/status` command, switch models interactively with `/model`, or set a default via the `ANTHROPIC_MODEL` environment variable (project- or user-level). Note that some plans restrict access to Opus models.

### What are the advantages of using Claude Code? <a href="#what-are-the-advantages-of-using-claude-code" id="what-are-the-advantages-of-using-claude-code"></a>

An important thing to remember here is that not all language models are equally skilled at this process. Understanding what a tool is for, when to use it, and how to combine multiple tools to solve a coding problem is a sophisticated skill. The Claude series of models was engineered for high proficiency in tool use. A model that is merely good at generating text might fail at the multistep reasoning required for our database example. This distinction has three direct, practical benefits, outlined below.

1. **More dependable problem-solving:** A fluent model with tools can reliably chain them to solve complex problems. In our example, it correctly deduced that it first needed to list files, and then read a specific one. A less capable model might get stuck in a loop, repeatedly ask for the same file, or give up. Claude’s proficiency means it can handle more intricate, real-world programming tasks that require multiple steps and logical deduction. This makes it a more dependable partner.
2. **Adaptable to your environment:** As a developer, your workflow is unique. You use proprietary scripts, internal APIs, and custom command-line tools. A generic assistant is useless in that environment. Because Claude is designed to adapt, you can define your own tools, and it will learn to use them. This means you can integrate Claude Code directly into your existing processes, automating your specific, day-to-day tasks, not just generic ones. It transforms the assistant from a third-party utility into a core, integrated part of your personal toolkit.
3. **Privacy by design:** Many assistants require “indexing” your project, sending your entire codebase to a third-party server to be stored and analyzed. This is a nonstarter for any project involving sensitive data or proprietary intellectual property. Due to its strong tool use, Claude Code can navigate your codebase live and on demand. It accesses only the necessary data, exactly when required, directly from your local machine. This architectural difference means your code stays where it belongs, that is, with you.

This combination of capability, extensibility, and security is not a marginal improvement. It represents a different philosophical approach to AI-assisted development. Many popular tools, like Cursor or Windsurf, achieve their codebase awareness by ingesting and indexing your entire project into their environment. Claude Code, by contrast, operates more like a surgical instrument. Its reliance on superior tool use means it interacts with your code on your terms and machine without needing to absorb the whole project. This distinction is everything for the developer who values control and architectural integrity.

### What are the limitations of Claude Code? <a href="#what-are-the-limitations-of-claude-code" id="what-are-the-limitations-of-claude-code"></a>

Despite its advantages, Claude Code, like any other tool, has limitations common to current-generation AI systems.

* **No tool memory across sessions:** Custom tools are understood only while their definitions are in scope. To keep them available, load them from a persistent config at startup.
* **Context window limits:** The model only “sees” the text you provide (prompts, retrieved files, command outputs). Large repositories require scoping or iterative retrieval; don’t assume full-project awareness.
* **Non-determinism:** The same prompt may yield slightly different plans. Favor idempotent commands, clear constraints, and small, checkable steps.
* **Execution constraints:** Long-running or interactive commands (prompts, TTY menus, editors) can stall the loop. Prefer non-interactive flags, timeouts, and scripted flows.
* **Verification required:** The model can reason and propose edits, but still be wrong or overconfident. Keep tests, linters, and CI as the authoritative reference.
* **Security and permissions:** Any tool that can read/write files or run commands carries risk. Scope access carefully, require confirmations for destructive actions, and avoid exposing secrets in plain text.
* **Privacy boundaries:** Claude Code reads only what it needs, when it needs it, but any content you surface as context may be sent to the model per your organization’s data policies. Use environment files/secret managers, and avoid pasting secrets.

### What are the prerequisites for the course? <a href="#what-are-the-prerequisites-for-the-course" id="what-are-the-prerequisites-for-the-course"></a>

This course is designed for developers. We will assume you are already comfortable with the features mentioned below.

* **A programming language:** While examples will be general, a solid understanding of at least one language (like Python, JavaScript, or Go) is essential.
* **The command line:** Claude Code is a terminal-based tool. You should be comfortable navigating directories and running basic commands.
* **Version control with Git:** We will integrate with GitHub, so a working knowledge of Git is highly recommended.

With these skills, you are well prepared to get the most out of this course.

### What will we learn in this course? <a href="#what-will-we-learn-in-this-course" id="what-will-we-learn-in-this-course"></a>

Understanding the foundation of tool use is the first step. Throughout this course, you will build on this knowledge to gain complete control over your development environment. You will learn to familiarize yourself with several skills, mentioned below.

* **Context management:** How to precisely define the scope of Claude’s work.
* **Orchestration:** How to build custom commands and orchestrate multiple sub-agents for complex, multistep tasks.
* **Collaboration:** Integrating Claude Code directly with GitHub to streamline your team’s workflow.
* **Custom workflows:** How to use hooks, SDKs, and other integrations to embed Claude’s intelligence in your build and deployment pipelines.

Now that you understand the principle separating a simple chatbot from a true coding assistant, the next logical step is to set up your own.
