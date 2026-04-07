# Course Schema Reference

This document defines the exact JSON structure for course data. The course renderer expects this format — deviations will break the UI.

## Top-level structure

The course is a JSON object with metadata and an array of modules.

```json
{
  "meta": {
    "title": "Webhook Delivery System",
    "summary": "A reliable webhook delivery service with retry logic, dead letter queue, and a dashboard to monitor delivery health.",
    "for": "Internal teams that need to send event notifications to third-party integrations without losing events.",
    "mode": "build",
    "level": "mid-senior"
  },
  "modules": [
    { module 1 },
    { module 2 }
  ]
}
```

### Meta object

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | The project name. Short, specific. "Webhook Delivery System" not "Learning about webhooks". |
| `summary` | string | 1-3 sentences describing what gets built and why it matters. Written for the person who opens this course cold and needs to know what they're looking at. |
| `for` | string | Who the project serves — real users with a real problem. "Internal teams that need reliable event delivery" not "anyone who wants to learn webhooks". |
| `mode` | string | One of `"learn"`, `"build"`, or `"train"`. Determines how the renderer displays stage content and how the coach behaves. |
| `level` | string | The learner's level as determined during intake. Freeform but typically: `"beginner"`, `"intermediate"`, `"advanced"`, `"mid-senior"`, `"senior"`. Informs stage density, hint depth, and how every field is calibrated within a mode. |

### Mode definitions

**`"learn"`** — The engineer is here to grow. Could be a beginner or advanced — the level field captures that. The course is pedagogical. Constraints force understanding, questions make them think, hints nudge without giving answers. Every field is written to teach.

**`"build"`** — The engineer knows what they want to build but can't fully visualize the architecture. They need gaps filled and decisions surfaced. The course is a structured build plan. No pedagogy — direct, opinionated, fills gaps. Questions become decisions to make. Hints become reference points. No coach involved.

**`"train"`** — A senior engineer is training a junior by having them build something real. The senior's philosophy shapes the course — their patterns, conventions, what they think matters. Coaching is directed toward the senior's vision, not generic best practices.

### How level calibrates content within a mode

The `mode` determines *what kind* of content each field contains. The `level` determines *how that content is pitched*. This matters most in learn and train modes.

**Beginner / intermediate:**
- More stages, smaller scope per stage — quick wins build confidence early
- Constraints are explicit about what to try: "Use only the `net/http` standard library — no frameworks"
- Questions start from fundamentals and build up: "What happens if two requests hit this endpoint at the same time?"
- Hints are generous — more keywords, more direction: `["http.HandleFunc", "sync.Mutex", "go run main.go"]`
- Verify items include understanding checks: "You can explain why you needed a mutex here"

**Advanced / senior (in learn mode):**
- Fewer stages, larger scope — they can handle more per stage
- Constraints target the genuinely new concepts, skip what they already know: "No `.clone()` anywhere — work with the borrow checker" not "Don't use third-party libraries"
- Questions assume existing knowledge and build on it: "How does Rust's ownership model compare to Go's garbage collection for latency-sensitive services?"
- Hints are terse and assume competence: `["lifetime elision rules", "impl Trait vs dyn Trait", "Pin<Box<dyn Future>>"]`
- Verify items skip basics, focus on the new domain: "Your code compiles with zero `.unwrap()` calls outside of tests" not "You can write a for loop"

**In train mode**, the level is the junior's level as assessed by the senior — the same calibration applies, but filtered through the senior's learning objectives. An intermediate junior gets more scaffolding than an advanced one, but both get constraints that enforce the team's conventions.

**In build mode**, level mostly affects density — a mid-senior gets fewer stages with larger scope, a less experienced builder gets more stages with more decisions surfaced per stage. But the content style stays direct and non-pedagogical regardless of level.

## Module object

```json
{
  "id": "m1",
  "num": "01",
  "title": "Module title — short and punchy",
  "tag": "Go",
  "tc": "go",
  "v2": false,
  "stages": [ ... ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique module identifier. Format: `m{N}` — `m1`, `m2`, etc. |
| `num` | string | Display number, zero-padded. `"01"`, `"02"`, etc. |
| `title` | string | Module title. Keep it under 8 words. Should communicate what the module delivers, not what it teaches. "The CSV pipeline" not "Learning to parse CSVs". |
| `tag` | string | Stack tag displayed in the UI. Examples: `"Go"`, `"Astro"`, `"TypeScript"`, `"Fullstack"`, `"Rust"`, `"Python"`. |
| `tc` | string | Tag class for styling. Maps to CSS classes. Use one of: `"go"` (teal), `"astro"` (purple), `"fullstack"` (yellow). For other stacks, use the closest match or `"go"` as default. |
| `v2` | boolean | If `true`, this module is deferred to v2. Renders in a separate section of the sidebar. |
| `stages` | array | Array of stage objects. Every module must end with a boss fight stage. |

## Stage object

Stage fields are the same across all modes, but their *content and purpose* differ by mode. The field names stay consistent so the renderer and executor can consume any course without branching on field names.

```json
{
  "id": "m1s1",
  "num": "1.1",
  "boss": false,
  "title": "Stage title — what you're building",
  "sub": "Why this matters for YOUR project.",
  "obj": "What you'll have built when this stage is done.",
  "callout": "Optional — important gotcha or context.",
  "constraints": [
    "Constraint 1",
    "Constraint 2"
  ],
  "questions": [
    "Question or decision 1",
    "Question or decision 2"
  ],
  "hints": [
    "hint_keyword",
    "another.Hint()"
  ],
  "verify": [
    "Verifiable outcome 1",
    "Verifiable outcome 2"
  ]
}
```

### Field details

**id** (string, required)
Unique stage identifier. Format: `m{module}s{stage}` for regular stages, `m{module}boss` for boss fights. Examples: `"m1s1"`, `"m3s4"`, `"m5boss"`.

**num** (string, required)
Display number. Format: `"{module}.{stage}"` for regular stages, `"⬡"` for boss fights. Examples: `"1.1"`, `"3.4"`, `"⬡"`.

**boss** (boolean, required)
Whether this is a boss fight stage. Boss fights get special styling (yellow highlight) and should integrate everything in the module.

**title** (string, required)
Short, punchy. Describes what you're building or accomplishing, not what you're learning. 
- Good: "Wire the full pipeline behind POST /report"
- Bad: "Learn about HTTP handlers"
- Good: "Parse runner labels into structured data"
- Bad: "Understanding string parsing in Go"

**sub** (string, required)
The most important field. It explains WHY this stage exists in the context of the person's specific project.

- Good: "POST /report takes 2-5 seconds because it calls the pricing API for every runner type in the CSV. That loading state is not optional — without it the user thinks the page is broken and closes the tab before seeing their savings number."
- Bad: "Loading states are important for user experience."

For boss fights, the sub field should set the stakes: what does it mean to complete this integration? Why does it matter?

**obj** (string, required)
What the person will have built when this stage is complete. Must be specific and verifiable. Can include `<code>` tags for inline code.

- Good: "5 reports per IP per hour, in-memory only. 10MB file size limit enforced before reading the body. Every response gets an `X-Request-ID` header that appears in your slog output."
- Bad: "Implement rate limiting for the API endpoint."

**callout** (string, optional)
Use sparingly. For important gotchas, surprising behavior, or critical context that deserves visual emphasis (renders as a blue-bordered callout box). Don't use for every stage — reserve it for genuine "watch out for this" moments.

**constraints** (array of strings, required)
3-5 per stage.

| Mode | What constraints mean |
|------|----------------------|
| `learn` | Each constraint forces a specific learning objective. Removes shortcuts that would bypass understanding. "Calculate in `int64` cents, not `float64` dollars." → forces learning about floating point precision. |
| `build` | Each constraint is a real requirement or architectural decision. "Use a transactional outbox, not direct publishing — the payment write and the event must be atomic." → surfaces an architectural pattern they need. |
| `train` | Each constraint enforces the senior's conventions and patterns. "Follow the error wrapping pattern used in the codebase — `fmt.Errorf('notification: %w', err)`." → teaches the team's way. |

**questions** (array of strings, required)
2-4 per stage.

| Mode | What questions mean |
|------|---------------------|
| `learn` | Questions that test understanding and create "aha" moments. "Why is float arithmetic wrong for money? What's the specific problem with 0.1 + 0.2?" Should require thinking, not Googling. |
| `build` | Decisions the engineer needs to make. "Polling or webhooks for delivery status? Polling is simpler but adds latency. Webhooks are real-time but you need to handle redelivery." These aren't pedagogical — they surface tradeoffs. |
| `train` | Questions the senior wants the junior to grapple with. Often tied to the codebase: "Look at how the billing service handles retries. Why do you think they chose exponential backoff with jitter instead of fixed intervals?" |

**hints** (array of strings, required)
4-6 per stage.

| Mode | What hints mean |
|------|-----------------|
| `learn` | Nudge keywords — function names, package names, patterns. Just enough to point direction without explaining the solution. Think search terms, not instructions. `["sync.Map", "X-Forwarded-For header", "r.RemoteAddr"]` |
| `build` | Reference points — libraries, docs, existing implementations to look at. Not nudges, just pointers. `["pgx.CopyFrom for bulk inserts", "opentelemetry-go for tracing", "see Shopify's webhook delivery blog post"]` |
| `train` | Pointers into the existing codebase. `["internal/events/bus.go fan-out pattern", "billing/handler.go idempotency check", "see PR #247 for why we moved to this approach"]` |

**verify** (array of strings, required)
3-6 per stage. These render as interactive checkboxes.

| Mode | What verify items mean |
|------|------------------------|
| `learn` | Mix mechanical verification with understanding checks. Mechanical: "`go test ./... passes`". Understanding: "You can explain why io.Reader is the right interface here." Product: "Someone else can use it without instructions." |
| `build` | Definition of done — production-oriented. "Handles 1000 concurrent webhook deliveries without degrading." "Dead letter queue entries include the original payload, failure reason, and attempt count." "Graceful shutdown drains in-flight deliveries." |
| `train` | Mix of the senior's expectations and verifiable outcomes. "Code follows the repository pattern used in the rest of the codebase." "Error messages include enough context for oncall to diagnose without reading the code." "The senior would approve this in code review." |

## Boss fight stages

Every module ends with a boss fight. Boss fights have special requirements:

1. **They integrate, they don't introduce.** A boss fight should combine everything from the module. New concepts belong in regular stages.
2. **They should be hard because of integration complexity**, not because of new material.
3. **The verify checklist should be comprehensive.** This is the checkpoint — everything should work before moving on.
4. **The sub field should raise the stakes.** What does completing this integration mean for the project?

## v2 modules

When a feature gets deferred during adaptation, move it to a module (or create a new module) with `"v2": true`. These render in a separate sidebar section below the main course, visible but clearly marked as future work. They serve as a roadmap — the person can see what's coming without it cluttering the active course.

## Generating good content — the quality bar

The difference between a good course and a bad one is entirely in the writing quality of the `sub`, `obj`, `constraints`, `questions`, and `verify` fields. Here's the quality bar:

**sub should make them care.** Connect the technical work to the product outcome. "Without rate limiting, someone hammers your endpoint and abuses the upstream API" — that's a reason to care. "Rate limiting is a best practice" — that's a platitude.

**obj should be unambiguous.** If two people read the objective, they should build roughly the same thing. If the objective is vague enough that wildly different implementations would satisfy it, it's too vague.

**constraints should serve the mode.** In learn mode, they teach through restriction. In build mode, they encode real architectural decisions. In train mode, they enforce team conventions. In all modes, every constraint should have a clear reason.

**questions should create clarity.** In learn mode, "aha" moments. In build mode, informed decisions. In train mode, understanding of team patterns. No mode should produce questions that can be answered by Googling.

**verify should be runnable.** As many items as possible should be things the person can actually execute: run a command, hit an endpoint, check a test. Subjective items ("you understand X") should be the minority — and in build mode, they should be near zero.
