# Course Design Philosophy

These principles govern how every course is structured. They apply across all three modes — learn, build, and train — but some carry more weight in certain modes. Where a principle applies differently by mode, it's noted.

## Principle 1: Build the real thing, not a simulation

The project is not a teaching exercise dressed up as a product. It's a real product that happens to teach (learn mode), a real system that needs architectural clarity (build mode), or a real codebase feature that trains someone on the team's patterns (train mode).

This means:
- No mock APIs when you can use real APIs
- No fake data when you can use real data
- No simulated users when you can describe real users
- The finished project should be deployable and usable, not just "working in a tutorial sense"

When scoping stages, ask: "Would a professional engineer do this step when building this product for real?" If yes, it belongs. If you're only including it because it's educational, find a way to make it real or cut it.

## Principle 2: Every stage produces something that works

No stage should end with "you now understand X." Every stage ends with something running, something testable, something visible.

This is critical for beginners in learn mode especially. They need the feedback loop of "I built this and it works" at every step. Long stretches of building without seeing results is where motivation dies.

The practical implication: sequence the course so that the earliest stages produce the most visible output. A working HTTP endpoint that returns hardcoded JSON is a better first stage than a perfectly designed data model that doesn't do anything yet. Front-load the dopamine of "it works" and back-load the architectural refinement.

In build mode, "works" means "verifiable." It might be a passing test, a deployed endpoint, or a working integration. The bar is production-readiness, not learning satisfaction.

## Principle 3: Understand the internals before using the abstraction

Before using a library, framework, or SDK — build the thing it does manually, at least once, at least partially. Then use the abstraction.

This is the two-pass pattern:
- Pass 1: Make the raw HTTP call yourself. Parse the JSON yourself. Handle the errors yourself.
- Pass 2: Now use the SDK. Notice how it handles the same things you just handled. Understand what it's doing for you and what it's hiding.

Use judgment about what's worth the two-pass treatment. Core to the learning objectives → two-pass. Incidental to the project → just use the library.

**In build mode**, this principle is relaxed. The person isn't here to learn what's under the hood — they're here for architectural clarity. Use the library directly unless understanding the internals is necessary to make the right architectural choice.

**In train mode**, follow the senior's lead. If the senior wants the junior to understand the team's custom middleware, two-pass it. If the senior just wants them to use the ORM correctly, skip the raw SQL pass.

## Principle 4: Progressive complexity with compounding stages

Each stage builds on the previous one. Nothing is throwaway. The code written in stage 1.1 is still running in the final boss fight. The person isn't starting over each module — they're adding layers to a growing system.

This means:
- Stage 1 might be a single endpoint returning hardcoded data
- Stage 2 replaces the hardcoded data with parsed input
- Stage 3 adds a real calculation to the parsed input
- Stage 4 integrates an external API into the calculation
- The boss fight runs all of it end-to-end with real data

Each stage is small enough to complete in 1-3 hours, but the cumulative effect is a complex system built piece by piece.

The compounding effect is also psychological: by module 3, the person is looking at a codebase they built from scratch. That ownership is the antidote to tutorial hell.

## Principle 5: Constraints are the curriculum

The learning doesn't come from the instructions — it comes from the constraints.

**In learn mode**, constraints force understanding. "Build an HTTP endpoint that handles concurrent requests without a race condition, using only the standard library, with structured logging" teaches everything. "Build an HTTP endpoint" teaches little.

Good learn-mode constraints:
- **Force understanding:** "No frameworks for this stage" means they have to understand what the framework does
- **Simulate real pressure:** "File size limit must be enforced before reading the body"
- **Create interesting problems:** "In-memory only, no external dependencies"
- **Prevent shortcuts that bypass learning:** "Tests must be table-driven"

**In build mode**, constraints are real architectural requirements. "Use a transactional outbox, not direct publishing." "Events must be idempotent." These aren't pedagogical — they're the decisions that shape the system.

**In train mode**, constraints enforce team conventions. "Follow the error wrapping pattern used in the rest of the codebase." "Use the repository pattern." The junior learns how to code *here*, not just how to code.

Every constraint should have a discoverable "why." The person might not understand the reason when they start, but by the time they finish, the constraint should feel obviously correct.

## Principle 6: Errors are curriculum, not obstacles

When the JSON decoder returns an empty result because the API wraps data in an envelope, that's not a bug in the course — that's the course. When the CSV parser chokes on triple-quoted fields, that's not a detour — that's the lesson.

Design stages so that likely errors are learning opportunities:
- If the API has a common gotcha, don't warn them about it — let them hit it
- If a data format has edge cases, include real data that triggers them
- If a language feature has surprising behavior, create a stage where that behavior matters

**In build mode**, errors are still valuable but for different reasons. They surface architectural decisions: "The webhook endpoint times out because the handler is doing synchronous work — you need to decide: background job queue or async processing?"

## Principle 7: Boss fights are integration checkpoints

Every module ends with a boss fight that integrates everything from that module. Boss fights don't introduce new concepts — they force everything to work together.

A good boss fight:
- Combines 3-5 stages worth of work into one end-to-end flow
- Uses real data or real inputs, not test fixtures
- Has a comprehensive verify checklist
- Sets the stakes: "This is the moment everything connects."
- Is hard because integration is hard, not because of new concepts

## Principle 8: The "why" matters more than the "what"

Every stage has a `sub` field that explains why this stage exists in the context of the project. This is the most important piece of writing in the entire course.

Generic: "Rate limiting protects your API from abuse."
Project-specific: "POST /report calls the pricing API on every request. Without rate limiting, someone hammers your endpoint and either abuses the upstream API or runs up your costs. And since this is a lead magnet, it's publicly accessible — anyone can hit it."

The project-specific version connects the technical work to a real consequence. It makes the constraint feel necessary, not academic.

**In build mode**, the sub field surfaces why this stage matters architecturally: "If the payment write and the event publish aren't atomic, you'll end up with payments that never trigger notifications. The transactional outbox guarantees both happen or neither does."

**In train mode**, the sub field connects to the team's context: "The event bus in `internal/events/bus.go` uses a fan-out pattern. Your notification service needs to subscribe to payment events and handle them idempotently — look at how `internal/billing/handler.go` does it for reference."

## Principle 9: Pacing follows energy, not a schedule

The course doesn't have a timeline. Stages are completed when they're completed.

Practically, this means:
- Never say "this should take X hours"
- The first few stages should be faster — build confidence, build momentum
- The hardest stages should come when the person has enough invested that they won't quit
- If someone goes quiet for a few days, don't assume they've quit

**In build mode**, pacing is less of a concern since there's no coach. The course is a plan, not a guided experience. But the sequencing still matters — front-load stages that unblock the most downstream work.

## Principle 10: Ship it or it didn't happen

The final module is always about deployment and real usage. The project isn't done when the tests pass locally — it's done when someone else can use it.

For beginners in learn mode, this might be the first thing they've ever deployed. That's a transformative experience, and the course should treat it as such.

**In build mode**, deployment is often the reason they needed the course in the first place. The final module should address the real deployment concerns: infrastructure, monitoring, graceful degradation, the stuff that separates "it works on my machine" from "it runs in production."

## Principle 11: Respect the existing codebase

When a course is built inside an existing codebase — which is the norm for train mode and common in build mode — the course teaches the codebase's patterns, not abstract best practices.

This means the course generation needs to understand the existing system:
- What patterns and conventions does the codebase follow?
- What services or modules will the new code interact with?
- What are the common pitfalls in this specific codebase?
- What does the team's code review care about?

The sub field for stages should reference real files, real services, and real patterns. Constraints should enforce team conventions.

**This principle is critical in train mode.** The whole point is the junior learning to work inside the existing system. A course that teaches abstract best practices that don't match the real codebase is actively harmful — the junior learns one way and then has to unlearn it.

**In learn mode**, this principle applies when the learner has an existing codebase. Respect it and build within it. When they're starting from scratch, this principle doesn't apply.

## Principle 12: Concepts before integration for advanced learners

When an advanced engineer in learn mode is tackling a genuinely new domain — CRDTs, WASM, eBPF, consensus algorithms — the course structure can invert slightly. Instead of building the full product incrementally, the early stages isolate the new concepts:

1. **Concept stages:** Build a minimal implementation that demonstrates the core concept. A toy CRDT. A hello-world WASM module. The goal is understanding, not production code.
2. **Application stages:** Apply the concept to the real project. Wire it into the actual system.
3. **Production stages:** Handle the hard edges. Failure modes, performance, edge cases, monitoring.

The two-pass pattern is especially powerful here. The concept stage IS the first pass. The application stage is the second pass.

This principle only applies when the new domain concepts are genuinely unfamiliar and complex enough to warrant isolation. For most learn mode courses — even advanced ones — the standard incremental build approach works fine.
