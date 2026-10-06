---
name: code-quality
description: Apply language-agnostic code quality principles when writing, reviewing, refactoring, or designing software. Favor simple, explicit, readable, testable code; clear responsibility boundaries; shallow control flow; deliberate abstractions; and isolation of side effects. Leave deterministic rules such as formatting, complexity thresholds, forbidden syntax, and coverage requirements to project tooling whenever possible.
---

# Code Quality

## Purpose

Produce code that is simple, explicit, easy to reason about, and easy to test.

This skill defines code-quality principles rather than formatting rules or arbitrary numeric limits.

Whenever a rule can be enforced reliably by tooling such as a formatter, linter, type checker, complexity analyzer, or test framework, prefer enforcing it there instead of spending reasoning on it.

## Core Principle

Prefer boring code.

Code should be predictable and unsurprising. Optimize for understanding, not cleverness or terseness.

## Guidelines

### Optimize for readability

Prefer:

- explicit logic over cleverness
- descriptive names over abbreviations
- straightforward control flow over compact expressions
- code that communicates intent without requiring mental decoding

Avoid unnecessary tricks, compressed syntax, or abstractions that make code harder to follow.

Concise code is desirable only when it remains clear.

### Use blank lines to show the steps

Group lines that do one step of an algorithm together, and separate steps with a blank line. Think of blank lines as paragraph breaks.

For example: validate the input, blank line, do the main work, blank line, build the result.

Do not jam a whole algorithm into one unbroken block. Do not put a blank line between every line either, or the grouping stops meaning anything.

Deciding where one step ends and the next begins takes judgment. If the project's linter already enforces specific blank-line rules, follow those and do not repeat them here.

### Keep responsibilities focused

Functions, classes, modules, and layers should have a clear and narrow responsibility.

A unit of code should do one conceptual thing well. When it starts coordinating unrelated concerns, separate those concerns when doing so improves clarity.

Do not split cohesive code merely to satisfy arbitrary size limits.

### Keep control flow shallow

Prefer linear control flow.

Use guard clauses, early returns, early validation, and extraction of independent logic when they reduce nesting.

Avoid deeply nested conditionals and unnecessarily complex branching.

### Prefer explicit APIs

Function signatures should make usage obvious.

Avoid long positional argument lists, especially when values can be confused or reordered.

When appropriate, use a primary input plus an options or configuration object.

```ts
sendMessage(message, {
  retries: 3,
  timeout: 5000,
});
```

Do not introduce configuration objects when a small positional API is already obvious.

### Separate pure logic from side effects

Prefer pure functions for business logic, calculations, and transformations.

Keep side effects behind clear boundaries, including:

- database access
- network calls
- filesystem operations
- external services
- system state
- clocks
- randomness

Push impure behavior toward the edges of the system when practical so that the majority of the codebase remains deterministic and easy to test.

### Respect architectural boundaries

Keep responsibilities between layers explicit.

The specific architecture may vary by project, but concerns should not leak unnecessarily across boundaries.

For example:

- transport layers handle transport concerns and input/output translation
- application or service layers coordinate use cases
- domain logic owns business rules
- persistence layers handle storage
- integration layers communicate with external systems

Avoid placing business rules in transport code, persistence concerns in domain logic, or infrastructure details throughout the application without a clear reason.

### Prefer testable design

Design behavior so that it can usually be tested without constructing the entire system.

Favor pure functions, deterministic behavior, small interfaces, explicit dependencies, and isolated side effects.

Use dependency injection where it meaningfully improves isolation or replaceability; do not introduce it mechanically.

Tests should primarily validate observable behavior rather than implementation details.

Do not distort production code solely to make tests easier to write.

### Use abstraction deliberately

Do not abstract merely because two pieces of code look similar.

Create abstractions when they represent the same concept, behavior, or responsibility and the abstraction makes the code easier to understand or change.

Prefer a small amount of clear duplication over a misleading or overly generic abstraction.

Avoid premature generalization.

### Comment intent, not syntax

Comments should explain information that the code cannot express clearly on its own.

Useful comments include:

- non-obvious business rules
- unusual constraints
- external system behavior
- intentional trade-offs
- surprising implementation decisions
- reasons why an apparently simpler solution is incorrect

Avoid comments that simply narrate the code.

Do not require comments or documentation blocks for every function.

## Decision Priority

When principles conflict, prioritize:

1. Correctness
2. Clarity
3. Simplicity
4. Maintainability
5. Testability
6. Consistency
7. Conciseness

Do not sacrifice a higher-priority property merely to improve a lower-priority one.

## Tooling Boundary

Do not spend significant reasoning enforcing rules that can be checked deterministically.

Prefer project tooling for rules such as:

- formatting
- maximum function length
- cyclomatic or cognitive complexity thresholds
- unused variables
- import ordering
- type correctness
- forbidden syntax
- test coverage thresholds
- statically enforceable naming conventions

This skill should focus primarily on properties that require judgment, including:

- whether responsibilities are well separated
- whether an abstraction is justified
- whether code is unnecessarily clever
- whether an API is easy to understand
- whether control flow is easy to follow
- whether side effects are appropriately isolated
- whether architectural boundaries are respected
- whether the design is easy to reason about and test

If a project already has automated rules for a concern, respect those rules and do not duplicate them as subjective review criteria unless they reveal a broader design problem.

## When Writing Code

Choose the simplest implementation that satisfies the requirement while remaining clear and maintainable.

Do not add abstractions, layers, configurability, extensibility, comments, or defensive code without a concrete reason.

Follow the conventions and architecture already established by the project unless they materially conflict with these principles or the task explicitly calls for improvement.

## When Reviewing or Refactoring Code

Focus on meaningful design and readability issues rather than stylistic preferences already handled by tooling.

Prefer small, local improvements over broad rewrites unless the existing structure prevents a clean solution.

Do not recommend a refactor solely because code could be written differently.

A change should have a clear benefit in correctness, clarity, responsibility separation, testability, or maintainability.

## Default Review Question

Ask:

> Is there a simpler, clearer, and more testable way to express this without introducing unnecessary abstraction?

If not, keep the implementation straightforward.
