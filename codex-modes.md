---
applyTo: '**'
description: >
  Global operation mode rules for AI agents. Defines Answer / Plan / Act modes
  and their switching logic across all agents and projects.
---

# 🧠 AI CORE RULES

AI agents operate under **three distinct modes**:

1. **Answer Mode** — Default conversational mode.
   Used for explanations, Q&A, and reasoning **without** modifying any files or code.
   The agent responds directly without planning or editing.

2. **Plan Mode** — Triggered automatically when the user's request
   involves **creating, editing, refactoring, or deleting** code/files.
   The agent will outline a detailed plan **before** making any non-trivial modifications.

3. **Act Mode** — Activated **only after explicit user approval** (typing `ACT`)
   or immediately for simple tweaks or operations that skip Plan Mode.
   The agent executes the approved plan or simple change and applies all intended updates.
   After actions complete, it returns to **Answer Mode**.

---

## 🧱 Code Quality Principles

When creating or editing code, strictly enforce: **KISS**, **YAGNI**, **DRY**, **SOLID**.

Operational checks:

- In **Plan Mode**: include a brief "K-Y-D-S" principle check in the plan.
- In **Act Mode**: if a change violates any principle, refactor before finalizing; prefer minimal, incremental edits.
- Favor small, focused functions; clear naming; elimination of duplication.
- Follow existing repository conventions and language style guidelines.
- Add minimal tests for new or changed public behavior when feasible.
- Avoid introducing new dependencies unless strictly necessary.
- Keep public APIs stable unless explicitly requested to change.
- If deviating from K/Y/D/S, call it out and justify briefly in the plan.

---

## 🧩 Core Behavior Rules

- The default state is **Answer Mode**.
- Mode changes follow this logic:

  | From   | To     | Trigger                                                           |
  | ------ | ------ | ----------------------------------------------------------------- |
  | Answer | Plan   | The user's request requires code or file modifications            |
  | Plan   | Act    | The user types `ACT` to approve the plan                          |
  | Act    | Answer | All changes are completed successfully                            |
  | Any    | Plan   | The user types `PLAN` manually                                    |

- **Auto-Act exemption**: If the task is a **simple tweak** (e.g., small rename, trivial refactor, git commits) or if estimated modification size < 10 lines and all K-Y-D-S checks pass, the agent may auto-enter Act Mode without waiting for user approval.

- When in **Plan Mode**, the agent must:

  1. Always include the **entire, current plan** in every response.
  2. Remind the user that code execution requires explicit approval (`ACT`).

- When in **Act Mode**, the agent must:

  1. Perform the agreed modifications or simple tweaks without deviation.
  2. Provide concise summaries of each action.
  3. Automatically return to **Answer Mode** when done.

- All responses should start with a mode indicator:

`# Mode: ANSWER`
or
`# Mode: PLAN`
or
`# Mode: ACT`

- Follow the **user's language** and tone in all modes.

---

