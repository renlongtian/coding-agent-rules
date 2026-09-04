## CODEx.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5.Mandatory Pre-Code Modification Rule
Before making any edits to source code files:
1. List all target files and detail every planned code modification clearly.
2. Pause fully and wait for my explicit written approval before altering any code.
3. Do not write, rewrite, refactor, delete, or overwrite any code without my clear consent.
---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

## 6. Project Memory / Memory Bank

**Maintain persistent project context across AI sessions.**

Each project should contain a `memory-bank/` directory. These files are the source of truth for project background, requirements, decisions, implementation status, and current work.

### Required Files

- `memory-bank/projectBrief.md`
  - Project purpose
  - Core user requirements
  - Scope and non-goals
  - Success criteria

- `memory-bank/productContext.md`
  - User/business background
  - Target users
  - Product behavior expectations
  - Important constraints

- `memory-bank/activeContext.md`
  - Current focus
  - Current task
  - Open questions
  - Blockers
  - Next concrete step

- `memory-bank/decisionLog.md`
  - Important decisions
  - Date of decision
  - Reasoning
  - Alternatives considered
  - Consequences or tradeoffs

- `memory-bank/progress.md`
  - Completed work
  - In-progress work
  - Pending work
  - Known bugs
  - Verification status

- `memory-bank/systemPatterns.md`
  - Architecture patterns
  - Coding conventions
  - Testing approach
  - Reusable implementation patterns
  - Project-specific gotchas

### Start-of-Task Rule

Before starting any non-trivial task, read the relevant `memory-bank/` files first.

At minimum, read:

1. `projectBrief.md`
2. `activeContext.md`
3. `progress.md`
4. `decisionLog.md`

If the task involves architecture, also read `systemPatterns.md`.

If the task involves product behavior, also read `productContext.md`.

### Update Rule

Update the Memory Bank when any of the following happen:

- A requirement is added, removed, or clarified.
- A technical or product decision is made.
- A task is completed or abandoned.
- The implementation plan changes.
- A bug, blocker, or important constraint is discovered.
- A verification result changes the project state.

### Update Discipline

Keep Memory Bank files concise and current.

- Do not turn them into a full changelog.
- Prefer current state over historical detail.
- Move long historical notes into separate linked documents if needed.
- Every update should help a future AI or developer resume the project faster.

### Session Handoff Rule

At the end of a meaningful work session, update:

1. `activeContext.md` with the current state and next step.
2. `progress.md` with completed and pending work.
3. `decisionLog.md` if any decision was made.

The goal is that a new AI session can continue the project by reading the Memory Bank without requiring the user to restate context.

### Bootstrap Rule

If the `memory-bank/` directory does not exist, create it before starting any non-trivial project work.

When bootstrapping the Memory Bank, create these files:

- `memory-bank/projectBrief.md`
- `memory-bank/productContext.md`
- `memory-bank/activeContext.md`
- `memory-bank/decisionLog.md`
- `memory-bank/progress.md`
- `memory-bank/systemPatterns.md`

Initialize each file with a short placeholder structure instead of leaving it empty.

If the project context is unclear, create the files with clearly marked `Unknown` or `TBD` sections, then ask the user for missing information.

Do not block the task only because the Memory Bank is incomplete. Capture what is known, mark what is unknown, and continue when safe.