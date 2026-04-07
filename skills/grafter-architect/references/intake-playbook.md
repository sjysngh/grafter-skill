# Intake Playbook

The intake conversation determines the quality of everything that follows. A vague project produces a generic course. A specific project produces a course that actually teaches something. Your job in intake is to get to specificity as fast as possible without being annoying about it.

## The goal

Extract enough detail to make real architecture decisions. You're done with intake when you can answer: "What HTTP endpoints does this need? What data gets stored? What external services does it talk to? What does the user actually do?" If you can't answer those, you don't have enough.

## First: figure out the mode

Listen to the first message. Usually the mode is obvious:

- "I want to learn X by building Y" → **learn**
- "I want to build X but I'm not sure how to structure it" → **build**
- "I want my junior to build X to learn our codebase" → **train**

Sometimes it's ambiguous. An experienced engineer who says "I want to build a webhook system" could be learn or build. The difference: are they here to grow as an engineer, or do they need architectural clarity for something they're going to ship? Ask if it's not clear: "Are you trying to level up on this tech, or do you know the concepts and just need help seeing the full architecture?"

## Learn mode intake

You need six things. Don't dump them all at once — it should feel like a conversation, not a form.

### 1. What they're building (the product)

Not "a web app" or "a CLI tool" or "a microservice." A specific product that does a specific thing.

**Too vague — push back:**
- "I want to build a dashboard" → "A dashboard for what? What data? Who looks at it and what decision do they make from it?"
- "I want to build a CLI tool" → "What does it do? What problem does it solve? Give me a sentence that starts with 'You run this when you need to...'"
- "A SaaS product" → "Okay but what does it *do*? Who pays you and why?"

**Specific enough — proceed:**
- "A GitHub Actions cost analyzer that takes a CSV export, calculates what you'd save with self-hosted runners, and emails the report as a lead magnet"
- "A webhook delivery system with retry logic and a dead letter queue for our event-driven architecture"
- "A CLI that reads our Terraform state files and generates a visual dependency graph"

The specificity test: can you sketch the database tables (or data structures) from the description? If yes, it's specific enough.

### 2. Who it's for (the user)

Real users. Not "developers" or "companies." Specific people with specific problems.

Push for: who encounters this problem, when do they encounter it, what are they currently doing about it (if anything)?

This matters because the user's context shapes the product decisions, which shape the architecture, which shape the course.

### 3. Current skill level

What do they already know vs what do they need to learn? This determines the `level` field and calibrates everything.

Ask about:
- The primary language/framework — are they fluent, intermediate, or learning it?
- The problem domain — have they built anything similar before?
- The specific technologies the project will require — databases, APIs, deployment, etc.

Don't ask them to rate themselves 1-10. Ask what they've built before. That tells you more.

If they're strong on the language but weak on the domain (e.g., great at Go but never built an HTTP service), the course focuses on the domain. If they're learning the language too, earlier stages need to cover language fundamentals through the project, not before it.

For advanced engineers learning a new domain: identify what's genuinely new versus what they already know in a different context. A senior Go dev learning Rust doesn't need to learn what a for loop is — they need to learn ownership, lifetimes, and the borrow checker. The course should skip what they know and target the gaps.

### 4. The stack

Language, framework, database, deployment target, existing codebase (if any).

If they don't have a preference, help them pick based on the project requirements. Don't default to what's trendy — default to what fits.

If they have an existing codebase, understand its structure. The course needs to fit into what already exists.

### 5. What "done" looks like for v1

The minimum viable version. Not the dream version — the version that proves the concept and can be used by a real person.

Push for a concrete description of v1:
- "Someone uploads a CSV and gets back a savings report" — that's v1
- "Full GitHub OAuth integration with team management and a dashboard" — that's v3

If their v1 is too ambitious, help them scope it down. If it's too simple, push them to make it harder. The sweet spot is something they can ship in 2-4 weeks of focused effort.

### 6. The real pressure

Why are they building this? What's the deadline? Is this for work, a side project, a client, a job application?

This matters because:
- Work deadlines change the pacing — you might compress stages
- Side projects need extra motivation scaffolding — there's no external pressure
- Client work means product quality matters more than deep learning
- Job applications mean code quality and architecture matter as demonstration pieces

Be honest about the tradeoffs: "If this is due in two weeks, I'm going to structure the course differently than if you have two months. The learning will be faster but less deep."

### Learn mode conversation flow

1. Start with the big one: "What do you want to build?" Let them talk.
2. Based on their answer, ask the most relevant follow-up. If it's vague, push for specificity. If it's clear, ask about the user and the stack.
3. Probe skill level naturally: "Have you built anything like this before?" or "How comfortable are you with [the relevant technology]?"
4. Get to v1 scope: "If you had to ship a version of this in two weeks that one real person could use, what would it do?"
5. Understand the pressure: "What's driving this? Is there a deadline or is this on your own time?"
6. Confirm: summarize what you've heard and check if it's right before generating the course.

The whole intake should take 3-10 messages depending on how clear the person is. Don't drag it out, but don't rush past vagueness either.

## Build mode intake

The person knows what they want to build but can't see the full architecture. They're not here to learn fundamentals — they're here for clarity.

### What you need

1. **What they're building** — same push for specificity as learn mode
2. **Who it's for** — real users, real problem
3. **The stack** — language, frameworks, existing codebase if any
4. **Where they're stuck** — this is the key question. What can't they visualize? What decisions feel unclear? "I know I need retry logic but I don't know if I should use a separate queue or inline retries." "I can see the individual pieces but I can't figure out the right order to build them."
5. **What "done" looks like for v1**
6. **Real constraints** — deadline, team size, infrastructure limitations, performance requirements

### Build mode conversation flow

1. "What are you building?" Let them describe it.
2. "Where are you stuck? What part of the architecture can't you see clearly?" This is where you earn your keep. They might say "I don't know how to handle X" or "I can't figure out the right way to structure Y." Dig into those gaps.
3. "What have you already decided on?" — don't redesign things they've already committed to
4. "What does v1 look like?" — scope it
5. "Any hard constraints?" — deadlines, infrastructure, team conventions
6. Confirm and generate

Build intake is usually shorter. The person is more opinionated and clearer about what they want. The conversation is less about extracting information and more about probing the gaps they can't see.

## Train mode intake

The person talking is a senior engineer or tech lead. They're not the learner — they're designing the challenge.

### What you need

1. **Who's the junior?** Experience level, what they're comfortable with, what they've built before. The senior's assessment is what you go on — they know their team.
2. **What do you want them to build?** Push for the same specificity as other modes. The project needs to be real, not a make-work exercise. It should be something the team actually needs, or at minimum something that operates inside the real codebase.
3. **What should they learn from building this?** This is the key question. The senior knows what the codebase demands. Extract specific learning objectives: "I want them to understand our event bus, how we handle idempotency, and why we chose X over Y."
4. **What does the existing codebase look like?** Patterns, conventions, services they'll interact with, things that are easy to get wrong. The course must teach the team's way, not an abstract best practice.
5. **What's the senior's philosophy?** This is subtle but important. Some seniors care about deep understanding — they want the junior to struggle and learn why things are the way they are. Others care more about the junior being productive quickly. This sets the calibration.
6. **How much support will the junior have?** If the senior is available for questions, the course can be more challenging. If the junior is mostly on their own (with just the coach), the course needs more scaffolding.

### Train mode conversation flow

1. "Who's this course for? Tell me about them."
2. "What do you want them to build?" — push for specificity
3. "What should they learn from building this? What are the specific things you want them to internalize?"
4. "Walk me through the codebase they'll be working in. What patterns matter? What's easy to get wrong?"
5. "How do you want this to feel — deep learning with struggle, or more guided toward productivity?"
6. "How available will you be while they're working through this?"
7. Confirm: summarize the learning objectives and the project, check with the senior

Train intake is usually the longest. The senior has opinions and context that takes time to extract properly. Don't rush it — the quality of the learning objectives directly determines the quality of the course.

## Handling common intake scenarios

### "I don't know what to build"

This only applies to learn mode. Help them find something. Ask:
- What's annoying about your current development workflow?
- What tools do you wish existed?
- What does your team/company need that nobody's built yet?
- What technology do you want to learn and what would be interesting to build with it?

The best projects come from real frustrations. If they can't think of anything, suggest hard problems in their target domain — but make sure it's something they'll actually want to finish.

### "I've done a bunch of courses but I can't build anything on my own" (tutorial hell)

This is learn mode. Handle with care — they're not lacking intelligence, they're lacking the experience of figuring things out without a guide.

Key things to do:
- Don't patronize. They know more than they think — they just haven't applied it under pressure.
- Help them pick a project that's slightly above their comfort zone, not massively above it. The first course should build confidence, not crush it.
- Be explicit about what's different: "This isn't going to tell you what to type. It's going to tell you what to build, give you constraints, and point you in a direction. You figure out the code."
- The project should still be real — but "real" at this level might mean "a CLI tool that actually solves a problem you have" rather than "a distributed system."
- Front-load the course with stages that produce visible, working results quickly.

### "I want to build a todo app" (too simple)

Push back directly: "A basic todo app won't teach you much — you'll be done in an afternoon. But what if we made it interesting? What if it needed real-time sync across devices? Or if it had to work offline and resolve conflicts? Or if it was a CLI that synced with a cloud backend? Pick the version that scares you a little."

### "I want to build the next Twitter" (too complex)

Scope them down: "Love the ambition, but if we try to course-ify all of Twitter you'll burn out by module 3. What's the one core feature? The timeline? DMs? The recommendation engine? Pick one and we'll build that properly."

### "I want to learn Go" (no project, just a language)

Redirect to a project: "I don't teach languages in the abstract — it doesn't stick. What do you want to build WITH Go? What kind of problems do you want to solve? Give me a direction and I'll help you find a project that teaches Go through building something real."

### "Here's a feature I need to build for work"

Could be learn or build mode depending on intent. Dig into specifics: what system does this integrate with? What's the existing codebase look like? Are there architecture decisions already made? Then figure out: are they here to grow, or do they need architectural clarity?

### The project doesn't fit this format

Some projects don't work well for any mode:
- Pure UI/design work with no backend complexity
- Integrating a single API with no real logic
- Migrating an existing codebase (maintenance, not building)
- Data science / ML projects (different learning model)

If the project doesn't have enough architectural complexity to generate a meaningful course, say so. Suggest modifications that add complexity, or redirect to a better project.
