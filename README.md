# presenterm — OpenCode Skill

An [OpenCode](https://opencode.ai) skill for creating and editing [presenterm](https://github.com/mfontanini/presenterm) terminal slide decks.

Create full presentations from scratch or edit existing ones — YAML frontmatter, slide separators, speaker notes, incremental reveals (`<!-- pause -->`), and executable demo blocks (`+exec`).

## Demo

<video src="https://github.com/user-attachments/assets/e68b4ab5-b723-4f01-ab54-a66303680440" controls width="100%"></video>

*A quick walkthrough of creating and running a presenterm presentation using this skill.*

## Install

```bash
# Clone into OpenCode's global skills directory
git clone https://github.com/pouralijan/presenterm-skill.git ~/.config/opencode/skills/presenterm
```

Or manually copy the `presenterm/` directory into one of:

| Location | Scope |
|---|---|
| `~/.config/opencode/skills/presenterm/` | Global (all projects) |
| `.opencode/skills/presenterm/` | Project-local (overrides global) |

## Usage

In any OpenCode session:

### Create a new presentation

```
/presenterm
```

Or load the skill programmatically:

```
skill(name="presenterm", user_message="--create")
```

### Edit an existing presentation

```
/presenterm --update
```

Or:

```
skill(name="presenterm", user_message="--update")
```

## What the skill covers

- YAML frontmatter with title, author, slide options
- Slide structure with `<!-- end_slide -->` separators
- Speaker notes with `<!-- speaker_note: | ... -->` blocks (time estimate + pause count)
- Incremental reveals via `<!-- pause -->`
- Executable demo commands (`+exec`) — one per slide, strict rule
- 7 update operations: insert, modify, remove, reorder slides, add demo slides, etc.
- Post-edit verification checklist

## License

MIT
