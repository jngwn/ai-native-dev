# AGENTS.md

## Scope

This folder contains notes and templates for AI-native software development.

The goal is to help a human start large projects with AI agents by writing clear instruction documents before implementation.

## Editing Principles

- Preserve practical, reusable guidance over abstract theory.
- Prefer concrete templates, checklists, and examples.
- Keep documents readable as personal engineering notes, not formal corporate process docs.
- Do not make the system heavier unless the added structure prevents real agent failure modes.
- When improving a document, clarify what decision the agent should not make alone.
- Clarify what artifact should be produced, how completion should be verified, and what remains out of scope.
- Avoid duplicating the same rule across many files unless it is intentionally central.
- If two documents disagree, report the conflict instead of silently choosing one.

## Improvement Standard

A good improvement makes it easier for an agent to:

- understand the project goal
- avoid unauthorized architectural decisions
- work in small verified steps
- report uncertainty clearly
- leave useful context for the next session

## Reference Project Analysis

When improving these documents from a reference project:

- Extract reusable agent workflow patterns, not stack-specific habits.
- Translate tool-specific commands into generic verification categories.
- Do not add a language, framework, package manager, or build tool as a recommendation unless the document is explicitly about that stack.
- Preserve concrete examples only when they clarify a general pattern.
- If a reference project is strong because of its workflow rather than its technology, document the workflow.
- If a tool name appears as an example, make clear that it is illustrative and not a preferred default.

## Do Not

- Add generic AI productivity advice.
- Add motivational prose.
- Turn templates into long essays.
- Remove constraints just to make the document shorter.
