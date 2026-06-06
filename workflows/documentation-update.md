# Workflow: Documentation Update

## Purpose

Create or update documentation in a controlled, targeted way — without overwriting the wrong files, inventing unsupported claims, or leaking private information into public-facing content.

This workflow exists because documentation tasks are easy to underestimate. Without a structured process, Claude can drift: rewriting sections that were not in scope, adding claims not supported by the code, adopting a promotional tone that does not match the project's style, or inadvertently including personal data, private paths, or internal details in content intended for a public audience.

Claude drafts and edits during this workflow. The human reviews the diff and decides whether to keep the changes.

---

## When to use this workflow

Use this workflow when any of the following are true:

- You are creating a new documentation file (README, guide, reference, workflow, template).
- You are updating an existing documentation file with new information.
- You want to add or revise a section in an existing document.
- You are preparing documentation for a public repository or portfolio.
- You are writing internal documentation that should not leak private details.
- A Developer or Architect role just completed a task and documentation needs updating to match.

Do not use this workflow for code comments, inline documentation, or docstrings — use the Developer role for those. Do not use this workflow if the goal is to review whether documentation is accurate; use the Analyst role for that.

---

## Recommended role

**Documentation Developer**

This role drafts and edits documentation. It does not design architecture, write code, run tests, or make strategic decisions. Keep this role active for the entire duration of this workflow.

---

## Allowed actions

- Read files explicitly listed in the prompt scope.
- Read files directly referenced by those files if context is needed to write accurately.
- Create a new documentation file if explicitly listed as a permitted output.
- Edit sections of an existing documentation file if explicitly listed as a permitted output.
- Run `git diff -- [filename]` after an edit to show what changed.
- Report findings, flag concerns, and recommend the next step.

---

## Forbidden actions

- Do not edit any file not explicitly listed in the permitted output.
- Do not rewrite large sections of an existing document without explicit human approval.
- Do not add claims, features, or behaviors that are not confirmed in the files you have read.
- Do not invent examples, commands, or output samples that have not been verified.
- Do not add screenshots, images, or binary files.
- Do not include personal data, private paths, credentials, or internal-only details.
- Do not adopt portfolio or promotional wording unless explicitly requested.
- Do not scan the whole repository.
- Do not use 1M context.
- Do not commit or push.
- Do not delete any file.
- Do not continue past a safety gate without explicit human permission.

---

## Documentation workflow steps

Follow these steps in order. Stop at each gate before continuing.

### Step 1 — Understand the documentation goal

Before reading any file, confirm with yourself (or ask the human if unclear):

- What is the documentation for? (A new file, an updated section, a corrected description.)
- What is the intended output? (File name, section name, approximate length.)
- What is the documentation type? (See Step 2.)
- Who is the intended audience? (See Step 3.)

If the goal is ambiguous, stop and ask before reading any files or writing anything.

**Gate:** If the goal, output, or audience is unclear, ask for clarification before proceeding.

---

### Step 2 — Identify the documentation type

Determine which type applies and keep it in mind throughout all subsequent steps.

**Creating new documentation:**
There is no existing file to preserve. The structure and tone must be established from scratch, guided by the audience and the information available in the inspected files. Do not import structure from unrelated documents without checking it fits.

**Updating existing documentation:**
An existing file already has an established tone, structure, and scope. The goal is to extend or correct it — not replace it. Preserve the existing structure unless the human explicitly asks for a restructure.

**Public-facing documentation:**
The content will be readable by anyone with access to the repository. Apply the public wording checks (see the Public Wording Checks section) and the private-data checks (see the Private-Data Checks section) to every sentence before finalising.

**Private or internal documentation:**
The content is intended for the project owner or a small internal team. Private-data checks still apply — internal documents should not contain secrets or credentials — but public wording checks are not required.

**Gate:** State the documentation type at the start of the report. If the public or private status is unclear, ask before proceeding.

---

### Step 3 — Identify the intended audience

Who will read this documentation?

- **Project owner (personal project):** Concise and practical. Assumes familiarity with the project. No need to explain basics the owner already knows.
- **Developer audience (open source or team):** Clear, technical, and precise. Assumes software engineering knowledge. Explain project-specific decisions; do not explain general programming concepts.
- **Non-technical audience:** Plain language. No jargon without explanation. Focus on what the thing does and why it matters, not how it works internally.
- **Portfolio or public audience:** Professional, accurate, and honest. Does not exaggerate scope or capability. Does not use marketing language unless the human has explicitly requested it.

Keep the identified audience in mind when choosing vocabulary, level of detail, and structure.

**Gate:** State the intended audience in the report before writing.

---

### Step 4 — Inspect relevant existing files

Read only the files listed in the prompt scope. Read them carefully before writing anything.

For each file, note:
- What it already covers.
- What it does not cover.
- Its tone, section structure, and vocabulary style.
- Any conventions it follows (e.g. heading format, table format, list style, link format).

If context files are listed (files to read for background, not to edit), read them and extract only what is needed for the writing task. Do not reproduce large portions of a context file verbatim.

Do not read files beyond the listed scope unless a listed file directly references them and you need that reference to write accurately.

**Gate:** Report the files inspected and the key conventions observed before writing.

---

### Step 5 — Check what is already documented

Before writing, identify what already exists that is relevant to the task:

- Is the topic already covered in the target file? If so, is this an addition or a correction?
- Is the topic covered in a different file that the target file should link to, rather than duplicate?
- Are there any contradictions between the existing documentation and what you are about to write?
- Are there section headings, tables, or references in the existing file that the new content must fit around?

For an update task: do not duplicate content that already exists. Do not add a new section for something already covered under a different heading.

For a creation task: check context files for the conventions the new file should follow. Do not invent a structure that conflicts with the existing framework style.

**Gate:** Report what is already documented on this topic and whether the new content adds to, corrects, or replaces it.

---

### Step 6 — Draft or update the documentation

Write the content. Apply the following rules throughout:

**Accuracy first:**
Only write what is confirmed by the files you have read. If a claim requires verification you cannot perform (e.g. "this command works on all platforms"), state it as an assumption or omit it. Do not fill gaps with plausible-sounding content.

**Preserve existing tone and structure:**
For update tasks, match the voice, heading style, list format, and link format of the existing file. Do not introduce a new style unless asked.

**Avoid invented examples:**
Commands, file paths, output samples, and code snippets must reflect what the files you have read actually show. Do not construct examples from general knowledge if they could be wrong for this specific project.

**No portfolio-first wording by default:**
Do not write promotional, achievement-framing, or self-marketing language unless the human has explicitly requested it for a portfolio or CV context. Plain, accurate descriptions are the default.

**Minimal scope:**
Write only what was asked for. Do not add sections, improve adjacent sections, or extend scope without explicit approval.

**Gate:** Before writing a large new section (more than approximately 10 lines) or rewriting an existing section, briefly describe the planned structure and ask for approval. For small additions or corrections, proceed and show the diff.

---

## Source verification rules

Apply these rules to every claim before including it in the documentation.

**Verified fact:** A statement directly supported by the content of a file you have read in this session. Cite the source file if there is any ambiguity.

**Assumption:** A statement inferred from the files you have read but not directly confirmed. Label it as an assumption in the report. Do not include unverified assumptions in public-facing documentation without flagging them for human review.

**Unsupported claim:** A statement that cannot be traced to any file you have read. Do not include it. If it seems important, flag it as an open question in the report.

**Gate:** If the task requires including a claim you cannot verify from the inspected files, stop and ask the human to confirm it before including it.

---

## Public wording checks

Apply these checks to all content destined for a public repository, portfolio, or public-facing page.

**Capability claims:**
Does the documentation claim the project does something it does not actually do? Check against the inspected files. Remove or qualify any claim that overstates scope, reliability, or completeness.

**Status claims:**
Does the documentation imply a feature is complete, tested, or production-ready when it is not? Use accurate status language: "planned", "in progress", "validated on low-risk tasks only", "not yet tested in production", as appropriate.

**Internal references:**
Does the documentation mention internal URLs, staging domains, internal codenames, intranet paths, or organisation-specific tooling that is not appropriate for a public audience? Remove or generalise them.

**Tone:**
Is the wording professional, accurate, and neutral? Remove marketing superlatives ("powerful", "seamless", "best-in-class") unless the human has explicitly requested them.

**Gate:** Flag any public wording concern before finalising. Do not publish content that overstates the project's capabilities or status.

---

## Private-data checks

Apply these checks to all documentation, public or internal.

Check the draft for:

- Real email addresses belonging to identifiable people (not placeholder addresses such as `user@example.com`).
- Phone numbers in any format.
- Physical addresses or GPS coordinates linked to a person.
- Full names combined with contact details, identification numbers, or sensitive context.
- Local filesystem paths that reveal a username or machine name (e.g. `/Users/yourname/`, `C:\Users\yourname\`). Generalise to `~/` or `<project-root>/` instead.
- Credentials, API keys, tokens, passwords, or secrets of any kind.
- CV content, employment history, salary figures, job application details, or interview notes.
- Private financial information.
- Screenshots or image files containing any of the above (flag the request; do not add images without human review).
- Internal company names or project codenames not intended for public disclosure.

**Gate:** If any of the above is found in the draft, remove or redact it and note what was removed in the report. Do not include it even in internal documentation.

---

## Output format

Always produce the report in this format after completing the documentation work. Do not omit sections.

```
Documentation type:
- [new / update]
- [public-facing / private or internal]

Intended audience:
- [description]

Files inspected:
- [list each file read]

Files created or edited:
- [list each file written, with a one-line description of what changed]

Verified facts used:
- [key claims in the documentation, with source file for each]

Assumptions made:
- [anything inferred but not directly confirmed — label each one ASSUMPTION]

Open questions:
- [anything that could not be determined from the inspected files]

Public wording concerns:
- [any issues found and how they were resolved, or "none"]

Private-data concerns:
- [any issues found and how they were resolved, or "none"]

Diff summary:
- [run git diff -- [filename] and paste the output, or summarise the key additions and removals]

Pre-commit review recommended:
- [Yes / No — reason]

Human approval required before:
- [list what the next step requires — e.g. "committing", "reviewing the draft of section X", "verifying the command example"]
```

---

## Example prompt

Copy this prompt, fill in the bracketed sections, and paste it into Claude Code.

```
AI Development OS is active.

ROLE:
Documentation Developer

Goal:
[Create / Update] [file name or section name] — [one sentence describing what it should contain].

Task type:
Documentation

Context:
[Describe the audience, the purpose of the document, and what already exists.
For an update: describe which section needs changing and why.
For a new file: describe what it should cover and what style it should follow.]

Documentation type:
[new file / update to existing file]
[public-facing / private or internal]

Inspection scope:
Before writing, inspect only:
- [list files Claude should read for context — not for editing]

Do not read unrelated files.
Do not scan the whole repository.
Do not use 1M context.

Permissions:
You may [create / edit] only:
- [exact file name]

Do not edit:
- [list files that must not be touched, especially existing framework files]
Do not commit automatically.

Rules:
- Only include claims confirmed by the files you read.
- Label assumptions clearly.
- Match the existing tone and structure.
- Do not add sections beyond what is requested.
- Do not use portfolio or promotional wording unless I ask for it.
- Check for private data, local paths, and internal references before finalising.

After the work:
Run: git diff -- [filename]
Run: git status --short
Do not commit.

Output only:
- documentation type and audience
- files inspected
- files created or edited
- verified facts used
- assumptions made
- open questions
- public wording concerns
- private-data concerns
- diff summary
- whether a pre-commit review is recommended
- what requires human approval before continuing
```
