# Course Schema Reference

This document defines the exact JSON structure for course data. The course renderer expects this format — deviations will break the UI.

## Top-level structure

The course is a JSON array of module objects. Not an object wrapping an array — the top-level value IS the array.

```json
[
  { module 1 },
  { module 2 },
  ...
]
```

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

```json
{
  "id": "m1s1",
  "num": "1.1",
  "boss": false,
  "title": "Stage title — what you're building",
  "sub": "Why this matters for YOUR project. Not a generic explanation.",
  "obj": "What you'll be able to do when this stage is done. Specific and verifiable.",
  "callout": "Optional — important gotcha or context that deserves visual emphasis.",
  "constraints": [
    "Constraint that forces a learning objective",
    "Another constraint"
  ],
  "questions": [
    "Question that makes them think, not Google",
    "Another question"
  ],
  "hints": [
    "nudge_keyword",
    "another.Hint()",
    "just enough to point direction"
  ],
  "verify": [
    "Specific verifiable outcome",
    "Another verification checkpoint"
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
This is the most important field. It explains WHY this stage exists in the context of the person's specific project. Not a generic explanation of the technology — a specific reason tied to their product.

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
Each constraint should force a specific learning objective. The constraint isn't arbitrary — there's a reason for it.

- "Calculate in `int64` cents, not `float64` dollars." → forces learning about floating point precision
- "No frameworks." → forces understanding of the underlying primitives
- "Accept `io.Reader`, not `[]byte` or `string`." → forces learning about Go interfaces

3-5 constraints per stage is the sweet spot. Every constraint should have a clear reason the person will understand (or discover).

**questions** (array of strings, required)
Questions the person should be able to answer before moving on. These test understanding, not recall.

- Good: "Why is float arithmetic wrong for money? What's the specific problem with 0.1 + 0.2?"
- Bad: "What is a float64 in Go?"
- Good: "What happens if two requests hit this endpoint simultaneously?"
- Bad: "What is concurrency?"

2-4 questions per stage. Each should require thinking, not Googling.

**hints** (array of strings, required)
Nudge keywords — function names, package names, patterns, just enough to point direction without explaining the solution. These render as small code-styled tags.

- Good: `["sync.Map", "X-Forwarded-For header", "r.RemoteAddr", "http.MaxBytesReader"]`
- Bad: `["Use sync.Map for thread-safe in-memory storage of rate limit counters"]`

4-6 hints per stage. Think of them as search terms, not instructions.

**verify** (array of strings, required)
The "you're done when" checklist. Mix mechanical verification (tests pass, endpoint returns data) with understanding verification (you can explain why X).

- Mechanical: "`go test ./... passes`"
- Understanding: "You can explain why io.Reader is the right interface here"
- Product: "Someone else can use it without instructions from you"

3-6 verification items per stage. These render as interactive checkboxes the person can tick off.

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

**constraints should teach through restriction.** Every constraint removes a shortcut that would bypass understanding. The person should feel the constraint pushing them toward deeper knowledge.

**questions should create "aha" moments.** The best questions are ones where the person thinks they know the answer, tries it, and discovers something surprising.

**verify should be runnable.** As many items as possible should be things the person can actually execute: run a command, hit an endpoint, check a test. Subjective items ("you understand X") should be the minority.
