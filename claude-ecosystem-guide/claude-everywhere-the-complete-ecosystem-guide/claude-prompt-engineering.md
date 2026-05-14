---
icon: claude
---

# Claude Prompt Engineering



> Explore how to design effective prompts for Claude products by understanding the five key components of a good prompt. Learn to write specific, structured, and iterative instructions that enhance AI outputs across Claude.ai, Desktop, and Cowork. This lesson helps you develop practical prompt engineering skills and create reusable prompts tailored to your workflow.

Every Claude product responds to the same underlying models. The way you write a prompt shapes the quality of what you get back, whether you are chatting in [Claude.ai](http://claude.ai/), working through Desktop with MCP tools, or delegating a task to Cowork. Some prompting habits produce consistently better results across all of them. This lesson covers practical patterns you can apply immediately: how to structure a prompt, how to iterate, how to give multi-step instructions, and what changes across products.

### The anatomy of a good prompt <a href="#the-anatomy-of-a-good-prompt" id="the-anatomy-of-a-good-prompt"></a>

A well-structured prompt has up to five components. Not every prompt needs all five, but knowing them helps you diagnose why a response fell flat. If you only have time to add one component to a bare prompt, make it context: telling Claude who you are and what you are trying to accomplish eliminates most guessing on its own.

* **Role:** Who Claude should be in this context. “You are a financial analyst reviewing quarterly earnings” focuses Claude’s perspective and vocabulary.
* **Context:** This is the background information. Claude needs to do the task well. “Our company sells B2B software. Our main competitors are X and Y. We are preparing for a board meeting next week.”
* **Task:** What you want Claude to do. “Write a one-page competitive analysis comparing our Q3 results to X and Y.”
* **Format:** How the output should be structured. “Use a table for the comparison, then two paragraphs of analysis below it.”
* **Constraints:** What Claude should avoid or prioritize. “Do not include pricing data. Focus on market share and product launches.”

Even a short prompt benefits from applying one or two components:

> You are a team lead. Summarize this document for a busy executive who needs the key decision and the main risk, nothing else.

A prompt that uses all five:

> You are a marketing strategist for a mid-market SaaS company. We are launching a new product next month targeting operations managers at companies with 200-500 employees. Write a launch announcement email. Use a subject line, a three-paragraph body, and a clear call to action. Keep it under 200 words. Avoid jargon and do not mention competitor names.

Compare that to: “Write a launch email.” Both produce output. The first produces output you can actually use.

### Specificity wins <a href="#specificity-wins" id="specificity-wins"></a>

The single biggest improvement most people can make to their prompts is being more specific. Vague prompts force Claude to guess what you want. Specific prompts remove the guessing.

* **Vague:** “Summarize this report.”
* **Specific:** “Summarize this report in three bullet points. Focus on the revenue figures, the biggest risk factor, and the recommendation the authors make in the conclusion.”
* **Vague:** “Write a meeting summary.”
* **Specific:** “Write a meeting summary for the March 12 product sync. Include: key decisions made, open questions, action items with owners, and the next meeting date. Format as bullet points under each heading. Keep it under one page.”

The specific versions take an extra 30 seconds to write. They save minutes of follow-up and revision. The pattern holds for every type of task: the more precisely you describe the output, the closer Claude gets on the first try.

### Structured output <a href="#structured-output" id="structured-output"></a>

When you need information in a specific format, ask for it explicitly. Claude follows formatting instructions well, but it defaults to prose paragraphs unless you direct it otherwise.

Useful format instructions:

* **Tables:** “Present the comparison as a table with columns for product name, price, key features, and limitations.”
* **Bullet points:** “List the action items as bullet points, each starting with the owner’s name.”
* **Numbered steps:** “Write the process as numbered steps, in the order they should be performed.”
* **Headings and sections:** “Use H2 headings for each topic area, with 2-3 sentences under each.”
* **Specific structures:** “Use this format: Problem Statement (2 sentences), Root Cause (1 paragraph), Recommended Fix (bullet list), Timeline (table).”

You can also ask Claude to output data in formats ready for other tools. “Format the results as a CSV I can paste into a spreadsheet” or “Structure this as a JSON object with keys for name, date, and status” produces output you can use directly, not just read.

### Iterative prompting <a href="#iterative-prompting" id="iterative-prompting"></a>

The first response is rarely the final version. Iterative prompting (building on Claude’s output through follow-up messages) is how most practical work gets done.

#### The refinement loop <a href="#the-refinement-loop" id="the-refinement-loop"></a>

1. **Start broad.** Give Claude the task and see what it produces.
2. **Evaluate.** Read the output. What is right? What is off?
3. **Refine.** Tell Claude specifically what to change. “The tone is too formal. Make it conversational.” “Move the recommendations to the top.” “Expand the section on risks.”
4. **Repeat** until the output matches what you need.

This works because Claude retains the full conversation context. Each follow-up builds on everything before it. You do not need to restate the original task or re-upload files. In very long conversations, early context can be deprioritized as the thread grows, so for extended multi-day work, a Project is more reliable than a single long conversation.

#### Good follow-up prompts <a href="#good-follow-up-prompts" id="good-follow-up-prompts"></a>

Effective refinements are specific about what to change and why:

* “The third paragraph is too long. Split it into two and cut the example about retail.”
* “Good structure. Now make the executive summary more direct. Lead with the recommendation, not the background.”
* “Add a row to the comparison table for customer support response time.”
* “This is close. Rewrite the conclusion to emphasize urgency rather than opportunity.”

Ineffective refinements are vague: “Make it better” or “Try again.” These leave Claude guessing the same way a vague initial prompt does.

### Multi-step instructions <a href="#multi-step-instructions" id="multi-step-instructions"></a>

For complex tasks, break the work into numbered steps within a single prompt. This gives Claude a clear sequence to follow and makes the output easier to verify.

**Example:**

> 1. Read the attached spreadsheet of customer feedback.
> 2. Categorize each piece of feedback into one of these themes: pricing, product quality, customer support, onboarding.
> 3. Count the number of entries in each category.
> 4. Identify the top 3 most common complaints within each category.
> 5. Write a one-page summary with a table of category counts and a paragraph on each of the top themes.

Claude follows the steps in order and produces output that maps to each one. This is easier to review than a single open-ended instruction like “Analyze this customer feedback and write a summary,” because you can check each step’s output against what you expected.

Multi-step prompts also work well for workflows you repeat. Save the prompt in a Project’s instructions or as a reusable template, and you have a consistent process you can run with new data each time.

### Project instructions <a href="#project-instructions" id="project-instructions"></a>

The prompt patterns above work in individual conversations. **Project instructions** (covered in an earlier lesson) let you make some of these patterns persistent.

Instead of starting every conversation with “You are a financial analyst, use tables for comparisons, keep responses under 500 words,” set those as project instructions. Every conversation in the project inherits them, and your actual prompts can focus entirely on the task.

This layering is powerful:

* Project instructions handle the role, tone, format preferences, and recurring context.
* Individual prompts handle the specific task.

The result is shorter prompts that produce better output, because the foundational context is always present.

### What changes across products <a href="#what-changes-across-products" id="what-changes-across-products"></a>

The core prompting principles (be specific, provide context, state the format, iterate) apply everywhere. But each Claude product has characteristics that affect how you prompt.

#### Claude.ai chat <a href="#claudeai-chat" id="claudeai-chat"></a>

The standard environment. All patterns above work directly. You iterate through follow-up messages, and artifacts capture the output. Best for conversational back-and-forth where you want to shape the output incrementally.

#### Claude Desktop with MCP <a href="#claude-desktop-with-mcp" id="claude-desktop-with-mcp"></a>

When Claude has access to MCP tools, your prompts can reference external resources directly. “Read the latest report in my Documents folder and summarize it” works because Claude can access the filesystem. Prompts become more action-oriented: instead of uploading a file and asking about it, you tell Claude where to find it and what to do.

> Be specific about which files or tools you want Claude to use. “Check my calendar for next week” is clear. “Look at my stuff” is not.

#### Cowork <a href="#cowork" id="cowork"></a>

Cowork prompts are task definitions, not conversation starters. They should be more complete upfront because you are delegating execution rather than iterating in real time.

A good Cowork prompt includes:

* What to produce (the deliverable).
* Where to find the inputs (which files or folders).
* How to structure the output (format, length, style).
* Any constraints or priorities.

Think of a Cowork prompt the way you would think of a brief for a colleague: detailed enough that they can do the work without coming back to ask questions every five minutes.

### Common mistakes <a href="#common-mistakes" id="common-mistakes"></a>

A few patterns consistently produce weaker results:

* **Being too vague:** “Help me with this,” makes Claude guess everything. State the task, the format, and the constraints.
* **Overloading a single prompt:** Asking Claude to “research the market, analyze the data, write the report, create a presentation, and draft the email” in one prompt leads to shallow output across all five. Claude has to distribute its attention and output across every part of the request, which means less depth on each. Break large scopes into separate tasks or multi-step instructions.
* **Not providing enough context:** Claude does not know your company, your audience, or your goals unless you say so. A sentence of context often makes the difference between generic and useful output.
* **Not iterating:** Accepting the first response and moving on wastes the most powerful part of working with Claude. Follow-ups are where generic output becomes specifically useful.
* **Restating the entire task on each follow-up:** Claude remembers the conversation. “Rewrite paragraph two to be more concise” is sufficient. You do not need to re-explain the full task.

### Try this now <a href="#try-this-now" id="try-this-now"></a>

Take a prompt you have used recently or something you typed to Claude that got an output you were not fully satisfied with, or a task you do repeatedly.

1. Write down the original prompt as-is.
2. Rewrite it using the five-component anatomy: add a role, background context, a clear task, a format instruction, and at least one constraint.
3. Run both prompts in separate conversations (same model, same uploaded files) and compare the outputs.

The difference is usually visible on the first try. If it is not, the original prompt was already well-structured, which is also a useful data point. Save the improved version somewhere reusable: a Project instruction, a text shortcut, or a note.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Good prompts have a role, context, task, format, and constraints. Specificity eliminates guessing. Structured output instructions control the format. Iteration refines the result. Multi-step instructions guide complex tasks. Project instructions make recurring patterns persistent. These principles apply across Claude.ai, Desktop, and Cowork, with adjustments for each product’s interaction model.
