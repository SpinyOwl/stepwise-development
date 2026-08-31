---
name: stepwise-brainstorm
description: Explore and converge on a software design before planning. Use when a user asks to brainstorm or compare approaches, or when a change has materially unclear behavior, competing architecture choices, broad scope, or high-impact tradeoffs; the skill gathers context, asks bounded questions, compares approaches, gains design approval, and invokes stepwise-planner in the same context with a reviewed brief.
---

# Stepwise Brainstorm

## Outcome

Turn an ambiguous or high-impact request into an approved design and a reviewed Stepwise plan without implementing code. Conversation-only design work is the default; persist brainstorm artifacts only when the user explicitly approves them. After design approval, invoke the `stepwise-planner` skill in the current context rather than delegating to a named planner agent; require another approval only if the planning brief introduces a new material decision.

Keep semantic design decisions in the approved brief and authoritative Markdown
artifacts. Do not copy private reasoning or vendor-specific orchestration into
the brief.

## Golden Rule: YAGNI

Apply YAGNI (You Aren't Gonna Need It) as the default scope and design
principle throughout brainstorming. Design for the concrete problem and
validated requirements at hand, not for speculative future needs:

- Challenge requests, abstractions, extension points, configuration, and
  compatibility layers that do not serve a current, evidenced use case.
- Prefer the smallest design that satisfies the stated goal and acceptance
  criteria. Do not add work merely because it might be useful later.
- Separate confirmed current needs from possible future ideas. Record the
  latter as non-goals or follow-up candidates instead of including them in the
  approved scope.
- When comparing approaches, treat unnecessary complexity, implementation
  surface, maintenance cost, and unvalidated flexibility as explicit
  drawbacks.
- Revisit YAGNI during design approval and planning-brief review to detect
  scope creep before implementation begins.

## Documentation Navigation

Before exploring project context:

1. Read the project's `AGENTS.md` or equivalent instruction file.
2. Read `docs/README.md` when it exists.
3. Read `docs/folder-structure.md` when it exists; otherwise read the bundled
   `references/folder-structure.md` in this skill.
4. Follow the relevant section index and explicit document links only. Do not
   scan every document under `docs/`.

Use the shared folder convention for all documentation paths. Exploratory
brainstorming artifacts belong in `docs/drafts/`; coherent proposals belong in
`docs/design/`. Do not create either optional section unless the user asks for
a durable artifact or the project already uses that section.

## Workflow

1. Explore project context.
   - Read applicable repository instructions, existing plans, architecture documents, relevant source code, tests, and current conventions.
   - Start from `docs/README.md` and follow the smallest relevant document chain to the current epic, milestone, phase, or task.
   - Read linked architecture, decision, design, and reference documents only when they are relevant to the request.
   - Summarize only context that materially affects the decision.
   - Separate established facts from assumptions and unknowns.
    - Keep project access read-only during design exploration except for
      explicitly user-approved draft, design, or mockup artifacts and their
      required index updates. Planner-created plan artifacts are permitted
      later only through the approved same-context `stepwise-planner`
      invocation.

2. Clarify the problem.
   - Skip clarification when project context and the user's request already establish the decisions needed for a responsible recommendation.
   - Ask only questions whose answers materially affect scope, behavior, constraints, tradeoffs, or validation.
   - Group logically related questions and ask no more than three questions in one message.
   - Prefer questions that offer concrete options when the project context makes options clear.
   - Give only the context needed to explain why each question matters and what decision it informs, normally one or two sentences.
   - When a question has answer options, explain each option's main implication concisely; do not use unclear labels or repeat context shared by all options.
   - Do not repeat information already supplied by the user or established by project context.
   - Use no more than two clarification rounds by default. After that, proceed with clearly labeled assumptions when safe; continue asking only when an unresolved material decision would make a recommendation irresponsible.

3. Propose approaches.
   - Present two or three viable approaches unless the context leaves only one responsible option.
   - For each approach, explain the core idea, affected boundaries, benefits, tradeoffs, risks, and validation implications.
   - Recommend an approach and state the reasoning without treating it as approved.
   - Use a Mermaid diagram when it materially improves understanding of architecture, data flow, state transitions, user flow, ownership boundaries, or dependencies. Do not add diagrams as decoration.
   - When a visualization, concept, or comparison would materially improve the decision, offer to create a lightweight HTML page or small set of HTML variants and open them in a browser for review. If a connected Figma capability is available, also offer a Figma artifact as an alternative.
   - Create the selected visual artifact only after the user agrees. Keep it explicitly disposable and limited to exploring the current design decision; do not treat it as production code, a component implementation, or permission to expand scope.

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
   - Ask the next logically grouped set of at most three questions, applying the same per-question context and per-option explanation requirements, then revise the approaches and design.
    - Do not prepare a plan or invoke `stepwise-planner` until the user explicitly approves the design.
    - If the user requests a durable record of an unresolved exploration, store
     it in `docs/drafts/` and update the nearest index.
   - If the user requests a durable approved or active proposal, store it in
     `docs/design/` and update the nearest index.
   - Include a `Document Context` section and links to the relevant document
     chain in any persisted artifact.

6. Prepare and review the planning brief.
   - Convert the approved design into a concise planning brief for `stepwise-planner`.
   - Include the approved goal, non-goals, chosen approach, constraints, relevant context, risks, validation expectations, and resolved decisions.
   - State the intended planning level: high-level for roadmap, milestone, phase, or architectural direction; low-level for a concrete feature, fix, or refactor with known behavior.
   - Self-review the brief against the approved design for scope drift, missing constraints, premature implementation detail, and unresolved decisions.
   - Do not request a second approval when the brief faithfully restates the approved design. Ask only if preparing the brief exposes a new material decision or changes approved scope.
   - Keep the planning brief in the conversation rather than creating a separate brief artifact. Preserve any draft or design artifact the user already approved under the artifact exceptions.

7. Invoke and review Stepwise Planner in the same context.
   - Invoke the `stepwise-planner` skill in the current agent context with the reviewed planning brief. Do not delegate planning to a separate agent.
   - Reuse the project context already gathered during brainstorming. Supply a concise reference to that context and inspect only newly required information; do not restart discovery or redundantly reload the same files.
   - Allow `stepwise-planner` to create or update the plan artifacts required by its planning workflow. This is a read-only brainstorm exception after design approval, not permission to edit implementation files.
   - Review the resulting plan before presenting it to the user.
   - Confirm that the plan matches the approved goal, non-goals, chosen approach, constraints, risks, validation strategy, and intended planning level.
   - Confirm that dependency metadata, task or phase boundaries, and Mermaid diagrams are internally consistent when present.
   - If the plan is misaligned, incomplete, over-scoped, or internally inconsistent, reapply `stepwise-planner` in the same context to revise it before presenting it.
   - Present the reviewed plan, the verification of its design alignment, and any remaining decisions or risks.

## Delegated Invocation

When this skill is running inside a subagent invoked by another agent, the
subagent must keep clarification context visible to the calling agent:

- Do not ask the end user questions directly or hide the rationale in an
  internal tool call. Return each question as part of a clarification package
  for the calling agent.
- For every question, include the brief required context and concise option
  implications.
- Tell the calling agent that it must present the full clarification package
  to the user without reducing it to unexplained question text or labels.
- Wait for the calling agent to provide the user's answers before continuing
  the clarification process. The calling agent should include those answers
  in the next delegated invocation so unresolved decisions and their context
  remain connected.

When this skill is running in the primary agent, present the clarification
package directly to the user.

## Approval Gates

- Design approval: required before invoking `stepwise-planner`.
- Planning brief approval: required only when the brief introduces a new material decision or changes approved scope.
- Planner output is reviewed for alignment before it is presented as the final plan.

## Constraints

- Do not implement code, make unrelated project edits, stage changes, or create commits.
- Project access remains read-only except for explicitly user-approved draft or design documents and their required index updates, explicitly user-approved lightweight mockups, and plan artifacts created or updated while applying `stepwise-planner` after design approval.
- Keep mockups isolated from production code and remove or discard them when they are no longer useful unless the user explicitly approves them as durable artifacts.
- Do not persist other brainstorming artifacts without explicit user approval.
- Do not present a recommendation as a user decision.
- Do not invoke `stepwise-planner` until the design is explicitly approved.
- Do not create a redundant approval gate for a faithful restatement of the approved design.

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
- Reviewed planning brief summary
- Planning level selected and why
- `stepwise-planner` review result
- Remaining decisions, risks, or follow-up work
