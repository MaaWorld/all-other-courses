---
icon: openai
---

# Codex in the GitHub Workflow: Issues, Pull Requests, and CI

> Explore how to integrate OpenAI Codex with GitHub workflows by automating issue handling pull request reviews and continuous integration tasks. Understand how to configure Codex cloud environments enable code review features and use GitHub Actions for seamless automation. This lesson helps you improve development efficiency by delegating tasks directly from GitHub and managing Codex outputs effectively.

Development work is shaped by the tools a team shares: the issue tracker where bugs and features are logged, the pull request where code is proposed and reviewed, and the CI pipeline where quality gates run on every change. These are the surfaces where decisions about code happen and where most development friction accumulates.

Working with Codex locally still leaves a gap at each of these surfaces. A bug filed in GitHub is not automatically a Codex task. A pull request waiting for review is not automatically reviewed. A CI pipeline checking code quality still requires someone to configure and maintain the checks. A developer carries context manually from one tool to the next, opening a Codex session, writing a prompt, and returning the result to wherever the work lives.

The GitHub integration closes these gaps. Codex can receive tasks directly from issue comments, post code reviews on pull requests as a standard GitHub reviewer, and run in CI through a GitHub Action. The issue comment becomes the prompt. The PR comment becomes the trigger. The workflow file becomes the schedule. No local session required at any step.

### Prerequisites and setup <a href="#prerequisites-and-setup" id="prerequisites-and-setup"></a>

Two things must be in place before the GitHub integration is active:

1. Set up the [Codex cloud](https://developers.openai.com/codex/cloud).
2. Codex cloud environment must be configured for the repository. Cloud environments provide the isolated execution context Codex uses when triggered from GitHub. There is no local machine in the loop, so the task runs remotely in an OpenAI-managed container.
3. Code review must be enabled for the specific repository in [Codex settings](https://chatgpt.com/codex/cloud/settings/code-review). The settings page shows a list of connected repositories with a toggle for each. Enabling it for a repository is the only step required before `@codex` mentions in that repository become active.

<figure><img src="../../.gitbook/assets/image (22).png" alt="" width="563"><figcaption><p>Codex settings page showing the Code review toggle and the list of connected repositories</p></figcaption></figure>

With cloud configured and code review enabled, the integration is available at three distinct points in the shared workflow.

### `@codex` on issues <a href="#codex-on-issues" id="codex-on-issues"></a>

Mentioning `@codex` in a GitHub issue comment spins up a cloud task using the issue description and thread as context. Codex reads the issue, explores the relevant parts of the codebase, implements the requested fix or feature, and opens a draft pull request with its changes.

The draft PR includes a summary of what was changed and why, with a reference back to the original issue. Nothing else needs to happen locally. The issue comment is the prompt, and the draft PR is the output.

The quality of the output is shaped by the quality of the issue. An issue that includes a clear description of the problem, relevant file paths, reproduction steps, or the expected behaviour gives Codex the same context it would need from a direct prompt.

Consider a daily planner app that allows duplicate tasks to be added. We raise a GitHub issue and leave a comment to delegate the fix:

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption><p>Raising a Github issue and leaving a comment with @codex to resolve</p></figcaption></figure>

Codex picks up the task, locates the `add_todo` function in `app.py`, adds a duplicate check before the insert, and opens a draft PR with the fix.

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption><p>Codex resolve the issue</p></figcaption></figure>

Once the draft PR exists, it behaves like any other pull request. It can be reviewed, updated with follow-up `@codex` comments, or passed through the standard merge process. The next point where the integration adds value is the review itself.

### `@codex` on pull requests <a href="#codex-on-pull-requests" id="codex-on-pull-requests"></a>

In a pull request comment, what `@codex` does depends entirely on how the comment is written. Two behaviours cover the two most common situations.

#### Requesting a code review <a href="#requesting-a-code-review" id="requesting-a-code-review"></a>

Writing `@codex review` in a PR comment triggers a standard GitHub code review. Codex reads the diff, analyses the changes, and posts inline comments flagged by priority.

* P0 marks issues that should block the merge.
* P1 marks issues worth addressing but not blocking.

The review appears in the PR’s review thread exactly as a human reviewer’s would. The review scope can be narrowed with a one-line addition. For example `@codex review for missing tests` directs it toward test coverage gaps. The focus phrase travels with the comment and shapes what Codex looks for during that specific review.

Let’s try `@codex review for security regressions` on our repository:

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption><p>Asking codex for security review of PR</p></figcaption></figure>

We can see Codex focusing on the security-relevant patterns:

<figure><img src="../../.gitbook/assets/image (26).png" alt="" width="563"><figcaption><p>Codex security review on PR</p></figcaption></figure>

#### Delegating a follow-up task <a href="#delegating-a-follow-up-task" id="delegating-a-follow-up-task"></a>

Any `@codex` comment that does not begin with `review` starts a cloud task using the PR as context. This handles the work that arises during review: fixing a failing CI check, applying a change requested by a reviewer, adding a missing test for a specific route. The PR diff, description, and the full comment thread all become context for the task.

### Customising reviews and automatic reviews <a href="#customising-reviews-and-automatic-reviews" id="customising-reviews-and-automatic-reviews"></a>

By default, Codex applies its own judgment about what constitutes a P0 or P1 issue. Two mechanisms let us shape that judgement for the project.

* The first is the `## Review guidelines` section in `AGENTS.md`. When Codex reviews a pull request, it searches the repository for `AGENTS.md` files and follows any guidelines it finds under that heading. The items in the section become the review checklist. In multi-package repositories, Codex applies the closest `AGENTS.md` to each changed file, so different parts of the codebase can carry different review criteria without any central configuration. P0 and P1 are the only flag levels Codex uses in GitHub reviews by default. Adding custom categories requires explicit guidance:

```
## Review guidelines

- Flag any route missing authentication middleware as P0.
- Flag hardcoded configuration values as P1.
- Treat typos in documentation as P1.
```

* The second mechanism is automatic review. When enabled in Codex settings, Codex posts a review on every new pull request as soon as it is opened, without requiring an `@codex review` comment. This is the right setting for teams that want consistent coverage on every change rather than coverage that depends on someone remembering to trigger a review.

Comment-triggered review and automatic review both operate reactively, responding to activity in the PR. For teams that want Codex running as part of CI on every push, the GitHub Action provides a direct pipeline integration.

### The GitHub Action: `openai/codex-action@v1` <a href="#the-github-action-openaicodex-actionv1" id="the-github-action-openaicodex-actionv1"></a>

`openai/codex-action@v1` is a GitHub Action that runs Codex inside a CI/CD pipeline triggered by any GitHub workflow event: pull requests, pushes, scheduled runs, or manual dispatches. It installs the Codex CLI and runs `codex exec` under the permissions specified in the workflow file. No local machine is required. The key inputs are:

* `openai-api-key`**:** This is required. Store the key as a GitHub secret and reference it in the workflow.
* `prompt` or `prompt-file`**:** This contains the task instruction. For longer or reusable prompts, `prompt-file` points to a Markdown file committed in the repository. The convention is to store prompt files under `.github/codex/prompts/` so they are versioned alongside the workflow.
* `sandbox`**:** Match the sandbox mode to the task. `workspace-write` for tasks that need to read and edit files; `read-only` for review-only tasks.
* `output-file`**:** Saves the final Codex message to disk so later job steps can post it as a comment, upload it as an artifact, or compare it against a baseline.
* `safety-strategy`**:** Defaults to `drop-sudo`, which removes `sudo` access before Codex runs. Set to `unsafe` on Windows runners.

The `permissions` block on the job requires at minimum `contents: read` and `pull-requests: write` for a review workflow that posts results back to the PR.

The example below shows a two-job pattern: the first job runs Codex and captures its output, the second job posts the result as a PR comment.

```
# .github/workflows/codex-review.yml
name: Codex PR review

on:
  pull_request:
    types: [opened, synchronize, reopened]  # run on new PRs and updates

jobs:
  codex:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    outputs:
      final_message: ${{ steps.run_codex.outputs.final-message }}
    steps:
      - uses: actions/checkout@v4
        with:
          ref: refs/pull/${{ github.event.pull_request.number }}/merge  # check out the merge commit

      - name: Run Codex review
        id: run_codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt-file: .github/codex/prompts/review.md  # prompt file stored in the repo
          output-file: codex-output.md
          sandbox: workspace-write

  post_feedback:
    runs-on: ubuntu-latest
    needs: codex
    if: needs.codex.outputs.final_message != ''  # only post if Codex produced output
    steps:
      - name: Post Codex feedback as PR comment
        uses: actions/github-script@v7
        with:
          github-token: ${{ github.token }}
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.payload.pull_request.number,
              body: process.env.CODEX_FINAL_MESSAGE,
            });
        env:
          CODEX_FINAL_MESSAGE: ${{ needs.codex.outputs.final_message }}
```

The quality of the prompt file determines how useful the Action’s output is. A generic instruction returns a surface-level scan; a prompt that references the project’s `AGENTS.md` review guidelines and names the specific concerns to check returns findings that can be acted on directly.

### Reviewing Codex’s output <a href="#reviewing-codexs-output" id="reviewing-codexs-output"></a>

GitHub integration increases the volume of Codex output across the team without reducing the obligation to read each piece before it is merged. Issues produce draft PRs. Automatic review produces comments on every PR. The Action runs on every push. Each output is a draft until it has been reviewed by a human.

The checklist for reviewing Codex output is consistent regardless of how the output was generated:

* Is the logic correct for the task or issue described?
* Are there changes to files outside the stated scope of the task?
* Does the output follow the conventions defined in `AGENTS.md`?
* Are there security-sensitive patterns: hardcoded values, missing authentication, unvalidated input?

Automation makes Codex faster, not infallible. Consistent review is the habit that makes the speed gain sustainable and keeps the team in control of what reaches production.
