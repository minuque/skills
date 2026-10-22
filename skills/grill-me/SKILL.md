---
name: grill-me
description: Grill the user relentlessly about a plan, decision, or idea. A relentless interview to sharpen a plan or design. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask now without guessing at answers you haven't heard yet. Ask the frontier in one `ask_user_question` call, then wait. Do not write the questions as a chat template.

One call holds at most 4 questions. If the frontier is larger, ask the 4 that unblock the most. Each question has 2 to 4 options. Put the recommended option first and append `(Recommended)` to its label. Do not add an "Other" option. A question whose answer depends on another question still open in this round belongs to a later round.

Each round the user answers reshapes the tree. Recompute the frontier and ask the next round.

Finding facts is your job, never the user's. Look them up in this session. Do not dispatch a subagent for a fact one or two searches can answer. Do not ask the user for anything you could look up. The decisions are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
