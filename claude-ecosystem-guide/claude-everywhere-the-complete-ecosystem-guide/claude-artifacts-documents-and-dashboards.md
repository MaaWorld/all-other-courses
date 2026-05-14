---
icon: claude
---

# Claude Artifacts: Documents and Dashboards

> Explore how Claude transforms chat interactions into tangible outputs like documents, diagrams, and dashboards using artifacts. Understand the role of code execution in generating precise data analyses, charts, and downloadable files. Learn to create, iterate, and share artifacts to enhance your workflow with Claude.ai's production capabilities.

Beyond answering questions, Claude can produce tangible outputs such as documents, diagrams, dashboards, visualizations, and interactive components that live outside the chat. Artifacts are how Claude delivers these outputs. Code execution is how Claude works with data and files, running real code behind the scenes to calculate, chart, transform information, and generate finished files. Together, they turn Claude from a conversational assistant into something closer to a production tool. This lesson covers what you can create, how to iterate on it, and how to share the results.

### Artifacts <a href="#artifacts" id="artifacts"></a>

An **artifact** is a piece of generated content that appears in a dedicated panel next to the conversation, separate from the chat itself. When Claude produces something substantial (a document, a diagram, a piece of code, an interactive component), it renders it as an artifact rather than dumping it inline.

This separation matters. Chat messages scroll away. Artifacts persist in the side panel, where you can review, copy, download, or ask Claude to modify them. It is the difference between Claude telling you what a report could look like and Claude handing you the report.

#### What artifacts can be <a href="#what-artifacts-can-be" id="what-artifacts-can-be"></a>

Artifacts support a range of content types:

* **Documents:** Formatted text like reports, memos, proposals, and briefs. Claude renders them with headings, lists, and structure, ready to copy into your word processor.
* **Code:** Working code in languages like Python, JavaScript, or HTML. Useful even if you are not a developer, because the analysis tool (covered below) executes code artifacts automatically.
* **Mermaid diagrams:** Flowcharts, org charts, sequence diagrams, and process maps. You describe the structure in plain language, and Claude generates the visual. One thing to note: if you want to use a Mermaid diagram outside Claude.ai, take a screenshot or export it. Copying the artifact directly gives you the text syntax behind the diagram, not the rendered image.
* **SVG graphics:** Vector images like icons, simple illustrations, or data-driven visuals. SVG works best for simple shapes and icons; complex illustrations can have rendering inconsistencies.
* **Interactive components:** Dashboards, calculators, and tools that you can click, filter, and explore directly in the artifact panel.
* **Websites:** Full HTML pages with styling, rendered as a live preview. Useful for creating shareable mini-reports or simple landing pages without needing a web platform.

You can just describe the output you need, and Claude chooses the appropriate format. “Create a flowchart of our hiring process” produces a Mermaid diagram. “Build a dashboard that shows monthly revenue by region” produces an interactive component. “Write a one-page executive summary of this report” produces a document.

#### Iterating on artifacts <a href="#iterating-on-artifacts" id="iterating-on-artifacts"></a>

Artifacts are not final drafts. You refine them through conversation, the same way you would give feedback to a colleague.

After Claude generates an artifact, you can ask it to make changes:

* Make the chart a bar chart instead of a line chart.
* Add a column for year-over-year percentage change.
* Shorten the executive summary to half a page.
* Change the flowchart to include a decision point after the screening stage.

Claude updates the artifact in place. You see the new version immediately and can continue refining. This iterative loop (generate, review, refine) is how most practical work with artifacts happens. The first version is rarely the last.

#### Publishing and sharing <a href="#publishing-and-sharing" id="publishing-and-sharing"></a>

Once an artifact is ready, you can share it beyond the conversation.

* **Publish:** Available on Free, Pro, and Max plans. Make an artifact publicly accessible via a unique URL. Anyone with the link can view and interact with it, including people without a Claude account. This works well for sharing dashboards, reference documents, or interactive tools with stakeholders outside your organization.
* **Customize:** Anyone with a Claude account can open a published artifact in their own conversation and modify it independently. Changes they make do not affect your original. Useful for templates: publish a report format, and your team can customize it with their own data.
* **Team and Enterprise sharing:** On Team and Enterprise plans, artifacts cannot be published publicly. Instead, they can be shared internally with organization members who have the appropriate access.

Two details worth knowing before you publish. First, once you unpublish an artifact, you cannot re-publish that same version. Second, when someone views a published artifact, they also gain access to any files or attachments from the conversation that created it. Avoid publishing artifacts from conversations that include sensitive documents. Publishing is optional. By default, artifacts are private to your conversation.

### Code execution <a href="#code-execution" id="code-execution"></a>

**Code execution** is Claude’s built-in computing environment. When you ask Claude to do something that requires computation (math, data analysis, chart creation, or generating a file), it writes code, runs it in a private sandbox, and returns the result. You see the output, not the code (unless you ask for it). Code execution is available on all plans, including Free.

Beyond charts and calculations, code execution can produce downloadable files: Excel spreadsheets, Word documents, PowerPoint presentations, PDFs, and CSVs. Describe what you need, and Claude builds it.

Code execution elevates Claude's capabilities from insightful analysis to concrete deliverables. Rather than offering estimates, Claude can now compute precise values, perform advanced statistical analysis, create accurate visualizations, and produce finished files you can put to work right away.

#### When code execution activates <a href="#when-code-execution-activates" id="when-code-execution-activates"></a>

Claude uses the analysis tool automatically when your request involves:

* **Math and calculations:** “What is the compound annual growth rate of these revenue figures?” Claude writes the formula, computes it, and gives you the exact number.
* **Data analysis:** “Upload this CSV and tell me which product categories are growing fastest.” Claude parses the file, runs the analysis, and produces a summary with precise figures.
* **Charts and visualizations:** “Create a line chart showing monthly sales for the past year.” Claude generates the chart as an artifact.
* **Data transformation:** “Convert this spreadsheet into a pivot table grouped by region and quarter.” Claude restructures the data and presents the result.

You do not need to ask Claude to “use code execution.” If the task benefits from computation or file creation, Claude engages it. If you want to be explicit, phrases like “analyze this data,” “calculate,” “create a chart,” or “generate a spreadsheet” signal that code execution is appropriate.

#### A practical example <a href="#a-practical-example" id="a-practical-example"></a>

Consider this workflow. You upload a CSV of monthly sales data with columns for date, region, product, and revenue.

**Prompt:** _Analyze this data. Show me the top 5 products by total revenue, the month-over-month growth trend, and a breakdown by region. Present the results as a dashboard._

Claude reads the CSV, writes code to process it, and produces an interactive artifact with:

* A ranked table of the top 5 products
* A line chart of monthly revenue trends
* A bar chart breaking down revenue by region
* Filter controls to explore the data by different dimensions

This is a realistic initial output for a single exchange. The first version will likely need refinement. You can expect to ask for adjustments like “show the trend as a 3-month rolling average” or “sort the table by revenue descending” before it looks exactly how you might want it.

#### What code execution handles well <a href="#what-code-execution-handles-well" id="what-code-execution-handles-well"></a>

* Generating downloadable files (Excel spreadsheets, Word documents, PowerPoint presentations, PDFs, CSVs).
* Summarizing and aggregating tabular data.
* Calculating statistics (averages, medians, growth rates, correlations).
* Creating charts (line, bar, pie, scatter, area).
* Cleaning and transforming messy data.
* Comparing datasets.
* Running what-if scenarios (What happens to total cost if we increase headcount by 15%?).

#### Limits to keep in mind <a href="#limits-to-keep-in-mind" id="limits-to-keep-in-mind"></a>

Code execution runs in a sandboxed environment.

On Free, Pro, and Max plans, network access is enabled by default, which allows Claude to pull in packages and resources when needed. Team and Enterprise plans have network access disabled by default; organization administrators configure these settings.

For most spreadsheet and document work, network access is not relevant, as Claude works with the files you provide. If you need Claude to pull live data, combine code execution with web search: use web search to get the numbers, then ask Claude to analyze them.

> **Note:** The sandbox resets at the end of each conversation. Code, intermediate calculations, and computation state do not carry over to a new conversation. The artifact and any downloaded files persist, but the underlying work does not. For analyses you run repeatedly, save your prompt and source data in a Project so you can reproduce the workflow from scratch.

### Combining artifacts and code execution <a href="#combining-artifacts-and-code-execution" id="combining-artifacts-and-code-execution"></a>

In practice, code execution and artifacts work together constantly. Code execution does the computation; artifacts display the result. When Claude analyzes your spreadsheet and produces a chart, the chart is an artifact. When it calculates growth rates and formats them into a table, the table is an artifact. Code execution is the engine; the artifact is the deliverable.

This combination is what makes Claude effective for the kind of outputs that professionals actually need: not a chat message explaining what the data shows, but a polished visual you can paste into a slide deck, email to your manager, or publish for your team.

A few workflows that illustrate this:

* Upload a budget spreadsheet and ask for a variance analysis comparing actual vs. planned spending. Claude calculates the variances and presents them in a formatted table with conditional highlighting.
* Describe a business process and ask for a visual. Claude generates a Mermaid flowchart as an artifact that you can iterate on until it matches reality.
* Paste in survey results and ask for a summary. Claude runs the analysis, identifies key themes and statistical breakdowns, and produces a one-page report artifact.
* Ask for a project timeline calculator. Claude builds an interactive artifact where you input start dates and durations, and it calculates milestones and deadlines.

Each of these takes one or two exchanges to produce and another one or two to refine.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Artifacts give Claude a way to deliver real outputs. Documents, diagrams, dashboards, and interactive tools appear in a dedicated panel where you can review, refine, and share them. Code execution adds precision as Claude writes and executes code to calculate, chart, and transform data rather than approximating, and can deliver results as finished files you can download and use directly. Together, they turn a conversation into a production workflow.
