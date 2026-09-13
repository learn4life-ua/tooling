---
name: lean-coding
description: >
  Keep Codex implementations simple, reusable, and maintainable. Before adding code,
  check whether the capability already exists in the project, standard library,
  platform, or installed dependencies. Avoid unnecessary abstractions and new
  dependencies, while preserving correctness, security, accessibility, clarity,
  maintainability, and explicit user requirements.
metadata:
  version: "1.0.0"
  owner: "Learn4Life"
---

# Lean Coding

Use the simplest solution that fully satisfies the task without sacrificing correctness, safety, accessibility, clarity, or maintainability.

This skill adopts the useful minimalism ideas we identified while reviewing Ponytail, but it is an independent Learn4Life rule set. It does not install or invoke Ponytail, lifecycle hooks, external scripts, or third-party runtime code.

## Decision order

Before writing new code, check in this order:

1. Does the requested change actually require new code?
2. Does the required capability already exist in this codebase? Reuse it instead of reimplementing it.
3. Can the language standard library solve it cleanly?
4. Can a native browser, operating-system, database, framework, or platform feature solve it cleanly?
5. Does an already-installed dependency provide the needed capability?
6. Only then add the minimum new code needed for the task.

## Core rules

- Read and understand the affected code path before editing it.
- Prefer fixing the root cause over adding repeated guards around symptoms.
- Reuse existing helpers, components, utilities, styles, data structures, and project patterns when they already fit the task.
- Do not add a dependency when a clear, small, maintainable solution already exists in the project or platform.
- Do not introduce speculative abstractions, factories, interfaces, configuration layers, wrappers, or extension points "for later" unless the task genuinely requires them.
- Do not create extra files merely to make a small change look architecturally elaborate.
- Prefer boring, readable code over clever compression.
- A shorter implementation is not automatically better. Do not turn clear code into opaque one-liners merely to reduce line count.
- Preserve the existing project architecture and design system unless the user explicitly asks for a structural change.
- If two solutions are similarly simple, prefer the one that handles edge cases correctly and is easier to maintain.
- When changing shared logic, inspect relevant callers so the fix lands at the correct layer.

## Never simplify away

Do not remove or weaken:

- security controls;
- validation at trust boundaries;
- error handling that prevents data loss or corrupted state;
- accessibility requirements;
- privacy protections;
- tests or checks that protect non-trivial or high-risk logic;
- requirements explicitly requested by the user;
- necessary configuration for real hardware, deployment, localization, compatibility, or operational constraints.

## Dependencies

Before adding a new dependency:

1. confirm the project does not already provide the capability;
2. check the standard library and native platform;
3. check currently installed dependencies;
4. add a new dependency only when it materially improves correctness, reliability, maintainability, or development cost.

When a new dependency is justified, explain the reason briefly if it is not obvious from the task.

## Refactoring

Refactor only as far as needed to make the requested change correct and maintainable.

Do not combine a focused task with an unrelated architectural rewrite unless the user explicitly asks for it or the existing structure makes the requested change unsafe or impossible.

## Output behavior

For normal coding tasks, implement the requested result rather than debating whether the user should want it.

If a substantially simpler approach exists, use it when it fully satisfies the request. Mention the simplification briefly only when it helps the user understand an important trade-off.

Do not make brevity of the response or brevity of the source code an objective by itself.

## Priority

When rules conflict, use this order:

1. explicit user requirements;
2. correctness and data integrity;
3. security, privacy, and accessibility;
4. existing project architecture and compatibility;
5. maintainability and clarity;
6. reuse and minimal implementation;
7. code-size reduction.
