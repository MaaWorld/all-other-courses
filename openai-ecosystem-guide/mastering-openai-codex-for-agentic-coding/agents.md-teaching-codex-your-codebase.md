---
icon: openai
---

# AGENTS.md: Teaching Codex Your Codebase

> Explore how to write effective AGENTS.md files to give OpenAI Codex persistent knowledge about your project's conventions, test commands, and review guidelines. Understand the importance of specific instructions to maintain consistency and improve Codex's ability to deliver accurate, merge-ready code. Learn best practices for structuring AGENTS.md across global, repo, and subdirectory levels for scalable, automated agent workflows.

Two properties of Codex are relevant here: it can analyze a codebase and implement features, but it does not have inherent knowledge of the conventions that govern the codebase. When explicit instructions are absent, it relies on default assumptions. These defaults are often partially correct, but they may not align with the intended implementation.

Consider that we give Codex a prompt to add a new endpoint to an existing Python project. The code it returns works, and the tests pass. But on closer review, the following problems appear:

1. The new tests use `unittest` while the rest of the project uses `pytest`.
2. Logging is done with `print` statements while the project has a structured logger.
3. The variable names follow a slightly different pattern from every other file.

None of this is broken. All of it needs to be corrected before the code can be merged. The correction loop ends up taking longer than the original task, and it will repeat on every subsequent task unless we do something about it.

This is not a capability problem. It is an information problem. Codex made reasonable guesses because it had no project-specific instructions to go on. The solution is not a better prompt. A prompt is a one-time context. What Codex needs is persistent project knowledge: the rules, conventions, and preferences that apply to every task in this codebase. That is exactly what `AGENTS.md` provides.

### What `AGENTS.md` is <a href="#what-agentsmd-is" id="what-agentsmd-is"></a>

`AGENTS.md` is a Markdown file that Codex reads before executing any task. It is an instruction file written for the agent, not for human readers. Where a `README` explains a project to people who will work on it, `AGENTS.md` explains it to Codex: which tools to use, which conventions to follow, how to verify work, and what to avoid.

Codex loads the file at the start of every task, which means every prompt benefits from the same project context without us having to repeat it. Writing the test command once in `AGENTS.md` is enough for Codex to use the right test framework on every task in that project going forward.

<figure><img src="../../.gitbook/assets/image (12).png" alt="" width="310"><figcaption></figcaption></figure>

The closest analogy is the onboarding document we would hand a new engineer on their first day: here is the stack, here is how we test, here are the patterns we follow, and here is the legacy code to avoid copying. The difference is that Codex reads this document before every single task, not just once.

> **Fun fact: A growing cross-tool standard**

> `AGENTS.md` is supported by multiple agent tools, not just Codex. Tools that use their own instruction file format can simply point to it. A `CLAUDE.md` file containing only `Strictly follow the rules in ./AGENTS.md` redirects any Claude-based tool to the same shared file. One `AGENTS.md` governs all agents in the repo without duplication.

### How Codex resolves multiple `AGENTS.md` files <a href="#how-codex-resolves-multiple-agentsmd-files" id="how-codex-resolves-multiple-agentsmd-files"></a>

A single `AGENTS.md` at the repo root is enough for most projects. For larger codebases, Codex supports a layered system that allows different levels of instruction to coexist and override each other cleanly.

Codex builds the instruction chain once per run by loading files in this order:

* **Global:** `~/.codex/AGENTS.md` applies to every project on the machine. This is the right place for personal toolchain preferences and style defaults that apply everywhere.
* **Project root:** `<repo>/AGENTS.md` applies to the entire repository. This is where team conventions, test commands, project structure, and review guidelines belong.
* **Subdirectory:** `<repo>/services/api/AGENTS.md` applies only to tasks run in that specific directory. This is useful when one part of the codebase follows different rules from the rest.

Files are concatenated from the root down. Instructions closer to the current working directory take precedence over higher-level ones, so a subdirectory file can extend or override the repo-level file without modifying it.

At each level, Codex also checks for an `AGENTS.override.md` file. This is a temporary instruction file that, when placed at any level, replaces the regular `AGENTS.md` at that same level. If one exists, it is used instead of the regular file at that level. This is the right tool when we want to experiment with different instructions without touching the shared base file. Removing the override restores the original guidance.

<figure><img src="../../.gitbook/assets/image (13).png" alt="" width="318"><figcaption></figcaption></figure>

> There is one constraint about Codex worth knowing. It stops loading files once the combined size of all `AGENTS.md` files reaches 32 KiB. Files that are focused and non-repetitive ensure the most important instructions always make it through.

#### What to put in a global `AGENTS.md` <a href="#what-to-put-in-a-global-agentsmd" id="what-to-put-in-a-global-agentsmd"></a>

The global file is the right place for personal defaults that apply across every project: the language version, linter, formatter, and test runner we prefer everywhere. Style preferences that hold universally, such as docstring requirements or function length limits, also belong here. The file should stay short and stable. Most of the project-specific work happens at the repo level.

```
# Global agent instructions

## Toolchain
- Python 3.11+
- Use ruff for linting and black for formatting
- Use pytest for all tests

## Style
- Write docstrings for every public function
- Keep functions under 30 lines where possible
- Prefer explicit over implicit
```

The repo-level file is where the bulk of the value lives. It is worth building deliberately.

#### What to put in a repo-level `AGENTS.md` <a href="#what-to-put-in-a-repo-level-agentsmd" id="what-to-put-in-a-repo-level-agentsmd"></a>

A well-structured repo-level `AGENTS.md` covers six content areas, each serving a distinct purpose:

* **Set up and test commands:** The exact commands to install dependencies and run tests. Without a test command, Codex cannot verify its own work after making a change. This is the single highest-impact line in any repo-level `AGENTS.md`.
* **Dos and don’ts:** The project’s non-negotiable conventions. Which libraries to use, which patterns to follow, what to avoid. Specific beats vague: "use the logger from `app/logger.py`, never use `print`" is actionable; “use consistent logging” is not.
* **Project structure hints:** A short index of where key things live: the routes directory, the services layer, the models file. This saves Codex from re-exploring the codebase on every task and starts it where an experienced team member would start.
* **Concrete examples by file path:** Name real files that represent the right pattern, and real files that are legacy to avoid. "Follow the pattern in `app/routes/users.py`; avoid the style in `app/routes/admin.py`" is more effective than describing the pattern in prose. Codex mirrors real files more reliably than it follows abstract descriptions.
* **Safety and permissions:** Define which actions Codex can perform without confirmation, such as reading files and running single-file checks. Also specify which actions require approval, including installing packages, pushing to Git, and deleting files. This is separate from Codex’s built-in approval modes; it is guidance embedded directly in the instruction file itself.
* **Review guidelines:** A `## Review guidelines` section that Codex uses as its checklist when reviewing pull requests. Items should be specific and mechanical, not qualitative.

```
# Project instructions

## Setup
pip install -r requirements.txt

## Tests
pytest tests/ -v
# Single file: pytest tests/test_api.py -v

## Do
- Use the logger from app/logger.py for all logging
- Use enums from app/enums.py for status values
- Use the service layer in app/services/ for business logic

## Don't
- Do not use print statements for logging
- Do not fetch data directly inside route handlers
- Do not add new dependencies without approval

## Project structure
- Routes: app/routes/
- Services: app/services/
- Models: app/models.py

## Good examples
- Follow the pattern in app/routes/users.py
- Avoid the legacy style in app/routes/admin.py

## Safety and permissions
Allowed without asking: read files, run single-file tests, lint, type-check
Ask first: package installs, git push, deleting files, full project builds

## Review guidelines
- Flag any route missing authentication middleware
- Flag hardcoded values that should use constants or enums
- Flag any database query without an index on the filtered column
```

#### What to put in a subdirectory `AGENTS.md` <a href="#what-to-put-in-a-subdirectory-agentsmd" id="what-to-put-in-a-subdirectory-agentsmd"></a>

Large repositories often contain components that have evolved independently: a frontend package on one framework version alongside a backend on another, or a payments service with stricter security requirements than the rest of the API. A single root-level file cannot cover these differences without becoming unwieldy.

Subdirectory `AGENTS.md` files solve this cleanly. Each component carries its own rules. The instruction chain merges them at runtime, so Codex applies the conventions appropriate to the specific directory it is working in, without any manual context management from us.

### Practical tips for effective instructions <a href="#practical-tips-for-effective-instructions" id="practical-tips-for-effective-instructions"></a>

The most common mistake when writing `AGENTS.md` for the first time is trying to make it comprehensive before running any prompts. A more effective approach is iterative: run a task, note what the output got wrong, and add one rule. The right time to add a rule is the second time the same mistake appears, not the first.

Three other practices consistently produce better output:

* **Use file-scoped commands:** Define lint, type-check, and test commands that operate on a single file path rather than the full project. A per-file type check completes in seconds; a full build can take minutes. When verification is fast and cheap, Codex can run it after every change, which means correctness is checked continuously rather than only at the end.
* **Add a “when stuck” instruction:** Tell Codex to ask a clarifying question or propose a short plan before making large speculative changes. This costs one exchange and prevents a full wrong-direction run.
* **Keep files short:** The 32 KiB size limit means a bloated global file can crowd out the repo-level rules that matter most. Instructions that Codex already follows by default do not need to be written down.

> **The 32 KiB limit has a real consequence:**

> Codex loads `AGENTS.md` files in order and stops once the combined size reaches 32 KiB. A global file full of general programming advice that Codex already applies by default will consume space that could have held the specific project rules that actually change output. Shorter files are not just cleaner; they ensure the right instructions reach the model.

### `AGENTS.md` and GitHub PR review <a href="#agentsmd-and-github-pr-review" id="agentsmd-and-github-pr-review"></a>

When Codex reviews a pull request, either through a direct `@codex` mention in a PR comment or through an auto-review configuration, it searches the repo’s `AGENTS.md` for a `## Review guidelines` section. The items listed there become its review checklist. Review comments are flagged by priority:

* P0 marks issues that should block the merge.
* P1 marks issues that are worth addressing but are not blocking.

The checklist works best when each item is specific and verifiable: "flag any route missing `login_required`" gives Codex something concrete to check; “review for security issues” does not. The quality of the review is directly proportional to the quality of the guidelines written in the file.

### Common mistakes to avoid <a href="#common-mistakes-to-avoid" id="common-mistakes-to-avoid"></a>

Writing an effective `AGENTS.md` is straightforward, but a few patterns consistently produce poor results. Knowing what to avoid is as useful as knowing what to include.

* **Being too vague:** Rules like “write clean code” or “use good patterns” give Codex nothing to act on. Every rule should be specific enough to produce a different outcome on a real task.
* **Repeating what Codex already knows:** There is no need to describe Python syntax, general testing principles, or standard software practices. `AGENTS.md` is for project-specific rules that cannot be inferred from the code itself.
* **Omitting the test command:** The most impactful single addition to any repo-level `AGENTS.md`. Without it, Codex has no way to verify its own work.
* **Introducing conflicting instructions:** Adding new rules without reviewing the file as a whole can create contradictions. Contradictions produce unpredictable output.
* **Over-engineering upfront:** A focused two-line file that includes the test command and one project-specific convention is more valuable than a comprehensive file written before running a single prompt. Start small and iterate from real output.

A well-written `AGENTS.md` does not make Codex smarter: it makes Codex informed. The gap between capable and useful closes significantly once the agent knows the project’s test framework, its logging conventions, and the files worth using as examples. That knowledge lives in a file, and the file takes less time to write than correcting the output without it.
