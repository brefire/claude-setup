
# Claude Code as a System: A Developer's Guide to Reliable AI-Assisted Engineering

A prescriptive guide to implementing a philosophy where AI executes, you direct, and the system makes both accountable.

---

## The Philosophy

The core insight is simple: AI should be the executor, not the architect. The moment you hand over decision-making about scope, design, or what counts as done, you lose the ability to trust the output.

- Think of AI as a high-performance race car: extraordinary capability, but dangerous without discipline. You are the driver. Your configuration system is the circuit. Every task is a defined lap with a clear start, a specified route, and a finish line you can verify.
- Small, scoped tasks with clear boundaries prevent hallucination and scope creep.
- Negative constraints over positive instructions: define the boundaries, then build audits and remediation protocols to enforce them.
- The larger and vaguer the task, the more fragile the system becomes.

This matters more, not less, as you scale. A single misaligned agent in a multi-agent pipeline can propagate bad assumptions and missteps across every downstream step. The same principles that make a single Claude Code session reliable are what make a network of agents trustworthy.

---

## The Configuration Architecture

Claude Code's configuration system has three levels. Understanding how they compose is the foundation of everything else.

| Level | Path | Purpose |
|---|---|---|
| Global | `~/.claude/` | Your engineering principles. These never change. |
| Project | `.claude/` | Project-specific context: architecture, patterns, conventions. |
| Local | `.claude/settings.local.json` | Personal preferences. Not shared. |

Global rules define what the model is never allowed to do. Project rules define how it should behave in a specific codebase. Local rules are yours alone. Each level inherits from the one above, overriding downward, never upward.

If you're using both Claude Code and Cursor, this setup pays a compounding dividend: both tools read the same `.claude/` directory. Claude commands translate directly to Cursor skills. Your constraints, your principles, and your philosophy are consistent across both interfaces. You can cross-validate the same task through both tools using the same framework. One system, two surfaces.

---

## The Global CLAUDE.md

The global `CLAUDE.md` is the most important file in this system. It defines how the model reasons about every task before touching a single line of code. Without it, behavior is model-dependent. With it, behavior is deterministic.

The rules are organized around failure modes — the specific ways AI-assisted development goes wrong in practice:

- **Task Integrity**: A valid task has a single functional change, clear intent, clear location, clear output, and can be completed and verified in one pass. The model is required to reject tasks that don't meet this definition.
- **Scope and Judgement**: Work only on what was asked. Do not refactor, rename, or improve adjacent code. Scope creep is the most common way a session goes sideways.
- **Ambiguity Resolution**: When a task has more than one valid interpretation, do not guess. State what's ambiguous, list the interpretations, stop, and wait.
- **Verification Before Execution**: Before writing any code, restate what will change, which files are in scope, and what is explicitly out of scope.
- **Error Remediation**: When something unexpected happens, stop. State the error, restate the original scope, propose a path forward, and wait for direction. Do not expand scope to fix an error.
- **Output Quality**: No lint suppression, no `any` types, no hardcoded values, no placeholders. Code must be production-quality or the task is not done.
- **Git Rules**: No direct commits to main, no bundling unrelated changes, no push without explicit instruction.
- **Completion Protocol**: Every task ends with a PR-ready summary: what changed, which files were touched, what was verified, what the known limitations are.

---

## Commands

Commands are the interface between you and the model's behavior. Each one encodes a workflow, a specific sequence of reasoning and action appropriate to the type of task. Rather than writing the same instructions repeatedly, you invoke a command and the model knows exactly what protocol to follow.

| Command | Purpose |
|---|---|
| `/implement` | Scoped implementation with verification loop |
| `/fix-bug` | Analyze, restate, fix only the bug |
| `/review` | Analysis only, no changes |
| `/analyze` | Understand architecture before planning |
| `/breakdown` | Decompose work into executable tasks; human reviews before execution |
| `/create-pr` | Generate PR summary with scope compliance checklist |
| `/onboard-project` | Scan repo, produce project CLAUDE.md draft |
| `/quick-fix` | Trivial corrections without the full verification loop |

The `/breakdown` command is especially important for agentic work. Before any task is executed, it can be passed through `breakdown` to produce a sequence of valid, scoped subtasks. Each subtask is then a clean handoff to you, to another model, or to a downstream agent.

---

## Agents: Read-Only Enforcement

Agents in this system are specialists with constrained access. Every agent is restricted to `Read`, `Grep`, and `Glob` — no write access, by design. An agent that cannot modify files cannot break anything. It can only observe and report.

| Agent | Responsibility |
|---|---|
| `code-reviewer` | Quality, consistency, adherence to project principles |
| `security-reviewer` | Vulnerabilities, injection risks, auth issues, data exposure |
| `scope-auditor` | Verify completed work stays within CLAUDE.md boundaries |
| `dependency-analyst` | Research packages and present facts; human makes the decision |
| `test-analyst` | Identify coverage gaps; never writes tests |

The read-only constraint is not a limitation. It is what makes these agents safe to run automatically, in parallel, or as post-execution checks in a pipeline.

**On model selection:** Use more capable models (Opus, Sonnet) for planning, task decomposition, and ambiguity resolution, where reasoning quality matters most. Use faster, cheaper models for execution once a task is well-defined and scoped. A well-scoped task is a well-scoped task regardless of which model executes it. This is how you scale throughput without scaling cost linearly.

---

## The Workflow Loop

Every task, regardless of size or complexity, runs through the same loop. This is what makes the system consistent and auditable — you can inspect any step, at any point.

1. You decompose the work into valid tasks.
2. The model verifies each task against CLAUDE.md.
3. The model restates scope and waits for approval.
4. You approve.
5. The model executes.
6. The model reports completion with a PR-ready summary.
7. The scope auditor verifies adherence post-execution.

---

## In Practice

The commands and agents in this system are not used in isolation. They map to situations. The following scenarios show how to sequence them for common engineering workflows.

---

**Starting fresh on a new codebase**

Before writing any code, understand what you are working with.

1. Run `/analyze` to scan the architecture, identify patterns, and surface what the codebase is doing and how.
2. Run `/onboard-project` to generate a project-level CLAUDE.md based on what was found. Review the output before accepting it. This becomes the model's operating contract for the project.

---

**Implementing a feature**

Never start with implementation. Start with decomposition.

1. Run `/breakdown` on the feature description. Review the resulting task list. Reject or refine any task that is too vague or too large.
2. For each task, run `/implement`. The model will restate scope before touching any code.
3. After each implementation, run the `code-reviewer` agent to verify quality and consistency.
4. When all tasks are complete, run `/create-pr` to generate a PR summary with a scope compliance checklist.

---

**Fixing a bug**

The goal is to fix the bug, not the surrounding code.

1. Run `/fix-bug` with a clear description of the observed behavior and where it occurs. The model will analyze, restate the scope, and fix only what is broken.
2. Run the `scope-auditor` agent post-fix to confirm nothing outside the stated scope was changed.
3. If the bug is in authentication, data handling, or any security-sensitive path, also run `security-reviewer`.

---

**Reviewing before a merge**

Use agents for review, not the main session. They are read-only by design and cannot introduce changes.

1. `code-reviewer` for quality, consistency, and adherence to project conventions.
2. `security-reviewer` for vulnerability surface, injection risks, and data exposure.
3. `dependency-analyst` if new packages were added. It will surface facts about the dependency for you to make the decision.

---

**Decomposing work for a multi-agent pipeline**

`/breakdown` is the entry point for any agentic workflow.

1. Run `/breakdown` on the full body of work. The output is a sequence of valid, scoped tasks, each one a clean handoff.
2. Each task in the sequence goes through the full workflow loop: verify, approve, execute, report, audit.
3. The `scope-auditor` runs post-execution on every task. In a pipeline, this is your checkpoint before the next task begins.

---

**The system handling a multi-task prompt**

Most real prompts contain more than one task. They arrive as a stream of thought, not a scoped specification. The system's first job is to parse that stream and identify where one task ends and another begins.

A developer sends this prompt:

> "Add a loading spinner to the data table on the reports page and also clean up the API layer while you're at it."

The model identifies two tasks:

- **Task 1:** Add a loading spinner to the data table on the reports page. Valid: single functional change, clear location, identifiable output. The model reads the relevant component, restates what it will change and what it will not touch, and executes.
- **Task 2:** Clean up the API layer. Not yet valid: "clean up" has no defined output. It could mean removing unused endpoints, standardizing error handling, improving naming, adding types, or restructuring files. Two developers would produce entirely different results from the same instruction.

Task 1 is executed. Task 2 is deferred.

**The ambiguity resolution loop**

When the developer returns to Task 2:

> "ok let's do the API cleanup now"

The model does not begin. It surfaces the ambiguity: what does "clean up" mean in this context? It lists four possible interpretations and asks which applies. The developer answers, but only partially. The model acknowledges what has been resolved, identifies what is still missing, and waits.

Only after the scope is fully defined does the model produce a scope statement and begin work.

**What the system enforced**

Three mechanisms worked in sequence:

1. **Task identification is automatic.** The model parsed a stream-of-thought prompt and separated it into discrete tasks. This behavior is encoded in the global CLAUDE.md task integrity rules, not inferred case by case.

2. **Ambiguity is surfaced, not resolved by assumption.** At no point did the model guess what "clean up" meant and begin working. It listed specific interpretations, identified missing information, and stopped.

3. **Scope restatement gates execution.** Every task that was executed was preceded by an explicit statement of what would change, which files were in scope, and what was out of scope. The developer had a checkpoint before anything was written.

This is the workflow loop running as designed. The commands are the interface. The CLAUDE.md is the enforcement layer underneath.

---

## Closing

This is about controlling AI. Not constraining it for its own sake, but putting guardrails in place that translate intent into predictable, auditable outputs. When the system is working, you do not wonder what the model did or hope it stayed in scope. You know, because the protocol requires it to tell you.

The philosophy scales. A single session becomes a pipeline. A pipeline becomes a system. The rules do not change. The stakes do.

The setup takes time. The payoff is every task after that.
