# agent-skills

Portable [Agent Skills](https://agentskills.io/) for any agent or editor that loads `SKILL.md` from a skills directory.

## Install

Clone this repo and point your agent at the skills folder (or copy skill directories you want):

```bash
git clone https://github.com/dcmirandah/agent-skills.git
```

| Layout | Notes |
|--------|--------|
| User-level skills dir | Common pattern: `~/<tool>/skills/` — check your agent's docs |
| Project-level | Some tools support `.agents/skills/` or similar in the repo |

**Option A — symlink the whole tree**

```bash
SKILLS_DIR=~/.your-agent/skills   # set to your tool's skills directory
ln -sfn "$(pwd)" "$SKILLS_DIR/agent-skills"
```

**Option B — copy individual skills**

```bash
SKILLS_DIR=~/.your-agent/skills
cp -R powers caveman "$SKILLS_DIR/"
```

Each skill is a directory with a `SKILL.md` file. Agents discover skills from the frontmatter `name` and `description` fields.

## Skills in this repo

| Skill | Purpose |
|-------|---------|
| `powers/` | Workflow router and sub-skills (brainstorm, TDD, debug, plans, review, …) |
| `caveman` | Ultra-compressed responses when you want fewer tokens |
| `drawio` | Repo overviews and Draw.io diagrams |
| `prompt-engineer` | Turn rough notes into clear prompts for other models |
| `stress-tester` | Grill a design/plan (adapted from mattpocock `grill-me`) |

## Requirements

Some skills expect tools on your `PATH`. Check each skill’s `SKILL.md` for prerequisites.

## License

MIT — see [LICENSE](LICENSE).

Some skills are adapted from upstream projects; see [ATTRIBUTION.md](ATTRIBUTION.md)
(`caveman` and `stress-tester` from [mattpocock/skills](https://github.com/mattpocock/skills),
`powers` from [obra/superpowers](https://github.com/obra/superpowers)).
