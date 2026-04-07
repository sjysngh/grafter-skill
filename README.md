# Grafter

Three Claude Code skills that turn real projects into structured courses, coach you through building them, or just implement the stages for you. Not tutorials, not toy apps — you bring something you actually need to build and the course falls out of what the build demands.

## The three skills

**grafter-architect** — Runs the intake conversation, figures out what you're building and why, then generates a sequenced course with modules, stages, boss fights, and verification checklists. Produces a self-contained HTML file you open in a browser.

**grafter-coach** — Picks up an existing course and coaches you through building it. Nudges over answers when you're hitting a learning objective, straight answers when you're fighting incidental friction. Never writes code for you. That's the whole point.

**grafter-build** — Takes course stages (or a clipboard export from the course UI) and implements them. Treats constraints as requirements and verify items as definition of done. No teaching, no nudging — just builds what the spec says.

## Three modes

Every course is one of these. The architect figures out which one during intake.

**Learn** — You're here to grow. Could be a beginner building their first real thing or a senior picking up Rust after a decade of Go. The course is pedagogical — constraints force understanding, questions make you think, hints nudge without giving answers. The coach is involved. How much scaffolding you get depends on your level, but everyone gets the same respect and the same expectation that the project is real.

**Build** — You know what you want to build but you can't see the full architecture. You have the idea, maybe some of the pieces, but you need the gaps filled and the decisions surfaced. The course is a structured build plan, not a learning journey. Questions are decisions to make, not exercises. Hints are reference points, not nudges. No coach — the course itself is the deliverable.

**Train** — A senior engineer is training a junior by having them build something real inside the existing codebase. The senior defines the project, the learning objectives, and their philosophy on what matters. The course enforces the senior's vision — their patterns, their conventions, what they think the junior should internalize. The coach is involved, but directed toward the senior's goals rather than open-ended exploration.

## Installation

### Claude Code

Clone the repo somewhere, then symlink each skill into `~/.claude/skills/`:

```bash
git clone https://github.com/YOUR_USERNAME/grafter.git ~/grafter

ln -s ~/grafter/skills/grafter-architect ~/.claude/skills/grafter-architect
ln -s ~/grafter/skills/grafter-coach ~/.claude/skills/grafter-coach
ln -s ~/grafter/skills/grafter-build ~/.claude/skills/grafter-build
```

Three symlinks because `~/.claude/skills/` expects each entry to have a `SKILL.md` at its root. The repo is a monorepo with three skills inside it — each one needs its own link.

To update later, just `cd ~/grafter && git pull`. The symlinks stay valid.

### Claude.ai

Grab the tarball from releases. Upload it as a skill file in your project settings.

## What's in here

```
grafter/
├── README.md
├── shared/
│   └── course-schema.md                          # Schema — the contract between all three skills
├── skills/
│   ├── grafter-architect/
│   │   ├── SKILL.md                              # Intake + course generation
│   │   └── references/
│   │       ├── course-design-philosophy.md        # 12 principles behind course structure
│   │       ├── course-schema.md                   # Schema (copy)
│   │       ├── course-template.html               # HTML renderer with clipper panel
│   │       └── intake-playbook.md                 # How intake works for all three modes
│   ├── grafter-coach/
│   │   ├── SKILL.md                              # Guidance for learn + train modes
│   │   └── references/
│   │       ├── course-schema.md                   # Schema (copy)
│   │       └── pedagogy.md                        # Nudge vs answer rules, motivation handling
│   └── grafter-build/
│       ├── SKILL.md                              # Executor — takes stages, writes code
│       └── references/
│           └── course-schema.md                   # Schema (copy)
```

The course schema is the API between the skills. The architect produces it, the coach reads it, the builder consumes it. Same structure, three different consumers.

## Usage

Start a conversation with something real. The right skill triggers automatically.

**Learn mode:**
- "I want to build a webhook delivery system with retry logic and a dead letter queue"
- "I've done a bunch of Go tutorials but never built anything real — help me build something"
- "I want to learn Rust by building a CLI that generates dependency graphs"

**Build mode:**
- "I want to build X but I can't figure out how to structure the retry logic and dead letter queue"
- "Help me plan out building a plugin system — I know what I want but can't see the architecture"

**Train mode:**
- "Create a course for my junior to build our notification service — I want them to understand our event bus and idempotency patterns"
- "I need an onboarding course that teaches our codebase patterns through building a real feature"

**Coaching (existing course):**
- "I'm stuck on stage 2.3" / "Here's my code for the event handler" / "I finished module 1"

**Building (from clipper or direct):**
- Paste a clipper export or say "implement stages 2.1 through 2.3 from my course"

## Course output

You get a self-contained HTML file. Dark theme, monospace, looks like something a developer would actually want open on a second monitor.

What's in it:
- **Overview page** — what you're building, who it's for, the mode, a map of all modules
- **Left sidebar** — module/stage navigation with progress tracking
- **Stage content** — rendered differently by mode (learn/train shows nudges and thinking questions, build shows decisions and references)
- **Interactive verification checklists** — checkboxes that persist to localStorage
- **Boss fight stages** — highlighted, integration checkpoints that tie everything together
- **Clipper panel** — right sidebar that lets you highlight text, stage it as clips, and export a structured prompt for grafter-build. Highlight anything, click "+ clip", curate your selections, hit "export prompt" and you get a markdown spec ready to paste into a new conversation.

## Development

Edit the skill files directly, load them in Claude Code, try different project prompts. That's it.

The files that matter, by skill:

**grafter-architect:**
- `SKILL.md` — intake modes, course generation, personality
- `references/intake-playbook.md` — the intake conversation for each mode
- `references/course-design-philosophy.md` — the 12 principles
- `references/course-template.html` — the rendered course UI + clipper

**grafter-coach:**
- `SKILL.md` — learn vs train coaching, what it never does
- `references/pedagogy.md` — nudge-vs-answer rules, motivation handling

**grafter-build:**
- `SKILL.md` — how it consumes specs and writes code

**shared:**
- `course-schema.md` — the data contract. Change this and update the copies in each skill's references.

## License

MIT
