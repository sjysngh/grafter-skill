# Course Design Philosophy

These principles govern how every course is structured. They are the difference between a course that produces engineers and a course that produces people who completed a course.

## Principle 1: Build the real thing, not a simulation

The project is not a teaching exercise dressed up as a product. It's a real product that happens to teach. The person should be building something they'd actually use, deploy, or show to someone. If the project feels like a homework assignment, the course has failed.

This means:
- No mock APIs when you can use real APIs
- No fake data when you can use real data
- No simulated users when you can describe real users
- The finished project should be deployable and usable, not just "working in a tutorial sense"

When scoping stages, ask: "Would a professional engineer do this step when building this product for real?" If yes, it belongs. If you're only including it because it's educational, find a way to make it real or cut it.

## Principle 2: Every stage produces something that works

No stage should end with "you now understand X." Every stage ends with something running, something testable, something visible. The person should be able to demonstrate what they built at every checkpoint.

This is critical for tutorial hell survivors especially. They need the feedback loop of "I built this and it works" at every step. Long stretches of building without seeing results is where motivation dies and people revert to following instructions rather than thinking.

The practical implication: sequence the course so that the earliest stages produce the most visible output. A working HTTP endpoint that returns hardcoded JSON is a better first stage than a perfectly designed data model that doesn't do anything yet. Front-load the dopamine of "it works" and back-load the architectural refinement.

## Principle 3: Understand the internals before using the abstraction

Before using a library, framework, or SDK — build the thing it does manually, at least once, at least partially. Then use the abstraction.

This is the two-pass pattern:
- Pass 1: Make the raw HTTP call yourself. Parse the JSON yourself. Handle the errors yourself.
- Pass 2: Now use the SDK. Notice how it handles the same things you just handled. Understand what it's doing for you and what it's hiding.

The person who has written a raw HTTP client once and then switches to a library understands what the library does. The person who only ever used the library treats it as magic — and is helpless when it breaks.

This doesn't mean building everything from scratch. It means building the *important* things from scratch once, so the abstractions you use afterward are understood, not magical. Use judgment about what's worth the two-pass treatment and what isn't. Core to the learning objectives → two-pass. Incidental to the project → just use the library.

## Principle 4: Progressive complexity with compounding stages

Each stage builds on the previous one. Nothing is throwaway. The code written in stage 1.1 is still running in the final boss fight. The person isn't starting over each module — they're adding layers to a growing system.

This means:
- Stage 1 might be a single endpoint returning hardcoded data
- Stage 2 replaces the hardcoded data with parsed input
- Stage 3 adds a real calculation to the parsed input
- Stage 4 integrates an external API into the calculation
- The boss fight runs all of it end-to-end with real data

Each stage is small enough to complete in 1-3 hours, but the cumulative effect is a complex system the person built piece by piece. They understand every layer because they built every layer.

The compounding effect is also psychological: by module 3, the person is looking at a codebase they built from scratch. That's a different feeling from "I completed module 3 of someone else's course." That ownership is the antidote to tutorial hell.

## Principle 5: Constraints are the curriculum

The learning doesn't come from the instructions — it comes from the constraints. "Build an HTTP endpoint" teaches little. "Build an HTTP endpoint that handles concurrent requests without a race condition, using only the standard library, with structured logging" teaches everything.

Good constraints:
- **Force understanding:** "No frameworks for this stage" means they have to understand what the framework does
- **Simulate real pressure:** "File size limit must be enforced before reading the body" mirrors a real production concern
- **Create interesting problems:** "In-memory only, no external dependencies" forces them to think about data structure choice
- **Prevent shortcuts that bypass learning:** "Tests must be table-driven" forces thinking about edge cases

Bad constraints:
- Arbitrary restrictions with no learning value ("must be under 50 lines")
- Restrictions that cause busywork, not thinking ("must use this exact folder structure")
- Restrictions that make the task harder without making the person smarter

Every constraint should have a discoverable "why." The person might not understand the reason when they start the stage, but by the time they finish, the constraint should feel obviously correct.

## Principle 6: Errors are curriculum, not obstacles

When the JSON decoder returns an empty result because the API wraps data in an envelope, that's not a bug in the course — that's the course. When the CSV parser chokes on triple-quoted fields, that's not a detour — that's the lesson.

Design stages so that likely errors are learning opportunities:
- If the API has a common gotcha, don't warn them about it — let them hit it. Then the nudge helps them understand why.
- If a data format has edge cases, include real data that triggers them. Don't sanitize the inputs.
- If a language feature has surprising behavior, create a stage where that behavior matters.

The course shouldn't manufacture fake errors, but it should use real inputs and real APIs where real errors naturally occur. The person remembers the bug they debugged at 11pm far more than the concept they read about in a tutorial.

For tutorial hell survivors, this is transformative. Tutorials sanitize everything — clean data, expected responses, no surprises. Real building is full of surprises. The first time they debug a real error without someone telling them the answer, they cross a line from "following instructions" to "engineering."

## Principle 7: Boss fights are integration checkpoints

Every module ends with a boss fight that integrates everything from that module. Boss fights don't introduce new concepts — they force the person to make everything work together.

A good boss fight:
- Combines 3-5 stages worth of work into one end-to-end flow
- Uses real data or real inputs, not test fixtures
- Has a comprehensive verify checklist that tests the full integration
- Sets the stakes: "This is the moment everything connects. If this works, the product works."
- Is hard because integration is hard, not because of new concepts

The boss fight is also a psychological checkpoint. Completing a boss fight should feel like an accomplishment. The verify checklist should be thorough enough that when everything passes, the person knows — not believes, knows — that they've built something real.

## Principle 8: The "why" matters more than the "what"

Every stage has a `sub` field that explains why this stage exists in the context of the project. This is the most important piece of writing in the entire course. It's not "what you'll learn" — it's "why this matters for the thing you're building."

Generic: "Rate limiting protects your API from abuse."
Project-specific: "POST /report calls the pricing API on every request. Without rate limiting, someone hammers your endpoint and either abuses the upstream API or runs up your costs. And since this is a lead magnet, it's publicly accessible — anyone can hit it."

The project-specific version tells the person exactly why they should care. It connects the technical work to a real consequence in their product. It makes the constraint feel necessary, not academic.

For tutorial hell survivors, this is what was missing from every tutorial they ever took. Tutorials teach "what" and "how" but rarely "why this specific thing for this specific situation." The "why" is what turns knowledge into judgment.

## Principle 9: Pacing follows energy, not a schedule

The course doesn't have a timeline. Stages are completed when they're completed. Some stages will take 30 minutes. Some will take a day. The person works at their own pace, and the course adapts to that pace.

Practically, this means:
- Never say "this should take X hours." Estimated difficulty is okay, but clock-time estimates create anxiety and shame.
- The first few stages should be faster — build confidence, build momentum
- The hardest stages should come when the person has enough invested that they won't quit
- If someone goes quiet for a few days, don't assume they've quit — ask where they are and what's blocking them

## Principle 10: Ship it or it didn't happen

The final module is always about deployment and real usage. The project isn't done when the tests pass locally — it's done when someone else can use it. A real URL, real users, real data.

This is the ultimate test and the ultimate motivator. Everything in the course builds toward this: "Someone you've never met uses the thing you built, and it works."

For tutorial hell survivors, this might be the first thing they've ever deployed. That's a transformative experience, and the course should treat it as such. The final boss fight should be the biggest celebration in the course.
