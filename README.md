# Coding Agent Rules

A reusable set of coding-agent behavioral rules focused on safe code modification, minimal changes, verifiable execution, and persistent project context.

The goal is simple:

> Make AI coding agents behave more like disciplined software engineers and less like uncontrolled code generators.

These rules prioritize correctness, clarity, minimal diffs, and explicit verification over speed.

---

## Why This Repository Exists

AI coding agents are powerful, but they also tend to make several recurring mistakes:

- Making assumptions without asking
- Over-engineering simple tasks
- Refactoring unrelated code
- Modifying more files than necessary
- Claiming a task is complete without verification
- Losing project context between sessions
- Editing code before the user has approved the plan

This repository provides a reusable `AGENTS.md` designed to reduce those behaviors.

---

## Core Principles

### 1. Think Before Coding

Before writing code:

- State assumptions explicitly
- Surface ambiguity instead of silently guessing
- Present multiple interpretations when necessary
- Prefer simpler approaches when they solve the problem
- Ask for clarification when critical information is missing

The agent should understand the task before modifying the codebase.

---

### 2. Simplicity First

Write the minimum amount of code required to solve the task.

Avoid:

- Unrequested features
- Premature abstractions
- Unnecessary configurability
- Speculative flexibility
- Overly defensive code for impossible scenarios
- Large implementations when a much smaller solution exists

A good implementation should be easy to understand and easy to review.

---

### 3. Surgical Changes

Only change what the task requires.

When modifying an existing codebase:

- Do not refactor unrelated code
- Do not reformat unrelated files
- Do not rewrite adjacent comments unnecessarily
- Preserve the existing coding style
- Do not remove pre-existing dead code unless requested
- Remove only unused code introduced by your own changes

Every changed line should be traceable to the requested task.

---

### 4. Goal-Driven Execution

Tasks should be converted into verifiable goals.

Examples:

```text
"Add validation"
→ Write tests for invalid inputs
→ Implement validation
→ Verify the tests pass
````

```text
"Fix the bug"
→ Reproduce the bug
→ Create a failing test
→ Apply the fix
→ Verify the test passes
```

```text
"Refactor module X"
→ Verify existing tests
→ Refactor
→ Verify tests still pass
```

For multi-step tasks, use a concise plan:

```text
1. Step → verify: expected result
2. Step → verify: expected result
3. Step → verify: expected result
```

The agent should work toward measurable success criteria rather than vague goals such as:

```text
"make it work"
```

---

## Mandatory Pre-Code Approval

Before modifying any source code, the agent must:

1. List every target file
2. Explain every planned modification
3. Stop and wait for explicit user approval
4. Modify only the approved scope

The agent must not write, rewrite, refactor, delete, or overwrite source code before approval.

This rule intentionally favors safety and user control over maximum autonomy.

---

# Project Memory / Memory Bank

Long-running projects often lose important context between AI sessions.

To solve this, the rules define a persistent project memory system using a:

```text
memory-bank/
```

directory.

The Memory Bank stores project background, requirements, decisions, implementation status, and current work.

---

## Memory Bank Structure

```text
memory-bank/
├── projectBrief.md
├── productContext.md
├── activeContext.md
├── decisionLog.md
├── progress.md
└── systemPatterns.md
```

### `projectBrief.md`

Contains:

* Project purpose
* Core requirements
* Scope
* Non-goals
* Success criteria

---

### `productContext.md`

Contains:

* Business or user background
* Target users
* Expected product behavior
* Important constraints

---

### `activeContext.md`

Contains the current working state:

* Current focus
* Current task
* Open questions
* Blockers
* Next concrete step

---

### `decisionLog.md`

Records important decisions:

* Decision
* Date
* Reasoning
* Alternatives considered
* Consequences
* Tradeoffs

---

### `progress.md`

Tracks implementation progress:

* Completed work
* In-progress work
* Pending work
* Known bugs
* Verification status

---

### `systemPatterns.md`

Stores reusable technical knowledge:

* Architecture patterns
* Coding conventions
* Testing approach
* Reusable implementation patterns
* Project-specific gotchas

---

# Start-of-Task Workflow

Before starting a non-trivial task, the agent should read the relevant Memory Bank files.

At minimum:

```text
projectBrief.md
activeContext.md
progress.md
decisionLog.md
```

For architecture-related work:

```text
systemPatterns.md
```

For product-behavior-related work:

```text
productContext.md
```

This allows a new session to recover project context without requiring the user to explain everything again.

---

# Memory Update Rules

The Memory Bank should be updated when:

* A requirement is added
* A requirement is removed
* A requirement is clarified
* A technical decision is made
* A product decision is made
* A task is completed
* A task is abandoned
* The implementation plan changes
* A bug is discovered
* A blocker is discovered
* An important constraint is discovered
* Verification changes the known project state

Memory files should remain concise and useful.

They are not intended to become a complete project changelog.

Prefer:

```text
current useful state
```

over:

```text
complete historical record
```

---

# Session Handoff

At the end of a meaningful development session, update:

```text
activeContext.md
progress.md
decisionLog.md
```

when applicable.

The goal is simple:

> A new AI session should be able to continue the project by reading the Memory Bank without requiring the user to restate the entire context.

---

# Memory Bank Bootstrap

If the project does not already contain a `memory-bank/` directory, create one for non-trivial project work.

Initial structure:

```text
memory-bank/
├── projectBrief.md
├── productContext.md
├── activeContext.md
├── decisionLog.md
├── progress.md
└── systemPatterns.md
```

Each file should contain a short placeholder structure.

If information is unknown, explicitly mark it as:

```text
Unknown
```

or:

```text
TBD
```

An incomplete Memory Bank should not automatically block safe work.

Capture what is known, mark what is unknown, and continue when appropriate.

---

# Design Philosophy

These rules intentionally bias toward caution over speed.

For trivial tasks, judgment should still be used.

The rules are working when they produce:

* Smaller diffs
* Fewer unrelated modifications
* Less unnecessary refactoring
* Less over-engineering
* Earlier clarification of ambiguous requirements
* More verifiable task completion
* Better continuity between AI sessions

---

# Installation

The main rule file is:

```text
AGENTS.md
```

Place or reference it wherever your coding-agent environment loads persistent project or global instructions.

For project-specific behavior, additional instructions can be layered on top of the global rules.

---

# Repository Structure

A minimal repository can look like this:

```text
coding-agent-rules/
├── README.md
├── AGENTS.md
└── LICENSE
```

Future versions may separate reusable rules, workflows, or domain-specific instructions:

```text
coding-agent-rules/
├── README.md
├── AGENTS.md
├── LICENSE
│
├── rules/
│   ├── git-safety.md
│   ├── verification.md
│   └── embedded-c.md
│
└── skills/
    ├── systematic-debugging/
    └── code-review/
```

---

# Roadmap

Potential future improvements include:

* Git and uncommitted-change protection
* Verification-before-completion rules
* Systematic debugging workflow
* Dependency management rules
* Security and destructive-operation protection
* Code review workflow
* Embedded C / C++ rules
* MISRA-oriented rules
* AUTOSAR-oriented development guidance
* Reusable debugging and review skills

---

# Contributing

Suggestions and improvements are welcome.

When proposing a new rule, prefer rules that are:

* General
* Actionable
* Verifiable
* Concise
* Useful across multiple projects

Avoid adding rules that only address a single one-off situation.

