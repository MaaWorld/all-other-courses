---
icon: openai
---

# MCP: Connecting Codex to External Tools

> Explore how MCP servers extend OpenAI Codex's capabilities by connecting it to external tools and data sources. Understand the configuration and security considerations involved in integrating MCP servers to streamline coding tasks, access live documentation, issue tracking, design files, and more within a unified agentic workflow.

Codex is effective within the boundary of the local repository. It reads files, runs commands, writes code, and verifies its work through tests. But most software projects do not live entirely in one folder. A bug report lives in a GitHub issue. The API we are integrating against has documentation hosted externally. A design we need to implement exists as a Figma frame. The logs from last night’s outage are in Sentry.

Without a way to reach those systems, Codex has to work around the gap. We paste issue descriptions into the prompt manually. We copy documentation excerpts and append them as context. We screenshot Figma frames and describe what we see. Each workaround adds friction, breaks the task’s flow, and introduces the possibility of human error in the transcription.

<figure><img src="../../.gitbook/assets/image (18).png" alt="" width="185"><figcaption></figcaption></figure>

**Model Context Protocol (MCP)** is an open standard that solves this problem by giving AI agents a unified way to connect to external tools, data sources, and services. When we configure an MCP server for Codex, it gains the ability to read from and act on that system as part of any task, without any manual copying or pasting. A single prompt can tell Codex to read the open GitHub issue, check the relevant documentation, and implement the fix, all in one uninterrupted run.

### Scenario: Writing integration code without stale documentation <a href="#scenario-writing-integration-code-without-stale-documentation" id="scenario-writing-integration-code-without-stale-documentation"></a>

Imagine we are adding a new payment provider to our project. The provider has a Python SDK, but the version we need was released three months ago, and our model’s training data predates it. If we ask Codex to write the integration, it will produce code based on an older version of the API that may no longer work.

With MCP, we can connect Codex to a live documentation server before the task begins. Codex queries the current SDK reference, finds the correct method signatures, and writes integration code against the version we are actually using. No outdated patterns, no manual documentation lookup, no prompts that paste in three pages of API reference. The external tool becomes part of how Codex works, not an interruption we manage around it.

MCP provides access to the additional context Codex requires, bridging the gap between repository visibility and the information needed to complete tasks accurately.

### Two types of MCP servers <a href="#two-types-of-mcp-servers" id="two-types-of-mcp-servers"></a>

Codex supports two server types, each suited to a different class of tool.

* **STDIO servers:** They run as a local child process. Codex starts the server by executing a command on the machine and communicates with it over standard input and output. These servers are appropriate for tools that are installed locally, such as documentation fetchers, code analysis utilities, or any integration where the processing happens on the same machine as Codex.
* **Streamable HTTP servers:** They are accessed via a URL. Codex connects to an already-running remote service rather than launching anything locally. These servers support bearer token authentication and OAuth. They are appropriate for cloud-hosted services such as Figma, Sentry, or any API that exposes an MCP-compatible endpoint.

Both types are configured in the same place and accessed the same way during a task. The difference is entirely in how the server starts and where it runs.

### Configuring MCP servers <a href="#configuring-mcp-servers" id="configuring-mcp-servers"></a>

MCP configuration lives in `config.toml` alongside the rest of Codex settings. The global file at `~/.codex/config.toml` makes a server available across all projects. A project-scoped file at `.codex/config.toml` (inside a trusted project directory) limits the server to that project only. The CLI and the IDE extension share this configuration, so servers configured once work across both surfaces. There are two ways to add a server.

* The first is the CLI. Running `codex mcp add` with the server name and startup command handles the configuration automatically:

```
# Add the context7 documentation server using npx
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

* The second is editing `config.toml` directly, which gives us more control over every available option.

#### Configuring a STDIO server <a href="#configuring-a-stdio-server" id="configuring-a-stdio-server"></a>

Each server entry uses a `[mcp_servers.<server-name>]` table. For a STDIO server, the required field is `command`. We can also pass `args`, set environment variables under `env`, and specify a working directory with `cwd`:

```
# context7 documentation server — runs as a local process via npx
[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp"]

[mcp_servers.context7.env]
# Optional: pass any environment variables the server needs
LOG_LEVEL = "info"
```

#### Configuring a Streamable HTTP server <a href="#configuring-a-streamable-http-server" id="configuring-a-streamable-http-server"></a>

For HTTP servers, the required field is `url`. Authentication is handled through `bearer_token_env_var`, which names an environment variable that holds the token rather than embedding credentials in the config file directly:

```
# Figma MCP server — connects to the Figma cloud service
[mcp_servers.figma]
url = "https://mcp.figma.com/mcp"
bearer_token_env_var = "FIGMA_OAUTH_TOKEN"  # token is read from the environment at runtime
```

For servers that support OAuth, we authenticate using `codex mcp login <server-name>` rather than managing a token manually. Codex handles the OAuth flow and stores the credentials.

#### Controlling what each server can do <a href="#controlling-what-each-server-can-do" id="controlling-what-each-server-can-do"></a>

Every MCP server exposes a set of tools to Codex. By default, all tools the server advertises are available. Two configuration options let us narrow that scope.

`enabled_tools` is an allow list. When set, Codex can only use the tools named in the list, regardless of what the server exposes. `disabled_tools` is a deny list applied after `enabled_tools`. Naming a tool here removes it from what Codex can call, even if it appears in `enabled_tools`.

```
# Chrome DevTools MCP server with restricted tool access
[mcp_servers.chrome_devtools]
url = "http://localhost:3000/mcp"
enabled_tools = ["open", "screenshot", "evaluate"]  # only these three tools are available
disabled_tools = ["evaluate"]                        # applied after enabled_tools; removes evaluate
startup_timeout_sec = 20                             # wait up to 20 seconds for the server to start
tool_timeout_sec = 45                                # individual tool calls time out after 45 seconds
```

Two other options are worth knowing. Setting `enabled = false` disables a server without removing its configuration, which is useful when we want to temporarily stop Codex from using a server without losing the setup. Setting `required = true` makes Codex fail at startup if the server cannot initialise, rather than silently proceeding without it. For a task that depends entirely on a particular server being available, `required = true` surfaces the problem immediately instead of producing a failed task later.

### MCP servers worth knowing <a href="#mcp-servers-worth-knowing" id="mcp-servers-worth-knowing"></a>

The list of available MCP servers grows regularly. A few are especially useful for common development workflows:

* **Context7:** Fetches up-to-date library and framework documentation. When Codex writes integration code, it queries the current reference rather than relying on training data that may describe an older version.
* **GitHub MCP server:** Gives Codex access to GitHub beyond what `git` supports. Codex can read open issues, create branches, open pull requests, and check CI status as part of a task.
* **Figma (local and remote):** Connects Codex to Figma designs. Codex can inspect component structures and design tokens directly instead of working from a description.
* **Playwright:** Allows Codex to control and inspect a browser. Useful for writing and debugging end-to-end tests against a running application.
* **Chrome DevTools:** Similar to Playwright but connects directly to Chrome, giving Codex access to the DevTools protocol for inspection and evaluation.
* **Sentry:** Gives Codex access to error logs and issue data from Sentry. Codex can read the full stack trace and context for an error before writing a fix.
* **OpenAI Docs MCP:** Connects Codex to the OpenAI developer documentation, useful when building applications on top of the OpenAI API.

The right MCP servers to install depend entirely on the workflow. A good starting point is identifying the manual step that recurs most often in our tasks, whether that is looking up documentation, checking an open issue, or inspecting a design, and adding the server that eliminates it.

### Running Codex as an MCP server <a href="#running-codex-as-an-mcp-server" id="running-codex-as-an-mcp-server"></a>

The relationship between Codex and MCP is not one-directional. Codex can consume MCP servers, but it can also be exposed as one. Running `codex mcp-server` starts Codex as an MCP server over `stdio`, making it available as a tool that other agents or orchestration systems can call.

```
# Start Codex as an MCP server so other agents can call it
codex mcp-server
```

When running in this mode, Codex exposes two tools to the calling agent:

* `codex()` to start a new conversation.
* `codex-reply()` to continue an existing one.

This keeps Codex alive across multiple agent turns, so context is preserved between calls. The practical application is multi-agent pipelines. An orchestrator agent handles high-level planning and task decomposition. When a subtask requires writing or modifying code, the orchestrator delegates it to Codex via the MCP interface. Codex handles the implementation and returns the result. The orchestrator incorporates it and continues. Neither agent needs to know the internal details of how the other works; the MCP interface provides a clean boundary between them.

> If you want to learn more about MCP, explore our course [Mastering MCP: Building Advanced Agentic Applications](https://design-for-me.devpath.com/courses/advanced-model-context-protocol).

### Security considerations with MCP <a href="#security-considerations-with-mcp" id="security-considerations-with-mcp"></a>

Every MCP server connection introduces a new surface that Codex interacts with. Content that arrives through an MCP server can contain instructions that attempt to redirect or manipulate Codex’s behaviour. This is called prompt injection, and it is a real risk when the server pulls content from external sources such as issue trackers, documentation sites, or user-generated data.

Three practices reduce the risk.

1. Grant only the permissions each server genuinely needs. If a server only needs to read documentation, do not configure it with write access. Use `enabled_tools` to limit what tools the server exposes to Codex.
2. Mark non-essential servers with `enabled = false` when they are not needed for the current workflow.
3. Review any Codex output that consumed external data before accepting it. A Codex response that read from a GitHub issue or an external documentation page should be inspected with the same care as any other change that came from outside the codebase.

MCP makes Codex significantly more capable by connecting it to the systems where real development work happens. Treating those connections with the same scrutiny we apply to any external dependency keeps that capability from becoming a liability.

Configuring even a single MCP server changes what a task can accomplish. A development workflow that previously required switching between four tools and manually assembling context can become a single prompt with the right servers in place. Starting with one server that removes a step we already do manually is enough to understand how the whole system compounds.
