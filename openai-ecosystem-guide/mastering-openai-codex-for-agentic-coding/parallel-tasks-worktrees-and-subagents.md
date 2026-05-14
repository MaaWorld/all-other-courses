---
icon: openai
---

# Parallel Tasks, Worktrees, and Subagents

> Explore how to increase development efficiency by running parallel tasks in OpenAI Codex. Understand the role of projects, threads, and Git worktrees for isolating work, and learn to use the Handoff feature to move tasks between local and background execution. Discover how subagents enable concurrent work within a single task to handle complex workflows more effectively.

A typical development session follows a familiar rhythm: one task runs to completion, then the next begins. When we introduce an agent capable of executing work autonomously, that rhythm no longer has to be sequential. The bottleneck in a sequential model is not the quality of the work. It is the waiting. While one task runs, everything else stands still.

Codex can run multiple tasks simultaneously, but the benefit only materializes when we change how we scope and assign work. Running two parallel tasks on the same files invites conflicts. Running two parallel tasks on non-overlapping scopes is safe, fast, and the pattern Codex is designed for. The mental model shift is not about managing more work. It is about thinking in independently bounded units before any task starts.

<figure><img src="../../.gitbook/assets/image (19).png" alt="" width="399"><figcaption></figcaption></figure>

This lesson covers the mechanisms Codex provides for parallel execution: threads and projects as the fundamental containers, Git worktrees as the isolation layer, the Handoff feature for moving work between foreground and background, and subagents for complex in-task parallelism.

### Threads and projects <a href="#threads-and-projects" id="threads-and-projects"></a>

**A project** connects a local repository to Codex and groups all threads for that repo in one place. Every time we open a project, we see the full history of tasks we have delegated to it: completed threads, running threads, and threads awaiting review.

**A thread** is a single task session: one prompt, one workspace, one output. Threads within a project run independently of each other. A project supports up to six concurrent open threads by default, a limit controlled by the `agents.max_threads` configuration key, which defaults to `6`.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Cloud threads add another dimension to this model. They can run in the background without an active local session. When the local machine is offline, or the Codex app is closed, cloud threads continue in a remote environment and post their results when we return.

The thread and project model gives us a way to track parallel work without losing context. Each thread carries its own task history, and the project groups them so we can see the full picture of everything Codex is working on in a given repository. Isolation between threads, however, requires a mechanism at the file system level, which is where Git worktrees come in.

### Git worktrees and task isolation <a href="#git-worktrees-and-task-isolation" id="git-worktrees-and-task-isolation"></a>

Without isolation, two tasks on the same repository can overwrite each other’s file changes. The second task to touch a file wins, and the first task’s work is lost. For sequential work, this is not a concern, but for parallel work, it is the first problem to solve.

A **Git worktree** is an additional working directory linked to the same repository but checked out on its own branch. Changes made in one worktree do not affect any other. Each worktree has its own copy of every file, while all worktrees share the same underlying `.git` metadata about commits, branches, and history.

Codex creates worktrees in `$CODEX_HOME/worktrees` in a [detached HEAD](https://git-scm.com/docs/git-checkout#_detached_head) state, which allows multiple worktrees to coexist without branches colliding. Two worktree types serve different lifecycle needs.

* **Codex-managed worktrees:** They are lightweight and disposable. By default, each one is dedicated to a single thread. Codex keeps the most recent 15 Codex-managed worktrees and deletes older ones to manage disk space. Before deleting a worktree, Codex saves a snapshot of the work on it. If we reopen a thread whose worktree was deleted, the option to restore it appears automatically.
* **Permanent worktrees:** These are user-created and not auto-deleted. They are suited for shared, long-lived environments where multiple threads may use the same setup over time. Creating one from the project sidebar in the Codex app turns it into its own project entry with persistent state.

Both worktree types can be used interchangeably through the Handoff feature, which manages the transition between active and background work.

### The Handoff feature <a href="#the-handoff-feature" id="the-handoff-feature"></a>

**Handoff** moves a thread between Local mode, which runs tasks directly in the main project directory, and Worktree mode, which runs the same thread inside an isolated branch. Codex handles the underlying Git operations required to move work safely between the two environments. The practical use case is sequencing:

* Start a task locally for interactive iteration where we want to monitor progress closely.
* Then hand it off to a worktree once the direction is clear.
* The main directory becomes free for other work while Codex continues autonomously in the background.

Running the flow in the opposite direction is equally supported: if a thread is already running in a worktree and we want to bring it back for close review, Handoff moves it into Local.

Git enforces that the same branch can only be checked out in one place at a time. Handoff accounts for this constraint by managing branch state automatically during the transfer. If a thread is handed back to a worktree later, Codex returns it to the same worktree it used before, preserving all the work already done there.

With isolation and handoff covered, parallel execution becomes a matter of scoping work correctly before tasks begin.

### Running tasks in parallel: Scoping for independence <a href="#running-tasks-in-parallel-scoping-for-independence" id="running-tasks-in-parallel-scoping-for-independence"></a>

The rule for safe parallel work is straightforward. Each thread must operate on a non-overlapping set of files. When two threads edit the same file simultaneously, one overwrites the other’s changes, and the work is lost.

Scoping for independence means defining each task around a clearly bounded objective and a defined set of files before starting it. A task is well-scoped when its affected files can be named in advance, and none of them appear in another active thread.

A concrete example shows how this looks in practice. Consider three parallel threads running on the same Flask project:

* Thread 1 adds a `/export` API route in `app/routes/export.py`.
* Thread 2 writes pytest tests for existing routes in `tests/test_routes.py`.
* Thread 3 updates the project documentation in `README.md`.

All three run simultaneously. The file scopes do not overlap. No thread will overwrite another’s work. The pattern to avoid is two threads both described as “refactor the service layer” with no file boundary specified. Both will explore `app/services/`, edit the same files, and produce results that cannot be merged cleanly.

Threads handle task-level parallelism. For tasks that are themselves internally complex, such as large codebase explorations or multi-module analyses, subagents provide a second layer of concurrent work within a single thread.

### Subagents: Parallel work within a single task <a href="#subagents-parallel-work-within-a-single-task" id="subagents-parallel-work-within-a-single-task"></a>

Subagents are specialised child agents spawned within a single thread to handle distinct portions of a large task concurrently. Rather than one agent working through a long task sequentially, the orchestrating agent delegates separate portions to subagents that run in parallel, then collects and merges their results.

Codex only spawns subagents when explicitly requested via prompts such as:

* Spawn two agents to handle this: one for exploration, one for implementation.
* Use one agent per module and run them in parallel.
* Review this branch with parallel subagents: one for security risks, one for test gaps, and one for maintainability. Summarise the findings by category.

They are well-suited to read-heavy parallel work such as codebase exploration across separate modules, running multiple independent test analyses, or performing a security review across different subsystems simultaneously. For write-heavy tasks, the same file-scope independence rule applies.

<figure><img src="../../.gitbook/assets/image (21).png" alt="" width="438"><figcaption><p>Subagents: Parallel work within a single task</p></figcaption></figure>

Each subagent does its own model and tool work, consuming additional tokens and increasing both cost and latency. Spawning them when not needed adds overhead without benefit.

#### Types of subagents <a href="#types-of-subagents" id="types-of-subagents"></a>

Codex ships with three built-in agents:

* `default` for general-purpose work.
* `worker` for execution-focused implementation and fixes.
* `explorer` for read-heavy codebase exploration.

We can also define custom agents as standalone TOML files placed under `~/.codex/agents/` for personal use or `.codex/agents/` in the repository root for project-scoped agents. Each file requires three fields: `name`, `description`, and `developer_instructions`. If a custom agent name matches a built-in one, the custom agent takes precedence.

#### Subagent configuration <a href="#subagent-configuration" id="subagent-configuration"></a>

The three keys that govern subagent behaviour all live under the `[agents]` block in `config.toml`:

* `agents.max_threads`**:** Caps the total number of concurrently open agent threads, including subagent threads. Defaults to `6`. Workflows that exceed this cap are queued rather than rejected.
* `agents.max_depth`**:** Controls how many levels of nesting are allowed. Defaults to `1`, which allows direct child subagents but prevents deeper recursion. Raising it can turn broad delegation prompts into repeated fan-out, increasing token usage, latency, and local resource consumption.
* `agents.job_max_runtime_seconds`**:** Optional. Sets the per-worker timeout for CSV-based batch jobs. When left unset, the default is 1800 seconds per worker.

With local parallel workflows in place, the next step is extending Codex into the GitHub workflow, where tasks originate and where code is reviewed.

Threads, worktrees, and subagents form a layered model for parallel work. Threads provide independent task sessions within a project. Worktrees provide file-system isolation so those tasks do not conflict. Subagents extend the model by enabling parallel execution within a single task when the scope justifies it. Each layer is optional and can be used independently, but they compose well. A project running several worktree threads, each capable of spawning focused subagents, covers a significant amount of ground with a small amount of manual coordination.
