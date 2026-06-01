---
name: to-code
description: Takes the open PRD document along with their issues and implements the feature in a test-driven manner. Use when the PRD is ready for implementation and the agent needs to write code.
argument-hint: "PRD issue number, URL, or pasted text"
---

# To Code

Implement open issues one vertical slice at a time using TDD.

## Process

### 1. Gather context

Create `.scratch/context.md` with the following sections, then execute the commands and fill in the results:

<context-template>
# Context — <PRD title> (#N)

## Fetch commands

```bash
# PRD body + metadata
GH_PAGER=cat gh issue view <N> --repo <owner>/<repo>

# Child issues (linked or same-milestone)
GH_PAGER=cat gh issue list --repo <owner>/<repo> --milestone "<milestone>" --state open

# CONTEXT.md snapshot
cat CONTEXT.md

# Relevant ADRs
ls docs/adr/
```

## Skills to load

<!-- List each skill file to read before coding. Add or propose new ones if a domain is not covered. -->
- `.github/skills/tdd/SKILL.md` — red-green-refactor loop (always required)
- `.github/skills/grill-me-with-docs/SKILL.md` — if touching domain model or ADRs

<!-- Proposed new skills (ask user before implementing): -->
<!-- - `.github/skills/<name>/SKILL.md` — <one-line reason> -->

## PRD summary

<!-- Paste key goal, acceptance criteria, and out-of-scope notes after running the commands above. -->

## Open questions

<!-- Blockers or ambiguities that need human input before coding starts. -->
</context-template>

After filling `.scratch/context.md`, scan the "Skills to load" section: read each listed skill file now. If you identify a domain gap with no matching skill, add a commented proposal line and **ask the user whether to implement it before proceeding**.

### 2. Initialise progress tracker

Create or update `.scratch/progress.md` before writing any code. This file is ephemeral scratch — do not commit it.

<progress-template>
# PRD Progress — <title> (#N)

**PRD:** <link>  **Started:** <date>

## Slices

- [ ] #N — <title>
- [ ] #N — <title>

## Log

### #N — <title>
- **Commit:** <hash> — <message>
</progress-template>

Mark a slice `- [x]` and append a log entry when it closes.

### 3. Pick one slice

Work one issue at a time, in dependency order. Do not start the next slice until the current one is committed and closed.

### 4. Implement with TDD

Red → Green → Refactor. Read `.github/skills/tdd/SKILL.md` for details.

### 5. Verify

Run the full test suite. All tests must pass before proceeding.

### 6. Commit and close

Commit with a concise imperative message referencing the issue (`feat: add balance endpoint (#12)`). Close the issue on the tracker. Update `.scratch/progress.md`.

### 7. Report and stop

Give the user a one-paragraph summary of what was built, along with the TDD tests implemented, paste the current `.scratch/progress.md`, and stop — do not start the next slice.

## Rules

- **AFK by default** — pause only for true HITL blockers (architecture decision, missing secret, design review).
- **No vibe coding** — every behaviour must be covered by a test before it ships.
- **One slice at a time** — never commit half-finished work across multiple issues.
- **Keep progress.md current** — update at the start and end of every slice.

