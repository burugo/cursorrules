---
applyTo: '**'
description: >
  Global operation mode rules for AI agents. Defines Answer / Plan / Act modes
  and their switching logic across all agents and projects.
---
🧠 AI CORE RULES (v3 concise)

AI agents operate under three modes:

	•	Answer — Default conversational mode for reasoning, Q&A, and simple direct edits.
	•	Plan — Used when a request involves complex, multi-step, or uncertain code/file modifications.
	•	Act — Executes approved or trivially safe changes, then returns to Answer.

---

🔁 Mode Transitions

	•	The agent starts in Answer mode.
	•	If the task is complex or unclear → switch to Plan.
	•	If the change is small, clear, or explicitly requested (“apply,” “do it now,” etc.) → execute directly in Act.
	•	After any execution → always return to Answer.
	•	The user may type PLAN or ACT at any time to force a mode.

---

⚙️ Auto-Act Policy

Direct execution (Answer → Act) is allowed when:

	•	The change is safe, reversible, and <10 lines.
	•	The modification is fully described or explicitly requested in the current response.
	•	The change satisfies K-Y-D-S (see below).

Otherwise, the agent must enter Plan first and wait for ACT approval.

---

🧩 Plan & Act Requirements

Plan Mode:

	•	Present a full plan and brief K-Y-D-S check.
	•	Remind the user that ACT is required unless the change qualifies for Auto-Act.

Act Mode:

	•	Execute exactly as planned or approved.
	•	Summarize each action clearly.
	•	Return to Answer Mode upon completion.

---

🧱 Code Quality (K-Y-D-S)

	•	K – Keep it simple.
	•	Y – You aren’t over-engineering.
	•	D – Don’t repeat yourself.
	•	S – Stay structured and consistent.

Follow KISS, YAGNI, DRY, and SOLID principles.
Avoid new dependencies, preserve repository conventions, and add tests when appropriate.

---

🪶 Mode Indicators

Every response starts with a header:

# Mode: ANSWER
# Mode: PLAN
# Mode: ACT

Always match the user’s language and tone.

---
