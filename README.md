# unslop-kb

An Antigravity / Google Gemini skill to transform, de-slop, and format F5 Support Knowledge Base (KCS) articles into clean, production-ready Salesforce HTML and generate standardized F5 titles.

---

## What is `unslop-kb`?

When authoring or rewriting F5 Knowledge Base articles with LLMs, three major problems typically occur:

1. **The "Echo Chamber" Slop:** LLMs repeat the exact same point in the *Description*, the *Cause*, and again as a summary in *Additional Information*.
2. **Vague Environment Bullet Fillers:** LLMs add generic bullets like *"use case where catch-all DNS is needed"* or *"wildcard DNS handling"*.
3. **The MyF5 Code-Block Rendering Glitch:** In MyF5 (Salesforce Lightning Knowledge), the CMS wraps any standard `<pre>` or `<code>` tag in a heavy, full-width boxed widget with copy buttons. This creates oversized, broken-looking visual boxes across the entire screen for simple CLI commands and inline text.

`unslop-kb` fixes all of this in a single pass.

---

## Features

- 🧹 **Strict Technical De-slopping:** Enforces the core de-slop contract adapted specifically for technical support: removes repetitive codas, strips rhetorical framing, and deletes filler bullets.
- 🔒 **Zero Hallucination / Strict Preservation:** Preserves all error codes (`01070734:3`), daemons (`mcpd`), versions, CLI commands, and paths byte-for-byte.
- ⚡ **Native MyF5 Salesforce HTML:** Produces clean HTML without `<pre>` or `<code>` tags, formatting CLI commands and logs as clean monospace paragraphs (`<p style="margin-left: 20px; font-family: monospace; color: #333;">...`) and keywords with `<b>`.
- 🏷️ **Standard F5 Title Generation:** Automatically proposes title candidates in standard F5 Knowledge formats (Error message, Symptom/Trigger, and Cause-based).

---

## Installation Guide

### Option 1: Web Browser / Google Gemini Enterprise (No Terminal Required)

If you use the browser-based Gemini interface:

1. **Download the Skill File:**
   - Go to the [**Latest Release**](https://github.com/jesse-wahl/unslop-kb/releases/latest).
   - Click on [`SKILL.md`](https://raw.githubusercontent.com/jesse-wahl/unslop-kb/main/SKILL.md) to download it to your computer (or right-click and choose **Save Link As...**).
2. **Open Gemini in your browser:**
   - Log into your Gemini Enterprise / F5 Gemini workspace.
3. **Navigate to Skills:**
   - In the left sidebar, click on **Skills**.
4. **Upload the Skill:**
   - Click the **`+`** (plus) icon at the top of the Skills panel.
   - From the menu, select **Upload skill**.
   - Choose the `SKILL.md` file you downloaded in Step 1.
5. **Done!** The `unslop-kb` skill is now active and ready in your chat.

---

### Option 2: Command Line / Antigravity IDE (Global Config)

To install globally via CLI across all local coding sessions:

1. Create the skill folder:
   ```bash
   # Windows (PowerShell)
   mkdir -p "$HOME\.gemini\config\skills\unslop-kb"

   # macOS / Linux
   mkdir -p ~/.gemini/config/skills/unslop-kb
   ```

2. Copy [`SKILL.md`](SKILL.md) into that folder:
   - **Windows:** `C:\Users\<your-user>\.gemini\config\skills\unslop-kb\SKILL.md`
   - **macOS / Linux:** `~/.gemini/config/skills/unslop-kb/SKILL.md`

---

### Option 3: Workspace / Team Repository

To include the skill directly inside a project repository for your entire team:

1. Copy the skill to `.agents/skills/unslop-kb/SKILL.md` in your project root:
   ```bash
   mkdir -p .agents/skills/unslop-kb
   cp SKILL.md .agents/skills/unslop-kb/
   ```

---

## How to Use

Once installed, simply invoke the skill in your chat:

```text
/unslop-kb <paste your raw draft, notes, or AI-generated article here>
```

Or ask naturally:
> *"Use unslop-kb on this draft: ..."*

### Example Workflow

**Input:**
```text
/unslop-kb
Description: BIG-IP DNS fails when creating Wide IP with wildcard name...
Environment: BIG-IP DNS 17.1.3, wildcard handling
Cause: tmsh does not allow wildcard in object identifier.
Recommended Actions: Create with FQDN and add alias: tmsh create gtm wideip aaaa example.com aliases add { *.example.com } failure-rcode-response enabled
Additional Information: Wildcard should not be used as object name.
```

**Output:**
1. **Clean Source Code (MyF5 HTML):**
   ```html
   <h2>Description</h2>
   <p>When attempting to create a BIG-IP DNS Wide IP using a wildcard in the object name...</p>
   <p style="margin-left: 20px; font-family: monospace; color: #333;">Syntax Error: the "create" command does not accept wildcard configuration identifiers.</p>

   <h2>Environment</h2>
   <ul>
     <li>BIG-IP DNS 17.1.3</li>
     <li>Wildcard DNS Wide IP configuration</li>
   </ul>

   <h2>Cause</h2>
   <p>The <b>tmsh</b> utility does not allow wildcard characters in the Wide IP object name...</p>

   <h2>Recommended Actions</h2>
   <p>Create the Wide IP with a standard Fully Qualified Domain Name (FQDN) and add the wildcard entry to the <b>aliases</b> list:</p>
   <p style="margin-left: 20px; font-family: monospace; color: #333;">tmsh create gtm wideip aaaa example.com aliases add { *.example.com } failure-rcode noerror failure-rcode-response enabled</p>

   <h2>Additional Information</h2>
   <p>None</p>
   ```

2. **Title Suggestions:**
   - **Error Message:** `Error Message: Syntax Error: the "create" command does not accept wildcard configuration identifiers`
   - **Symptom / Trigger:** `tmsh syntax error when creating a BIG-IP DNS Wide IP with a wildcard name`
   - **Task / Solution:** `Configuring a wildcard Wide IP using aliases in BIG-IP DNS`

---

## License

[MIT](LICENSE)
