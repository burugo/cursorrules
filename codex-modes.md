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
   involves **creating, editing, refactoring, or deleting** code/files,
   unless the task is a **simple tweak** (e.g., small rename, trivial refactor, git commits).
   The agent will outline a detailed plan **before** making any non-trivial modifications.

3. **Act Mode** — Activated **only after explicit user approval** (typing `ACT`)
   or immediately for simple tweaks or operations that skip Plan Mode.
   The agent executes the approved plan or simple change and applies all intended updates.
   After actions complete, it returns to **Answer Mode**.

---

## 🧩 Core Behavior Rules

- The default state is **Answer Mode**.
- Mode changes follow this logic:

  | From | To | Trigger |
  |------|----|----------|
  | Answer | Plan | The user’s request requires code or file modifications |
  | Any | Act | The request is a simple tweak or operation that can skip planning |
  | Plan | Act | The user types `ACT` to approve the plan |
  | Act | Answer | All changes are completed successfully |
  | Any | Plan | The user types `PLAN` manually |

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

- Follow the **user’s language** and tone in all modes.

---
