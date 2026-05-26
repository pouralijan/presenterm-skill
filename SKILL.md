---
name: presenterm
description: Create and update presenterm-native markdown presentations with YAML frontmatter, slide separators, speaker notes, pauses, and executable demo blocks.
license: MIT
compatibility: opencode
metadata:
  format: presenterm
  markdown: native
---

# Presenterm Skill

This skill handles both creating new presentations and editing existing ones. Load it with `/presenterm` (defaults to create mode) or with parameters for specific modes.

## Parameters

Pass via `user_message` when loading the skill:

| Parameter | Behavior |
|---|---|
| `--create` (default) | Create a new presentation from scratch |
| `--update` | Edit an existing presentation |

`/presenterm` without arguments defaults to `--create`.

---

# Common Rules (Both Modes)

These rules apply whether creating or updating a presentation.

## Required Frontmatter

Every presentation file must start with YAML frontmatter:

```yaml
---
title: Presentation Title Here
author: Your Name
options:
    h1_slide_titles: true
---
```

- `options.h1_slide_titles: true` — presenterm uses `<h1>` headings as slide titles
- No trailing spaces in frontmatter

## Slide Structure

Each slide is separated by `<!-- end_slide -->` on its own line with blank lines before and after.

### Slide 1: Title/Cover Slide

```markdown
# Main Title

<!-- speaker_note: |
  Speaker notes for the title slide. Explain what this presentation covers.

  Estimated time in brackets: [2 minutes]
-->

*Subtitle or tagline*

Author Name


<!-- end_slide -->
```

### Content Slides

Use heading level (`<h1>`) for slide title, then content below:

```markdown
# Slide Title

<!-- speaker_note: |
  Speaker notes — what to say during this slide. Never leave empty.

  Write complete narration. Cover:
  - Key point to emphasize
  - Context or background
  - What the audience should take away

  End with time estimate: [3 minutes]
-->

**Bold for key term** — explanation follows em dash

- Bullet points with **bold highlights**
- Each point on its own line
- Tables, lists, and code blocks are supported
```

## Tables

Tables **must** use standard GFM (GitHub Flavored Markdown) pipe syntax. Every table requires exactly three parts: a header row, a separator row, and data rows. **No other table format works in presenterm.**

```markdown
| Header 1 | Header 2 | Header 3 |
|---|---|---|
| Cell A   | Cell B   | Cell C   |
```

**CRITICAL RULES:**
- Start every row with `|` and end with `|` — using only the ASCII pipe character (`|`, U+007C)
- The second line **must** be a separator row containing only `|`, `-`, and `:` (e.g. `|---|---|---|`)
- You must have exactly one separator row — presenterm will not render a table without it
- **NEVER** use Unicode box-drawing characters (`│`, `┼`, `─`, `├`) — presenterm does not support them
- **NEVER** align columns with spaces and pipes without a separator row — this is plain text, not a table

## Speaker Notes Rules

- Every slide MUST have a `<!-- speaker_note: |` block — even if brief
- The `|` after the colon is REQUIRED — it marks a YAML literal block
- Start each line inside the note with two spaces
- End each note block with `-->` on its own line
- Include a **time estimate** in brackets at the end: `[2 minutes]`
- Count the `<!-- pause -->` markers on the slide and include that number as a **pause count** on the line after the time estimate. For example, 2 pause markers → `[2 pauses]`.
- Speaker notes are published with `presenterm -P` (uppercase P)

## Incremental Reveals

Use `<!-- pause -->` to reveal content progressively within a single slide:

```markdown
# Title

<!-- speaker_note: |
  Walk through each point one by one. Let the audience absorb before advancing.

  [2 minutes]
  [2 pauses]
-->

First point shown immediately.

<!-- pause -->

Second point revealed on next key press.

<!-- pause -->

Third point revealed.
```

## Executable Demo Blocks — One Command Per Slide

For live demo commands, use the `+exec` attribute on fenced code blocks:

````markdown
```bash +exec
ls -la
```
````

**STRICT RULE — NEVER VIOLATE: Each executable command MUST be on its own dedicated slide.** NEVER put multiple `+exec` blocks in the same slide. NEVER bundle commands. If there are N demo commands, there must be N separate demo slides, each ending with `<!-- end_slide -->`.

Each command-slide must have:
- A descriptive `<h1>` title explaining what the command does
- A `<!-- speaker_note: |` block with narration
- Exactly one `+exec` code block
- A trailing `<!-- end_slide -->`

Users execute each with `Ctrl+E`. Output is stateful (persists across slides).

## Tools Not Supported by Presenterm

- **NO** `{.annotations}` or `{.custom-class}` on code blocks — presenterm ignores them
- **NO** diagrams inside tables — presenterm doesn't render them
- **NO** raw HTML beyond the specific comment directives above

## Typical Slide Deck Structure

| # | Slide | Purpose |
|---|-------|---------|
| 1 | Title/Cover | Presentation name, author |
| 2..N-2 | Content | One concept per slide |
| N-1 | Summary | Key takeaways, quick reference table |
| N | Q&A | Open questions, resources, contact |

## Running the Presentation

- `presenterm file.md` — normal mode, navigate with arrow keys
- `presenterm -P file.md` — **publish** speaker notes to a browser window (uppercase P)
- `presenterm -l file.md` — **listen** mode (auto-advance slides)
- `presenterm -x file.md` — enable **exec** (Ctrl+E to run +exec blocks)
- `Ctrl+E` — execute a `+exec` code snippet
- `Ctrl+R` — reload presentation / clear exec state

---

# Create Mode (`--create`, default)

When loading this skill without a parameter or with `--create`, follow these instructions to create a new presentation.

## Create from Scratch

Create a complete `.md` file following the Common Rules above. Every element must be in presenterm-native markdown format — no substitutions, no plain markdown.

### Demo Slide Examples

Example — two commands = two slides:

```markdown
# List Directory Contents

<!-- speaker_note: |
  Let's see what's in the current directory. This lists all files and directories with details.

  [30 seconds]
-->

```bash +exec
ls -la
```


<!-- end_slide -->

# Check Disk Usage

<!-- speaker_note: |
  Now let's check the filesystem. We'll look at disk space usage across mounted volumes.

  [30 seconds]
-->

```bash +exec
df -h
```


<!-- end_slide -->
```

## Output File

- Save to the current working directory
- Name: `<topic>-<subtopic>.md` using kebab-case
- Use `.md` extension
- Always write to a file — not stdout

---

# Update Mode (`--update`)

When loading this skill with `--update`, follow these instructions to edit an existing presentation.

## Read First, Edit Second

Always read the full file before making any changes. Understand the slide structure, the flow, and the surrounding content before editing.

## Core Invariants (Must Never Break)

1. Every slide ends with `<!-- end_slide -->` on its own line — never leave a slide unterminated
2. Every slide has a `<!-- speaker_note: |` block — never remove or leave empty
3. The `|` after `speaker_note:` is always present — never change it to a colon or omit it
4. `h1_slide_titles: true` is respected — each `<h1>` is a slide title
5. Exec `+exec` blocks are never combined — still one per slide

## Operations

### 1. Insert a New Slide

Identify the anchor — the existing slide **after** which the new slide goes. Find its `<!-- end_slide -->` boundary.

```markdown
# Existing Slide Title

<!-- speaker_note: |
  Existing content.
-->

Some content

<!-- end_slide -->

# New Slide Title       ← insert here

<!-- speaker_note: |
  Speaker notes for the new slide.

  [2 minutes]
-->

New slide content here.


<!-- end_slide -->
```

**DO NOT** insert inside an existing slide's boundary. The new slide must be a complete, standalone block between `<!-- end_slide -->` markers.

### 2. Insert a Slide at the Beginning (After Title Slide)

Insert right after the title slide's `<!-- end_slide -->`. The title slide is the first slide.

### 3. Insert a Slide at the End (Before Q&A)

Insert before the final Q&A slide (if one exists). Q&A should remain the last slide.

### 4. Modify an Existing Slide

Find the slide by its `<h1>` heading text. Replace content **between** the `# Heading` line and the `<!-- end_slide -->` line.

Keep:
- The `<h1>` heading (unless directed to rename)
- The `<!-- speaker_note: | ... -->` block (update the narration inside, not the wrapper)
- The `<!-- end_slide -->` terminator

```
BEFORE:
# Old Topic          ← keep heading unless renaming
                      ← keep speaker_note block
<!-- speaker_note: |   ← keep wrapper
  Old narration.       ← replace content inside
-->                    ← keep close
Old content.           ← replace content

<!-- end_slide -->    ← keep terminator

AFTER:
# Old Topic
<!-- speaker_note: |
  Updated narration.

  [3 minutes]
-->
Updated content.


<!-- end_slide -->
```

### 5. Add an Exec Demo Slide

Find the spot where the demo belongs. Insert a complete demo slide:

```markdown
# List Directory Contents

<!-- speaker_note: |
  Let's see what's in the current directory. This shows files and directories with details.

  [30 seconds]
-->

```bash +exec
ls -la
```


<!-- end_slide -->
```

Remember: one `+exec` block per slide. If the user asks for multiple demo commands, create multiple slides.

### 6. Remove a Slide

Delete everything from the `<h1>` heading through its `<!-- end_slide -->` terminator. Include the trailing blank line.

**Never leave two `<!-- end_slide -->` tags adjacent** — if removing a slide creates `<!-- end_slide -->\n<!-- end_slide -->`, collapse them to one.

### 7. Reorder Slides

To move a slide: cut the block from its current position (heading through its `<!-- end_slide -->`), then insert it at the target position following the insert rules above.

## Verification After Edits

After changes, verify:
- [ ] All `<!-- end_slide -->` tags are paired properly (each opens and closes a slide)
- [ ] Every slide has exactly one `<!-- speaker_note: | ... -->` block
- [ ] No two `+exec` blocks share a slide
- [ ] Speaker notes all end with a time estimate `[N minutes]` and, if the slide has pauses, a pause count `[N pauses]`
- [ ] Every table has a separator row (`|---|---|`) — no aligned pipe text or Unicode box-drawing characters
- [ ] The file still starts with valid YAML frontmatter
- [ ] No empty slides (a slide with only a heading and speaker note is OK, but nothing less)
- [ ] No orphaned content outside any slide boundary

## Common Mistakes to Avoid

- **Splitting a slide**: Don't cut in the middle of a slide's content. Only split at `<!-- end_slide -->` boundaries.
- **Duplicate speaker notes**: A slide must have exactly one `<!-- speaker_note: |` block. Don't add a second.
- **Missing time estimate**: Every speaker note block should end with `[N minutes]` or `[Remaining time]`.
- **Missing pause count**: If the slide contains `<!-- pause -->` markers, the speaker notes must include a pause count: `[N pauses]`.
- **Broken frontmatter**: If modifying the first slide, don't accidentally merge content into the frontmatter.
- **Missing blank lines**: Ensure blank lines before/after `<!-- end_slide -->` for readability.
- **Bad table format**: Tables must use GFM pipe syntax with a separator row. Unicode box-drawing (`│`, `┼`, `─`) or aligned text with pipes without a separator row will not render.
