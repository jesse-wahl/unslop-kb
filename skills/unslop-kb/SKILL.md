---
name: unslop-kb
description: Transform, de-slop, and format F5 / MyF5 Knowledge Base articles into clean, production-ready HTML (preventing broken Salesforce code-block widgets) and suggest standard F5 KB titles.
user-invocable: true
argument-hint: "[article text]"
---

# F5 Knowledge Base (MyF5) Article Formatter & De-slopper

Transform raw, drafted, or AI-generated F5 Support Knowledge Base articles into concise, highly readable, production-ready content formatted specifically for the MyF5 (Salesforce Lightning Knowledge) CMS.

## Core De-Slop Contract & Rules

Follow the strict de-slopping principles adapted from the unslop core contract for technical writing:

1. **Anti-Filler & Zero Redundancy:**
   - **Eliminate the "Echo Chamber":** Do not restate facts across multiple sections. If the *Cause* explains the issue, *Additional Information* must not re-summarize it. Set *Additional Information* to "None" unless there are genuine caveats, edge cases, or RCA/diagnostic steps.
   - **Purge Vague Bullets:** Replace vague noun phrases in *Environment* (e.g., *"use case where X is needed"*) with concrete technical conditions or remove them.
   - **Delete Conversational / Rhetorical Framing:** Remove phrases like *"It should be noted that"*, *"Users may observe that"*, *"In order to configure"*, *"This config allows..."*.
   - **Plain Verb Substitution:** Use direct, active technical verbs (*fails*, *returns*, *rejects*, *requires*).

2. **Strict Technical Preservation:**
   - **Preserve Facts & Identifiers Byte-for-Byte:** Never alter error strings, error codes (`01070734:3`), log facilities (`mcpd`), daemon names, product names, versions, or CLI syntax.
   - **No Hallucinated Parameters:** Never add CLI flags, parameters, or advice not present or directly implied by the source.
   - **Force-Bearing Words:** Preserve *"must"*, *"never"*, and *"only"* when they represent technical or security constraints.

3. **Standard F5 KCS Structure:**
   Every article must follow the canonical 5-section layout:
   - `<h2>Description</h2>` (What happens + exact error output or symptom)
   - `<h2>Environment</h2>` (Clean bullet list of product, versions, modules, prerequisites)
   - `<h2>Cause</h2>` (Underlying mechanism / reason for behavior)
   - `<h2>Recommended Actions</h2>` (Numbered imperative procedures with exact commands)
   - `<h2>Additional Information</h2>` ("None" or unique reference notes)

## Native MyF5 HTML Source Code Rules

Follow these strict HTML rules to prevent MyF5's Salesforce theme rendering bugs:
- **STRICTLY NO `<pre>` OR `<code>` TAGS:** The MyF5 / Salesforce theme renders `<pre>` and `<code>` tags as oversized, full-width container boxes with copy widgets, which distorts single-line commands and inline text.
- **Inline Code/Keywords/Terms:** Use `<b>...</b>` instead of `<code>...</code>` (e.g., `<b>tmsh</b>`, `<b>aliases</b>`, `<b>Pending</b>`, `<b>qkview</b>`).
- **CLI Commands, Log Snippets & Error Messages:** Format standalone commands and error blocks as indented monospace paragraphs:
  `<p style="margin-left: 20px; font-family: monospace; color: #333;">command or error output</p>`
- **Lists:** Use standard `<ul>` and `<ol>` tags with clean `<li>` items (no nested `<p>` tags inside `<li>`).
- **Subheadings:** Use `<h3>` inside sections (such as under Recommended Actions for sub-procedures).

## Title Suggestions

Provide 2–3 high-quality title options following standard F5 Knowledge naming conventions:
- **Error Message Format:** `Error Message: <error or status string>`
- **Symptom / Trigger Format:** `<Symptom/Behavior> when <trigger or condition>`
- **Task / Cause Format:** `<Component/Module> fails to <action> due to <cause>`

## Output Format

Always output:
1. The **Clean Source Code (MyF5 HTML)** inside a copyable `html` code block.
2. The **Title Suggestions** section immediately following.
