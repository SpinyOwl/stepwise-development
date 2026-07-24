---
name: stepwise-brainstorm
description: Explore and converge on a software design before planning. Use when a user asks to brainstorm a feature, change, architecture, workflow, or product behavior; the skill gathers context, asks focused questions, compares approaches, gains design approval, and hands an approved planning brief to stepwise-planner.
---

# Stepwise Brainstorm

## Outcome

Turn an ambiguous request into an approved design and a reviewed dependency-graph plan without implementing code or persisting brainstorming artifacts. Use `stepwise-planner` only after the user approves both the design and the planning brief.

## Workflow

1. Explore project context.
   - Read applicable repository instructions, existing plans, architecture documents, relevant source code, tests, and current conventions.
   - Summarize only context that materially affects the decision.
   - Separate established facts from assumptions and unknowns.
   - Do not edit code, configuration, documentation, or planning files during brainstorming.

2. Clarify the problem.
   - Ask only questions whose answers materially affect scope, behavior, constraints, tradeoffs, or validation.
   - Group logically related questions and ask no more than three questions in one message.
   - Prefer questions that offer concrete options when the project context makes options clear.
   - Do not repeat information already supplied by the user or established by project context.
   - If essential answers remain unknown, continue this step after each user response.

3. Propose approaches.
   - Present two or three viable approaches unless the context leaves only one responsible option.
   - For each approach, explain the core idea, affected boundaries, benefits, tradeoffs, risks, and validation implications.
   - Recommend an approach and state the reasoning without treating it as approved.
   - Use a Mermaid diagram when it materially improves understanding of architecture, data flow, state transitions, user flow, ownership boundaries, or dependencies. Do not add diagrams as decoration.

4. Present the design.
   - Present a concise, decision-ready design with these sections:
     - Goal
     - Non-goals
     - Context and constraints
     - Chosen approach
     - Architecture or behavior
     - User experience or API behavior, when relevant
     - Data, state, or integration implications, when relevant
     - Risks and mitigations
     - Validation strategy
     - Open questions
   - Clearly label assumptions and unresolved decisions.
   - Ask the user to approve the design or identify changes needed.

5. Iterate until design approval.
   - If the user rejects the design or requests material changes, return to clarification.
   - Ask the next logically grouped set of at most three questions, then revise the approaches and design.
   - Do not prepare a plan or invoke `stepwise-planner` until the user explicitly approves the design.

6. Prepare and review the planning brief.
   - Convert the approved design into a concise planning brief for `stepwise-planner`.
   - Include the approved goal, non-goals, chosen approach, constraints, relevant context, risks, validation expectations, and resolved decisions.
   - State the intended planning level: high-level for roadmap, milestone, phase, or architectural direction; low-level for a concrete feature, fix, or refactor with known behavior.
   - Self-review the brief against the approved design for scope drift, missing constraints, premature implementation detail, and unresolved decisions.
   - Present the brief to the user for review and request approval.
   - Keep the design and planning brief in the conversation; do not create a project artifact at this stage.

7. Invoke and review Stepwise Planner.
   - Invoke `stepwise-planner` with the user-approved planning brief and relevant project context.
   - Review the resulting plan before presenting it to the user.
   - Confirm that the plan matches the approved goal, non-goals, chosen approach, constraints, risks, validation strategy, and intended planning level.
   - Confirm that dependency metadata, task or phase boundaries, and Mermaid diagrams are internally consistent when present.
   - If the plan is misaligned, incomplete, over-scoped, or internally inconsistent, ask `stepwise-planner` to revise it before presenting it.
   - Present the reviewed plan, the verification of its design alignment, and any remaining decisions or risks.

## Approval Gates

- Design approval: required before preparing a planning brief.
- Planning brief approval: required before invoking `stepwise-planner`.
- Planner output is reviewed for alignment before it is presented as the final plan.

## Constraints

- Do not implement code, make project edits, stage changes, or create commits.
- Do not persist brainstorming artifacts unless the user explicitly asks for a document.
- Do not present a recommendation as a user decision.
- Do not skip approval gates because a design seems obvious.
- Do not invoke `stepwise-planner` until the planning brief is explicitly approved.

## Output Markers

Use emojis sparingly in user-facing headings and result labels, never as decoration or on every bullet:

- `📋` plan, scope, or planning brief
- `🔍` investigation or review
- `💡` recommendation or decision
- `✅` verified completion or passed result
- `✔️` individual checked item or affirmative confirmation
- `⚠️` risk, limitation, or non-blocking concern
- `❗` action needed or material decision required
- `✖️` intentionally not applicable, rejected option, or skipped item
- `❌` failure, blocker, or unmet requirement
- `🧪` automated or manual verification

Keep task IDs, markdown checkboxes, commands, filenames, code, and Mermaid diagrams plain text. In final responses, use `💡` for recommendations and `❗` for required design or planning-brief approval.

## Final Response

When the reviewed plan is ready, report:

- Approved design summary
- Approved planning brief summary
- Planning level selected and why
- `stepwise-planner` review result
- Remaining decisions, risks, or follow-up work
