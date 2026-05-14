---
icon: claude
---

# Document and File Analysis with Claude

> Explore how to use Claude.ai for advanced document and file analysis. Learn to manage multiple files, extract structured data from unstructured documents, analyze images, compare and synthesize across documents, and handle very long files effectively. This lesson equips you to orchestrate document workflows that enhance productivity and accuracy with Claude's AI capabilities.

Uploading a single file and asking Claude to summarize it is straightforward. This lesson goes further. You will learn how to work with multiple files at once, pull structured data out of messy documents, read and interpret images, and handle documents that are too long to fit in a single conversation.

### Conversations vs. Projects: Where to upload <a href="#conversations-vs-projects-where-to-upload" id="conversations-vs-projects-where-to-upload"></a>

Before uploading a file, decide where it belongs. The choice affects how long the file stays available and how many conversations can access it.

|                  | **Upload to a Conversation**                        | **Upload to a Project**                                   |
| ---------------- | --------------------------------------------------- | --------------------------------------------------------- |
| **Availability** | Only in that conversation                           | Every conversation in the project                         |
| **Best for**     | One-time analysis, quick questions about a document | Reference material you return to repeatedly               |
| **Example**      | Summarize this meeting transcript.                  | Client brand guidelines, quarterly report templates, SOPs |
| **Persistence**  | Gone when the conversation ends                     | Stays until you remove it                                 |

The rule of thumb: if you will reference the file more than once, put it in a Project. If it is a one-off task, upload it directly into the conversation.

### Multi-file analysis <a href="#multi-file-analysis" id="multi-file-analysis"></a>

Claude can work across multiple files uploaded in the same conversation. This is where document workflows become genuinely useful, because the value is not in summarizing any single file but in finding patterns, differences, and connections across several.

#### Comparing documents <a href="#comparing-documents" id="comparing-documents"></a>

Upload two or more files and ask Claude to compare them. Claude reads all files, identifies the relevant dimensions, and presents the comparison in a structured format.

**Practical examples:**

* **Vendor proposals:** Upload three competing proposals and ask Claude to create a comparison matrix covering pricing, scope, timeline, and key differentiators.
* **Contract versions:** Upload a draft and a final version of a contract and ask Claude to identify every change, addition, and deletion.
* **Research papers:** Upload several papers on the same topic and ask Claude to synthesize the findings, noting where they agree and where they diverge.

#### Synthesizing across files <a href="#synthesizing-across-files" id="synthesizing-across-files"></a>

Comparison is one pattern. Synthesis is another. Instead of asking “how are these different?” you ask “what do these tell me together?”

* Upload meeting notes from the past month and ask Claude to identify recurring themes, unresolved action items, and shifting priorities.
* Upload a budget spreadsheet and a project plan, and ask Claude to flag any line items that are not accounted for in the budget.
* Upload customer feedback from multiple sources (surveys, support tickets, review excerpts) and ask Claude to categorize the feedback by theme and sentiment.

Claude treats all uploaded files as part of the same context. It can reference specific sections of one document while answering questions about another. File count and total size limits apply per conversation; check [Claude.ai](http://claude.ai/) for current limits before building a large multi-file workflow.

### Extraction workflows <a href="#extraction-workflows" id="extraction-workflows"></a>

Some of the most practical file work is not analysis but extraction: pulling structured data out of unstructured documents. Claude is effective at reading documents that were designed for humans (contracts, invoices, reports) and turning their contents into clean, structured output.

Here are some common extraction patterns:

* **Contracts:** Upload a PDF and ask Claude to extract all dates, obligations, payment terms, termination clauses, and parties into a table.
* **Invoices:** Upload a batch of invoices and ask Claude to pull vendor name, invoice number, amount, and due date into a spreadsheet-ready format.
* **Reports:** Upload an annual report and ask Claude to extract key financial metrics, executive summary points, and risk factors.
* **Speaker or contributor bios:** Upload several bios and ask Claude to create a comparison table by background, expertise, and relevant highlights.

The pattern is consistent: upload the document, describe the fields you want, and specify the output format (table, bullet list, CSV). Claude reads the document, locates the relevant information, and structures it.

> Scanned PDFs (documents that were photographed or photocopied rather than created digitally) are processed as images rather than text. Extraction from scanned documents is less reliable than from text-based PDFs and may miss fields or misread values. If accuracy is important, verifying the output against the original is always a good idea.

#### Improving extraction accuracy <a href="#improving-extraction-accuracy" id="improving-extraction-accuracy"></a>

A few techniques make extraction more reliable:

* **Name the fields explicitly:** “Extract the payment terms” is good. “Extract the payment amount, due date, late penalty, and accepted payment methods” is better. The more specific you are about what you want, the less Claude has to guess.
* **Provide an example:** If you want a particular table structure, describe it or show the first row. Claude will follow the pattern.
* **Ask Claude to flag uncertainty:** Add “If any field is ambiguous or not found, note it rather than guessing.” This prevents Claude from filling gaps with plausible but incorrect information.

### Image analysis <a href="#image-analysis" id="image-analysis"></a>

Claude reads images the same way it reads documents. We upload the file and ask questions about it. This applies to photos, screenshots, charts, diagrams, and handwritten notes.

Here are some things that work well:

* **Screenshots:** Upload a screenshot of a dashboard, error message, or application interface. Claude reads the text and visual elements and can answer questions, identify issues, or describe what it sees.
* **Charts and graphs:** Upload a chart from a report. Claude interprets the data, describes the trends, and can extract approximate values from the visual.
* **Whiteboard photos:** Upload a photo of a whiteboard from a meeting. Claude transcribes the text, interprets diagrams, and organizes the content into structured notes.
* **Handwritten notes:** Upload a photo of handwritten notes. Claude reads the handwriting and produces a clean, typed version. For best results, use good lighting, dark ink on a white background, and print lettering rather than cursive where possible.
* **Receipts and forms:** Upload a photo of a receipt or filled-in form. Claude extracts the relevant fields.

Claude analyzes images based on what is visually present. It reads text in images accurately in most cases, but very small text, poor lighting, or heavy distortion can reduce accuracy. For charts, Claude can identify trends and approximate values but may not extract precise data points. If you need exact numbers from a chart, ask Claude to estimate and flag where it is uncertain, or provide the underlying data as a spreadsheet alongside the image.

### Handling long documents <a href="#handling-long-documents" id="handling-long-documents"></a>

Most documents fit comfortably within Claude’s context window. But when you are working with very long files (a 200-page report, a full regulatory filing, a book-length document), you may approach the limit. Here are strategies for working effectively with large files.

* **Break it into parts:** If a document is too large for a single conversation, split it into logical sections and upload each section into its own separate conversation. Ask Claude to analyze each section, then use a final conversation or a Project to bring the findings together. Note that uploading sections across multiple messages in the same conversation does not help — all messages share the same context window.
* **Use Projects for persistent access:** Upload the full document to a Project’s knowledge base. The Project’s 200K token context window gives you more room than a single conversation, and every conversation in the project can access the document. This lets you ask different questions across multiple conversations without re-uploading.
* **Ask focused questions:** Long documents strain the context window most when your questions are broad. “Summarize this entire report” forces Claude to hold the full document in active context. “What does Section 4 say about supply chain risks?” lets Claude focus on the relevant section. Targeted questions work better with large files than sweeping ones.
* **Combine with extraction:** For very long documents, a two-pass approach works well. First, ask Claude to extract a structured overview (table of contents, key sections, important figures). Then use that overview to identify which sections you want to explore in depth. This way, you get a map of the document first and dive into the details selectively.

### Try this now <a href="#try-this-now" id="try-this-now"></a>

Find a contract, invoice, report, or any structured document from your current work.

1. Upload it to a new [Claude.ai](http://claude.ai/) conversation.
2. Name the specific fields you want extracted. For a contract: “Extract the parties, effective date, payment terms, termination clause, and renewal conditions.” For an invoice: “Extract the vendor name, invoice number, line items, total amount, and due date.”
3. Add to your prompt: “If any field is ambiguous or not found, note it rather than guessing.”
4. Review the output. If a field is missing or wrong, follow up with a more specific instruction: “The payment terms are in Section 4 under ‘Billing.’ Try again.”

The goal is to get comfortable with the field-naming technique and the uncertainty flag. Both carry over to any document type you work with.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Working with files is where Claude moves beyond conversation and into practical document work. Multi-file analysis lets you compare, contrast, and synthesize across documents. Extraction workflows turn unstructured files into clean, structured data. Image analysis reads screenshots, charts, and handwritten notes. And with the right strategies, even very long documents become manageable. The key is matching the approach to the task: upload to a conversation for one-off analysis, upload to a Project for recurring reference, and break large files into focused pieces when the context window gets tight.
