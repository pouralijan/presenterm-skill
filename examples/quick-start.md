---
title: "Quick Start Guide"
author: "Your Name"
options:
    h1_slide_titles: true
---

# Quick Start Guide

<!-- speaker_note: |
  A minimal but complete presentation to get started quickly.

  [1 minute]
-->

*A minimal presenterm presentation covering the essentials*

Your Name


<!-- end_slide -->

# Slides

<!-- speaker_note: |
  Each slide starts with an <h1> title and ends with the end_slide marker.

  Use the pause command to reveal content progressively.

  [2 minutes]
  [1 pause]
-->

### A slide can have:

- **Bold text** for emphasis
- Bullet lists like this one
- `inline code` for commands
- Tables, block quotes, and more

<!-- pause -->

### Demo commands use `+exec`

```bash +exec
echo "Hello from presenterm!"
```


<!-- end_slide -->

# Speaker Notes

<!-- speaker_note: |
  Every slide has a speaker_note block with narration.

  The | after the colon is required — it starts a YAML literal block.
  Each line is indented with 2 spaces.
  End with a time estimate in brackets.

  Time estimates help you pace the presentation.
  Pause counts help you know how many clicks per slide.

  [2 minutes]
  [2 pauses]
-->

### Presenter mode

- Run `presenterm -P` to publish notes to a browser
- Your audience sees only the slides

<!-- pause -->

### Speaker note format

```
<!-- speaker_note: |
  Narration here.

  [2 minutes]
  [2 pauses]
-->
```

<!-- pause -->

### Remember

Every slide **must** have a speaker notes block — even if brief


<!-- end_slide -->

# Running

<!-- speaker_note: |
  Quick overview of how to run and navigate.

  [1 minute]
-->

| Action | Command |
|---|---|
| Normal mode | `presenterm file.md` |
| Speaker notes | `presenterm -P file.md` |
| Exec mode | `presenterm -x file.md` |
| Next/Prev | Arrow keys or `j`/`k` |
| Run demo | `Ctrl+E` |
| Reload | `Ctrl+R` |


<!-- end_slide -->

# Summary

<!-- speaker_note: |
  Three things to remember:
  1. One exec command per slide
  2. Every slide needs speaker notes
  3. h1 headings become slide titles

  [1 minute]
-->

| Element | Required | Notes |
|---|---|---|
| Frontmatter | ✅ | title, author, options |
| Speaker notes | ✅ | time estimate + pause count |
| end_slide | ✅ | separates slides |
| exec commands | ⚠️ | one per slide maximum |


<!-- end_slide -->

# Q&A

<!-- speaker_note: |
  Open for questions.

  [Remaining time]
-->

*Questions?*

**Docs:** [mfontanini.github.io/presenterm](https://mfontanini.github.io/presenterm/)


<!-- end_slide -->
