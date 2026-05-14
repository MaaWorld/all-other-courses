---
icon: claude
---

# Installing and Launching Claude Code

> Explore the steps to install and launch Claude Code, ensuring your system meets requirements, setting up authentication, and configuring the environment. By the end, you'll have the tool ready on your machine, enabling you to begin AI-driven coding with confidence and security

We’ve established the core principle of tool use that gives Claude Code its power. Now it’s time to get that power working on your machine. By the end of this lesson, you’ll have Claude Code installed, authenticated, and running.

This process is straightforward, but precision is key. We’ll walk through it step-by-step.

### How to install Claude Code <a href="#how-to-install-claude-code" id="how-to-install-claude-code"></a>

Before installing anything, verify that your system is ready. Skipping this quick check can turn minor issues into major headaches.

First, ensure the environment meets these minimum requirements mentioned below.

* **Operating system:** macOS 10.15 or later, a modern Linux distribution. Another option can also be Windows 10 or later, with WSL or Git Bash.
* **Hardware:** At least 4 GB of RAM.

Next, the most important software dependency is Node.js. Let’s check if you have the correct version. Open your terminal and run this command:

```
node -v
```

If you see a version like `v18.0.0` or later, you’re good to go. If you see an error or an older version, install or update Node.js from the official website before proceeding.

```
npm install -g @anthropic-ai/claude-code
```

Now, note a critical point. If the command fails with a permission error, do not prefix it with `sudo`. Using `sudo` with `npm install -g` can create permission problems and security risks. Instead, use a Node version manager such as nvm, or configure npm to use a different directory.

### How to use Claude Code <a href="#how-to-use-claude-code" id="how-to-use-claude-code"></a>

With the installation complete, it’s time to run the tool for the first time. In any existing project directory, run:

```
claude
```

On first run, Claude Code asks you to authenticate. Choose one of the paths mentioned below:

* **Claude.ai subscription:** If you have a Claude Pro or Max subscription, log in with your [Claude.ai](https://design-for-me.devpath.com/courses/claude-code/Claude.ai) account.
* **Anthropic Console:** This common option opens a browser window for you to log in with your Anthropic account. It requires active billing in the Anthropic Console.
* **3rd-party platform:** Authenticate via Amazon Bedrock, Microsoft Foundry, or Vertex AI.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Congratulations! You’ve installed Claude Code, completed the initial authentication, and configured Educative’s environment variable setup. Although this took some work, you’ve removed authentication friction for the rest of the course.

Starting with the next lesson, Claude Code will start with a single command, so you can focus on learning AI-powered development instead of wrestling with setup. You now have one of the most powerful AI coding tools at your fingertips, preconfigured and ready to support your software development.

In the remainder of this course, we’ll engage in real coding sessions with Claude Code and explore the fundamental patterns that make AI-assisted development effective.
