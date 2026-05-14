---
icon: claude
---

# Claude Desktop and MCP: Local Files and External Apps

> Explore how Claude Desktop and the Model Context Protocol (MCP) extend Claude's capabilities beyond the browser. Learn to connect Claude to local files and external apps securely using desktop extensions and connectors. Understand how to manage permissions, set up integrations, and control Claude's access to enhance your productivity with files and services you already use.

Everything covered so far happens inside a browser. You upload files, type prompts, and get responses, all within Claude.ai. **Claude Desktop** changes the relationship. It is a native application that runs on your computer and can connect to tools, services, and local files through a technology called **MCP**. This lesson covers what Desktop adds, how connections work, and two practical ways to extend what Claude can reach: Connectors for external services, and desktop extensions for local files.

### Why Claude Desktop <a href="#why-claude-desktop" id="why-claude-desktop"></a>

Claude Desktop provides the same core experience as Claude.ai (chat, artifacts, projects, memory, styles) but runs as a standalone application on macOS or Windows. You can download it from [claude.ai/download](http://claude.ai/download).

The practical reasons to use it:

* **Always available:** A native app in your dock or taskbar, not a browser tab competing with fifty others.
* **Local file access:** Desktop can connect to files and folders on your machine through desktop extensions, without uploading anything to the web.
* **Focused work sessions:** A dedicated window for extended work, separate from browser distractions.

If you never need Claude to access anything outside the chat window, [Claude.ai](http://claude.ai/) in the browser is sufficient. Claude Desktop becomes essential when you want Claude to work with files on your computer.

### MCP: The universal adapter <a href="#mcp-the-universal-adapter" id="mcp-the-universal-adapter"></a>

**MCP (Model Context Protocol)** is the open standard that makes all of these connections possible. The simplest way to think about it: MCP is a universal adapter for AI. The same way a USB-C port connects your laptop to monitors, drives, and peripherals through a single standard, MCP connects Claude to files, calendars, databases, and services through a single protocol.

You do not need to understand how MCP works under the hood. What matters is what it enables: Claude can reach beyond the chat window and interact with the tools and data you already use.

Two things are built on MCP: **Connectors** for external web-based services, and **desktop extensions** for local tools and files. They serve different purposes and have different setup paths.

### Connectors <a href="#connectors" id="connectors"></a>

**Connectors** are the simplest way to link Claude to external services. They require no technical setup: browse a directory, authenticate with your account, and the connection is ready. Connectors work across all Claude surfaces (web, Desktop, and mobile) and are available on all plans.

The Connectors Directory includes integrations for tools like Slack, Google Drive, Google Calendar, Linear, Notion, and more. Each connector page describes what it can do: some are read-only (they retrieve information), others can take actions like creating issues, sending messages, or updating records.

#### Setting up a connector <a href="#setting-up-a-connector" id="setting-up-a-connector"></a>

1. From any conversation, click the "**+**" button in the lower-left corner, hover over "Connectors," and select "Manage" connectors. Or go to Customize > Connectors in settings.
2. Browse the directory and click on the connector you want.
3. Click "Connect" and follow the authentication prompts (typically OAuth: you log in to the service and grant Claude access).
4. Once connected, toggle the connector on in any conversation where you want Claude to use it.

> **Team and Enterprise:** An organization owner must enable connectors at the organization level before members can use them.

With a connector active, you use it through natural conversation. “What meetings do I have next week?” draws on your calendar. “Find the Q3 budget document in our shared Drive” searches your files. Claude engages the connected service when your request calls for it.

#### A note on sensitive integrations <a href="#a-note-on-sensitive-integrations" id="a-note-on-sensitive-integrations"></a>

Email connectors (Gmail, Outlook) let Claude read and draft emails. Review the permissions carefully before connecting. Granting read access to your inbox is a significant authorization, and the same applies to any integration that can take actions on your behalf. Only connect integrations you actively need, and disconnect them when you are done.

### Desktop extensions <a href="#desktop-extensions" id="desktop-extensions"></a>

For access to files on your computer, Claude Desktop adds a second layer with **desktop extensions**. These are locally-installed packages that run on your machine and give Claude access to things that cannot go through a remote connector, primarily your local file system.

Desktop extensions are installed through Claude Desktop’s built-in directory. No additional software is required as Claude Desktop includes its own runtime environment.

#### Installing an extension <a href="#installing-an-extension" id="installing-an-extension"></a>

1. Open Claude Desktop and go to Settings > Extensions.
2. Click "Browse extensions" to open the directory.
3. Find the extension you want (for local file access, look for the "filesystem" extension) and click "Install".
4. Configure the required settings. For the filesystem extension, you specify which folder on your computer Claude can access. Claude cannot reach anything outside the folder you designate.

After installation, the extension is available in your conversations automatically.

> If you’re unable to view the extension in the drop-down menu after installing it, restarting Claude Desktop is a good idea.

#### Using the filesystem extension <a href="#using-the-filesystem-extension" id="using-the-filesystem-extension"></a>

Once the filesystem extension is set up, you can ask Claude to work with your files directly:

* “What files are in my work folder?”
* “Read the Q3 report and summarize the key findings.”
* “Create a new folder called Archive and move all files older than January into it.”
* “Find all spreadsheets in this folder and list them with their file sizes.”

Claude reads the folder contents, proposes actions, and waits for your approval before executing anything.

#### The approval flow <a href="#the-approval-flow" id="the-approval-flow"></a>

Every action Claude takes through a desktop extension requires your explicit approval. When Claude wants to read a file, it tells you which file and asks permission. When it wants to move or create a file, it describes the action and waits for you to confirm.

You can deny any action at any point. Claude never acts on your files without your knowledge.

### Security and trust <a href="#security-and-trust" id="security-and-trust"></a>

Both Connectors and desktop extensions give Claude access beyond the chat window. That access comes with clear boundaries:

* **Explicit approval:** Claude describes every action before taking it. You approve or deny each one. Nothing happens without your consent.
* **Scoped access:** You choose which folders, calendars, or services Claude can reach. The filesystem extension only accesses the directory you configure. A calendar connector only reads the calendar you authorize.
* **No unsolicited actions:** Claude does not initiate actions through connections outside of your active conversations. It acts only when you ask it to, within a session you have started.
* **You can revoke access:** Disconnect a connector in settings or uninstall a desktop extension at any time, and the access is gone.

The principle is straightforward: you control what Claude can reach, and Claude always asks before acting.

### Try this now <a href="#try-this-now" id="try-this-now"></a>

Choose one of the following based on what is most useful for your work.

**Option A: Connect an external service**

1. Go to Customize > Connectors in [Claude.ai](http://claude.ai/) settings.
2. Browse the directory and find a service you already use (Google Drive, Google Calendar, Slack, or another tool in the list).
3. Connect it and authenticate.
4. Start a conversation, toggle the connector on, and ask Claude something that uses it: “What is on my calendar this week?” or “Find documents related to \[project name] in my Drive.”

**Option B: Set up local file access**

1. Download and install Claude Desktop from [claude.ai/download](http://claude.ai/download).
2. Go to Settings > Extensions, browse the directory, and install the filesystem extension.
3. Configure it to point to a folder of your choice.
4. Ask Claude: _What files are in this folder?_ Then ask it to read and summarize one.

Either path demonstrates the core pattern: Claude reaching beyond the chat window to work with things that live outside it.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Claude Desktop extends Claude beyond the browser through two complementary mechanisms. Connectors link Claude to external services (calendars, drives, project tools) through a directory of ready-to-use integrations that require no technical setup. Desktop extensions give Claude access to files on your local machine through locally-installed packages. Both operate on the same principle: you control what Claude can reach, and every action requires your approval.
