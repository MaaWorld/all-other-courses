---
icon: claude
---

# Claude Cowork: Multi-Step Task Execution

> \Explore how to use Claude Cowork to delegate and automate complex multi-step tasks by connecting to your local files and applications via Claude Desktop. Understand setting up workspace folders, creating context files, and guiding Cowork through planning, execution, and output of tasks. Learn to manage tasks interactively, leverage plugins and skills to extend functionality, and optimize workflows for report generation, file organization, and data analysis.

Claude.ai answers questions. Claude Desktop connects to your tools. Claude Cowork does the work. It is an autonomous agent that reads your files, plans multi-step tasks, executes them, and produces real outputs. Instead of asking Claude to explain how to create a summary report and then doing it yourself, you tell Cowork to create the report. It reads the data, decides how to structure the output, writes the document, and saves it. You steer the process, but Claude does the execution. This lesson covers how to set Cowork up, how it works, what it is good at, and how to guide it effectively.

### What is Claude Cowork? <a href="#what-is-claude-cowork" id="what-is-claude-cowork"></a>

**Cowork** is a Claude product where Claude works directly with your files, folders, and applications. The core difference from [Claude.ai](http://claude.ai/) chat is the relationship between you and Claude.

In a chat conversation, you and Claude exchange messages. You ask, Claude responds, you refine, Claude responds again. The output is the conversation itself, and anything Claude produces (an artifact, an analysis, a draft) exists within that conversation.

In Cowork, you assign a task. Claude plans how to accomplish it, breaks it into steps, executes each step, and delivers the result. You are not exchanging messages back and forth. You are delegating work and reviewing the output.

Think of the difference this way: [Claude.ai](http://claude.ai/) chat is a conversation with a knowledgeable colleague. Cowork is handing a task to an assistant who goes away, does the work, and comes back with the result.

### Setting up Cowork <a href="#setting-up-cowork" id="setting-up-cowork"></a>

Cowork is accessed through Claude Desktop. Download it from [claude.ai/download](http://claude.ai/download) if you have not already. You need a Pro plan or higher. The Desktop app must remain open for the duration of any task; closing it will interrupt execution in progress.

When you start Cowork, you select a folder on your computer. This is the workspace Cowork can access: the files it can read, the location where it saves outputs. Choosing the right folder and organizing it well makes a significant difference in output quality.

#### Folder architecture <a href="#folder-architecture" id="folder-architecture"></a>

Before your first real task, set up a simple folder structure. A pattern that works well:

```markdown
// index.md

CLAUDE COWORK/
├── ABOUT ME/          ← who you are, your style, your preferences
├── PROJECTS/          ← subfolders for each active project or client
│   ├── client-alpha/
│   └── quarterly-reporting/
├── TEMPLATES/         ← reusable structures and formats
└── CLAUDE OUTPUTS/    ← where Cowork saves everything it creates

```

The separation matters. Cowork reads from ABOUT ME, PROJECTS, and TEMPLATES for context. It writes to CLAUDE OUTPUTS. This keeps your source material untouched and Cowork’s deliverables organized.

#### Context files <a href="#context-files" id="context-files"></a>

The files in your workspace shape how Cowork writes and works. Two files are particularly useful to create before you start:

* `about-me.md`: A markdown file describing who you are, what you do, your current priorities, and how you communicate. This is not Memory (which works in [Claude.ai](http://claude.ai/) chat). Cowork reads files, so it needs this information in a file.

Example content:

```markdown
// index.md

# About Me
- Role: Marketing Director at a B2B fintech startup
- Team: 4 direct reports (content, demand gen, product marketing, design)
- Current priority: Q3 product launch campaign
- Communication style: direct, data-driven, no jargon
- Deliverables usually go to: VP of Product, CEO, or external partners
```

* `writing-guide.md`**:** A file that tells Cowork how to write. This is surprisingly effective. Instead of only describing what you want, you can also describe what you want to avoid: “Do not use phrases like ‘dive into,’ ‘leverage,’ or ‘at the end of the day.’ Do not start paragraphs with ‘It is worth noting.’ Avoid bullet points that all start with gerunds.”

One strong context file is worth more than dozens of shallow uploads. Focus on quality over quantity.

#### Global instructions <a href="#global-instructions" id="global-instructions"></a>

Cowork has its own persistent instructions, separate from [Claude.ai](http://claude.ai/) Project instructions. Access them through: Settings > Cowork > Edit Global Instructions.

These instructions apply to every Cowork session. Use them to set operating rules:

```md
// index.md

- Read the ABOUT ME/ folder before starting any task
- Check the matching PROJECTS/ subfolder for relevant context
- Study TEMPLATES/ for format patterns before creating new deliverables
- Save all outputs to CLAUDE OUTPUTS/ only
- Never modify or delete files in ABOUT ME/, PROJECTS/, or TEMPLATES/
- Use this naming convention for outputs: project_content-type_v1.ext
  (Example: client-alpha_brief_v1.md)
- When a task is unclear, ask clarifying questions before executing
```

With these in place, every session starts with Cowork already knowing where to look, where to save, and how to behave. You do not repeat this setup in every prompt.

#### The task loop <a href="#the-task-loop" id="the-task-loop"></a>

When you give Cowork a task, it follows a consistent process:

1. **Plan:** Cowork reads your instructions, examines the available files and context, and creates a plan. It identifies what steps are needed and in what order.
2. **Execute:** Cowork works through the plan step by step. It reads files, processes data, writes content, creates documents, and organizes outputs.
3. **Produce:** Cowork delivers the result. This might be a saved file, a report, a reorganized folder, or any other tangible output.

You can watch this process unfold. Cowork shows you what it is doing at each stage, so you know where it is in the task and can intervene if needed.

#### Clarifying questions before execution <a href="#clarifying-questions-before-execution" id="clarifying-questions-before-execution"></a>

One of Cowork’s most useful behaviors is asking you questions before it starts working. When you include “Ask me questions before executing” in your prompt, Cowork generates an interactive form with specific questions about your task: what audience is this for, which sections to prioritize, what tone to use, and how long the output should be.

This takes under a minute to complete and dramatically improves first-draft quality. Instead of Cowork guessing your preferences, it gathers them upfront. You approve the plan, and Cowork executes with confidence.

#### A prompt template that works for most tasks <a href="#a-prompt-template-that-works-for-most-tasks" id="a-prompt-template-that-works-for-most-tasks"></a>

Most of your Cowork sessions can start with a variation of this template:

> I want to \[TASK] so that \[SUCCESS CRITERIA]. First, explore my CLAUDE COWORK folder for relevant context. Then, ask me clarifying questions before you execute. I want to refine the approach before you begin.

For example: “I want to create a competitive analysis of our top three competitors so that I can present it at Thursday’s leadership meeting. First, explore my CLAUDE COWORK folder for relevant context. Then, ask me clarifying questions before you execute. I want to refine the approach before you begin.”

This pattern gives Cowork a clear goal, tells it to use your context files, and forces alignment before execution. Save it as a text shortcut on your computer (on Mac: System Settings > Keyboard > Text Replacements, map something like `/cwprompt` to the template) so you can insert it instantly.

#### What shapes the plan <a href="#what-shapes-the-plan" id="what-shapes-the-plan"></a>

The quality of Cowork’s output depends heavily on what you provide upfront:

* **Clear task description:** “Create a summary report from the Q3 sales data in this folder” is good. “Do something with these files” is not.
* **Available files:** Cowork works with the files it can access. Make sure the relevant data, templates, and reference documents are in the workspace.
* **Constraints and preferences:** “Use the same format as last quarter’s report” or “Keep it under two pages” helps Cowork make better decisions about structure and scope.

The more specific your input, the less Cowork has to guess, and the closer the first output will be to what you want.

### Managing a task <a href="#managing-a-task" id="managing-a-task"></a>

Cowork is autonomous, but you are not locked out once a task starts. You can steer it during execution. This is one of the most important skills for working with Cowork effectively.

#### Course correction <a href="#course-correction" id="course-correction"></a>

If you see Cowork heading in a direction that does not match what you want, you can redirect it mid-task:

* “Focus on the North America region only, skip the others.”
* “Use a table format instead of paragraphs for the comparison.”
* “The data in the second spreadsheet is more recent. Prioritize that one.”

Cowork adjusts its plan and continues from where it is, incorporating your feedback without starting over.

#### Checking progress <a href="#checking-progress" id="checking-progress"></a>

For longer tasks, check in on what Cowork has produced so far. If the intermediate output looks right, let it continue. If something is off, correct it early before Cowork builds on a wrong foundation.

#### Knowing when to intervene <a href="#knowing-when-to-intervene" id="knowing-when-to-intervene"></a>

Not every task needs steering. Short, well-defined tasks (“Organize these files by date into subfolders”) usually run cleanly without intervention. Longer, more ambiguous tasks (“Analyze this data and create a presentation”) benefit from a checkpoint after the initial plan or first major output.

A practical rule: if the task takes more than a few minutes, glance at the plan and first output before letting Cowork finish.

### What Cowork does well <a href="#what-cowork-does-well" id="what-cowork-does-well"></a>

Cowork is most effective for tasks that are multi-step, file-based, and produce a concrete output. Here are patterns that work well:

* **Report generation:** “Read the sales data in this folder, calculate the key metrics, and create a formatted summary report.” Cowork reads the data, runs the analysis, writes the report, and saves it.
* **File organization:** “Sort the files in this folder into subfolders by type: documents, spreadsheets, images, and presentations.” Cowork scans the folder, identifies file types, creates subfolders, and moves the files. For your first file organization task, run it on a test folder rather than live data until you are comfortable with how Cowork handles your files.
* **Data transformation:** “Take this CSV of customer feedback and create a structured analysis: categorize by theme, rate sentiment, and produce a summary with the top issues.” Cowork processes the data and produces the output.
* **Content creation from source material:** “Read these three meeting transcripts and create a single executive brief covering the key decisions, open questions, and action items.” Cowork reads all three files, synthesizes the content, and writes the brief.
* **Batch processing:** “For each client folder, read the latest invoice and create a payment summary spreadsheet.” Cowork works through each folder and compiles the results.

The common thread is that these tasks have a clear input (files), a clear process (read, analyze, transform), and a clear output (a deliverable). Cowork handles the execution, so you do not have to do each step manually.

### When to use Cowork vs. Claude.ai chat <a href="#when-to-use-cowork-vs-claudeai-chat" id="when-to-use-cowork-vs-claudeai-chat"></a>

The two products overlap in some areas but serve different purposes. Choosing the right one saves time and produces better results.

|                 | **Claude.ai Chat**                                          | **Cowork**                                       |
| --------------- | ----------------------------------------------------------- | ------------------------------------------------ |
| **Interaction** | Back-and-forth conversation                                 | Task delegation and review                       |
| **Best for**    | Questions, brainstorming, drafting, and iterating on ideas  | Multi-step tasks that produce file-based outputs |
| **Output**      | Chat responses, artifacts within the conversation           | Files saved to your workspace                    |
| **Control**     | You guide every response                                    | You set the task and steer as needed             |
| **Speed**       | Immediate responses per message                             | Longer execution, but hands-free                 |
| **Example**     | “Help me think through the structure for this presentation” | “Read my notes and create the presentation”      |

Use chat when you want to think alongside Claude: brainstorming, exploring options, refining a draft through iteration. Use Cowork when you know what you want produced, and you want Claude to do the execution.

A workflow that combines both is common. Start in [Claude.ai](http://claude.ai/) chat to explore an approach, decide on a structure, and align on the output format. Then hand the task to Cowork to produce the actual deliverable. The thinking happens in chat; the doing happens in Cowork.

### Plugins and slash commands <a href="#plugins-and-slash-commands" id="plugins-and-slash-commands"></a>

**Plugins** extend what Cowork can do beyond its default file-based capabilities. Each plugin adds specialized workflows and slash commands that you can invoke directly in the chat.

#### Installing plugins <a href="#installing-plugins" id="installing-plugins"></a>

1. Open Cowork.
2. Click Customize > Browse plugins.
3. Browse the directory, select a plugin, and click install. Plugins are free.
4. Type "**/**" in the Cowork chat to see all available slash commands from your installed plugins.

#### What plugins are available <a href="#what-plugins-are-available" id="what-plugins-are-available"></a>

Plugins cover a range of professional workflows. The actual command names depend on which plugins you install, and you can also create custom commands for your own workflows. Here are a few popular ones:

* **Marketing:** Generates LinkedIn posts, newsletters, and marketing copy using your voice profile and brand context.
* **Data:** Creates interactive dashboards from CSV files, analyzes datasets with summary statistics and visualizations.
* **Legal:** Contract review, clause flagging, and compliance checking.
* **Sales:** Prospect research and outreach drafting.
* **Productivity:** Task workflows and project planning.
* **Scheduling:** Sets up recurring automated tasks (for example, a weekly briefing that runs every Monday morning and saves the output to a designated folder). Scheduled tasks require Claude Desktop to be running at the scheduled time.

Each plugin asks setup questions the first time you use it (your role, preferences, typical audience) and remembers your answers for future sessions. After that, invoking a slash command produces output tailored to your context without additional prompting.

### Skills <a href="#skills" id="skills"></a>

**Skills** are reusable instruction sets that teach Cowork specific workflows. If you have a recurring task with particular steps, formatting, and quality standards, a skill encodes that knowledge so Cowork executes it consistently every time. Think of a skill as a saved recipe: you define the process once, and Cowork follows it on demand.

For example, a “Weekly Status Report” skill might specify: read the PROJECTS folder for updates since last Friday, summarize each project’s status in two bullet points, flag any items marked as blocked, and format the output using the template in TEMPLATES/weekly-status.md. Once the skill is saved, you invoke it with a single command rather than re-explaining the process each week.

### Usage and limits <a href="#usage-and-limits" id="usage-and-limits"></a>

Cowork consumes tokens faster than [Claude.ai](http://claude.ai/) chat. A single session that reads multiple files, plans, and produces a deliverable uses significantly more than a standard chat conversation. Pro plan limits may not be sufficient for regular Cowork use. Max plan is recommended if Cowork becomes a core part of your workflow.

> Cowork requires the Claude Desktop app (macOS or Windows). It does not run in the browser.

Cowork is powerful but not infallible. For complex multi-step tasks, the output is right most of the time, but occasionally sections may be misaligned, or a file may be misread. Always review deliverables before sending them to clients or stakeholders. If Cowork moves or creates files incorrectly, check the CLAUDE OUTPUTS folder first. Cowork writes there rather than modifying your source files, so the damage is usually contained. For any files moved from their original location, your operating system’s undo function (Cmd+Z on Mac, Ctrl+Z on Windows) can reverse recent file operations. Treat Cowork like a capable but new team member: trust, but verify.

### Try this now <a href="#try-this-now" id="try-this-now"></a>

Set up the foundation for Cowork before your first real task.

1. Create a folder called "CLAUDE COWORK" on your computer. Inside it, create four subfolders: _ABOUT ME, PROJECTS, TEMPLATES,_ and _CLAUDE OUTPUTS._
2. In the _ABOUT ME_ folder, create a file called `about-me.md`. Fill it in: your role, your team, your current priorities, your communication style, and who your work typically goes to.
3. Open Cowork in Claude Desktop, set your workspace to the CLAUDE COWORK folder, and add the global instructions from this lesson to Settings > Cowork > Edit Global Instructions.
4. Run your first task using the prompt template: “I want to \[TASK] so that \[SUCCESS CRITERIA]. First, explore my CLAUDE COWORK folder for relevant context. Then, ask me clarifying questions before you execute.”

Use a real but low-stakes task for this first run, such as a file reorganization, a summary of a document already in the folder, or a draft of something short.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Cowork shifts Claude from answering questions to executing tasks. Setting it up well (folder architecture, context files, global instructions) makes the difference between generic output and output that sounds like it came from your team. The task loop (plan, execute, produce) handles multi-step work autonomously, and clarifying questions keep output aligned with what you actually need. Plugins and slash commands add specialized workflows. For thinking and iterating, Claude.ai chat remains the better tool. For doing, Cowork takes over.
