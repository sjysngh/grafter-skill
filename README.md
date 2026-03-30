# Grafter

A Claude Code skill that builds courses from real projects. Not tutorials, not toy apps — you bring something you actually need to ship and the course falls out of what the build demands.

The whole premise is that the best way to learn something is to build something hard with it. Grafter just adds structure so you don't wander in circles.

## What it does

Four phases, and you never have to think about which one you're in:

- **Intake** — figures out what you're building, who it's for, and where your gaps are. Pushes back if the project is vague or too small to be interesting.
- **Course generation** — looks at the architecture your project actually needs and sequences it into modules, stages, boss fights, and verification checklists. Everything compounds — code from stage 1 still runs in the final boss fight.
- **Guidance** — coaches you while you build. Nudges over answers when you're hitting a learning objective, straight answers when you're fighting incidental friction like import paths or env setup. Knows the difference.
- **Adaptation** — the project changed? Cool, the course changes with it. Deferred a feature to v2, discovered a new requirement, API came back shaped differently than you expected — the course adjusts.

## Who it's for

Four kinds of people, honestly:

- **Engineers stretching into new territory** — you know how to code, you're picking up a new language or domain or kind of system. You don't need hand-holding, you need the hard parts surfaced and structured.
- **Tutorial hell survivors** — you've done the courses, watched the videos, followed the instructions. But the moment the instructions stop, you freeze. You don't lack knowledge — you lack the experience of figuring things out. This gives you that experience with enough structure that you don't drown, but not so much that you're just following along again.
- **Seniors training juniors** — you want a junior to learn your codebase by building something real inside it. You define the project and the learning objectives, the skill generates the course and does the coaching. Frees you from answering questions all day while making sure they learn the right things the right way — your team's patterns, not abstract best practices.
- **Seniors exploring unfamiliar domains** — CRDTs, WASM, eBPF, whatever's new to you. You don't need programming instruction. You need to fill conceptual gaps in something unfamiliar by building with it. The course focuses on the new domain concepts, zero time wasted on things you already know.

## The spectrum thing

Every course sits somewhere between pure learning and pure shipping. This gets set during intake and shapes how every stage comes out.

**Learning mode** — more stages, smaller scope, more constraints that force understanding, nudge-heavy guidance. For tutorial hell survivors and juniors being onboarded.

**Shipping mode** — fewer stages, larger scope, constraints focused on production quality, more direct guidance. For experienced engineers who need velocity with understanding, not deep exploration of every concept.

**Exploration mode** — stages organized around concepts not features, deep questions, hints that point to papers and specs instead of just function names. For seniors who need to build judgment in a new domain, not just make code work.

Most courses blend these. A junior starts in learning mode and shifts toward shipping mode as they level up. A senior learning CRDTs is in exploration mode for the conceptual stages and shipping mode when wiring it into their system. It adjusts per-stage, not just per-course.

## Installation

### Claude Code

Clone and symlink — keeps it up to date with a `git pull`:

```bash
git clone https://github.com/YOUR_USERNAME/grafter.git ~/grafter
ln -s ~/grafter /path/to/claude-code/skills/grafter
```

Or just clone it straight into your skills directory if you prefer:

```bash
git clone https://github.com/YOUR_USERNAME/grafter.git /path/to/claude-code/skills/grafter
```

### Claude.ai

Grab the tarball from releases. Upload it as a skill file in your project settings.

## What's in here

```
grafter/
├── SKILL.md                              # The brain — workflow, personality, phase detection
├── references/
│   ├── course-design-philosophy.md       # 12 principles that govern course structure
│   ├── course-schema.md                  # JSON schema for course data
│   ├── intake-playbook.md                # How intake works across all three paths
│   ├── pedagogy.md                       # Nudge vs answer rules, motivation, guidance modes
│   └── course-template.html              # Self-contained HTML renderer
```

## Usage

Start a conversation with something real:

- "I want to build a webhook delivery system with retry logic and a dead letter queue"
- "Help me build a CLI that reads Terraform state and generates dependency graphs"
- "I've done a bunch of Go tutorials but never built anything real — help me build something"
- "Create a course for my junior to build our notification service — I want them to understand our event bus and idempotency patterns"
- "I need to learn WASM by building a plugin system for our editor"

It figures out which intake path to use, asks the right questions, generates the course, and coaches you through building it.

## Course output

You get a self-contained HTML file. Dark theme, monospace, looks like something a developer would actually want open on a second monitor. Sidebar navigation, interactive verification checklists, boss fight stages highlighted, progress saved to localStorage. Open it in a browser and go.

## Development

Edit the files directly, load the skill in Claude Code, try different project prompts. That's it.

The files that matter:
- `SKILL.md` — workflow, personality, phase detection
- `references/pedagogy.md` — nudge-vs-answer behavior and guidance modes
- `references/course-design-philosophy.md` — the 12 principles behind course structure
- `references/course-schema.md` — course data format
- `references/course-template.html` — the rendered course UI

## License

MIT
