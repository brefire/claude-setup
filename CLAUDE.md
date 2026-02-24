---
description: 
alwaysApply: true
---

# Global Principles

## Task Integrity (PRIMARY PRINCIPLE)

This is the most important section of this document. All other
rules depend on tasks being properly scoped. A poorly defined
task will produce poor results regardless of other safeguards.

### What constitutes a valid task
A valid task is a single functional change with:
- A clear intent (what should be different when this is done).
- A clear location (where in the codebase this change lives).
- A clear output (what the deliverable looks like).
- A scope that can be completed and verified in a single pass.

### Do not accept tasks that:
- Require you to make architectural or design decisions that
  have not been made by the user.
- Contain multiple unrelated functional changes bundled together.
- Are described in terms of an outcome without specifying the
  change (e.g., "make it better", "optimize this").
- Would require installing infrastructure, frameworks, or
  foundational code that does not yet exist.
- Are vague enough that two competent developers would produce
  fundamentally different implementations from the same description.

### When a task is too large or ambiguous
- Do not attempt partial implementation of an ambiguous task.
- Do not break the task down yourself and begin executing.
  Task decomposition is the user's responsibility.
- Instead:
  (a) State why the task as described is too broad or ambiguous
      to produce a reliable result.
  (b) Identify the specific areas that need clarification or
      decomposition.
  (c) Suggest how the task could be broken into smaller units,
      but do not begin work on any of them.
  (d) Wait for the user to provide a refined task.

### Examples
VALID: "Add a blue CTA button to the hero section of the landing
page at src/components/Hero.tsx. The button text should be
'Get Started', it should use our existing Button component with
the primary variant, and link to /signup."

NOT VALID: "Build an app that does stock analysis."
→ This requires architectural decisions, technology selection,
  feature scoping, and design — none of which have been provided.
  This is a project, not a task.

NOT VALID: "Improve the dashboard."
→ Improve how? Performance? Layout? Data accuracy? This has no
  clear intent, no clear output, and no way to verify completion.

NOT VALID: "Set up the backend and create the API for user
management and also build the frontend forms."
→ This is multiple unrelated functional changes. Each API
  endpoint and each form is its own task.

---

## Scope & Judgement
- Analyze the codebase and determine what is in scope for a task.
  Scope includes the target code and its direct dependencies.
- Do not extend scope beyond what is directly required to complete
  the stated task.
- Do not refactor, rename, or reorganize code that functions correctly
  and is not part of the task.
- Do not "fix" or "improve" code you encounter outside the task scope.
- Do not add features, behaviors, or capabilities not explicitly requested.

---

## Actions Requiring Approval
- Do not install, add, or upgrade dependencies or packages without
  discussing the reason and getting confirmation.
- Do not run bash/shell commands unless the task explicitly requires
  execution (e.g., "run the tests"). Analysis and code changes do
  not require command execution.
- Do not delete files, data, or existing implementations without
  explicit confirmation.
- Do not run destructive commands (rm -rf, drop, truncate, force push).

---

## Ambiguity Resolution
- Do not interpret ambiguous requirements by choosing the most
  likely intent. When a task has more than one valid interpretation:
  (a) State what is ambiguous.
  (b) List the possible interpretations you see.
  (c) State what information you would need to proceed.
  (d) Stop and wait for direction.
- Do not fill gaps in a specification with reasonable defaults.
  Missing details are missing — surface them, do not invent them.

---

## Verification Before Execution
- Do not begin work until you have restated:
  (a) What you will change.
  (b) What files are in scope based on your analysis.
  (c) What is out of scope.
  (d) What category of changes these are (new code, modification, deletion).
- Do not assume intent. If the task is ambiguous, follow the
  Ambiguity Resolution protocol above.

---

## Error & Unexpected Finding Remediation
- Do not attempt to fix errors or resolve unexpected findings
  autonomously. When an error or unexpected condition occurs:
  (a) Stop work immediately.
  (b) State what the error or unexpected finding is.
  (c) Restate the original task and current scope.
  (d) State whether the error is within scope or outside scope.
  (e) Propose a path forward without taking action.
  (f) Wait for direction before resuming.
- Do not expand scope to accommodate an error. If fixing the
  error requires changes outside the original scope, that is a
  new task — surface it as such.
- Do not suppress, silence, or work around errors to maintain
  progress on the original task.

---

## Output Quality
- Do not produce code that introduces patterns inconsistent with
  the existing codebase.
- Do not hardcode values to satisfy a requirement or shortcut
  a proper implementation.
- Do not generate placeholder or stub implementations unless
  explicitly asked.
- Do not add comments explaining obvious code. Only comment
  non-obvious decisions.
- Do not disable, suppress, or bypass lint rules to resolve errors.
  This includes eslint-disable, @ts-ignore, @ts-expect-error, noqa,
  or any equivalent mechanism. If the code cannot satisfy the linter,
  surface the issue — do not silence it.
- Do not use loosely typed or untyped constructs. All code must be
  strongly typed. Do not use `any`, implicit `any`, type assertions
  that bypass safety (as any), or untyped function signatures.
  If a proper type does not exist, define one.

---

## Git & Version Control
- Do not make changes directly on the main/master branch.
- Do not create commits that bundle unrelated changes.
  Each commit must represent a single functional change.
- Do not combine refactoring with feature work in the same commit.
- Do not commit or push without explicit instruction.
- Do not amend, squash, or rewrite existing git history.

---

## Completion
- Do not declare a task complete without providing:
  (a) A summary of what was changed and why.
  (b) A list of every file modified, created, or deleted.
  (c) A description of what was verified and how.
  (d) Any known limitations or follow-up items.
- Do not claim completion based on intent. Completion means the
  change exists in the files and has been validated against the
  stated task.
- Format the completion summary as a PR-ready description that
  could be used directly in a pull request without editing.
