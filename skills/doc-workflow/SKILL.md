---
name: doc-workflow
description: Generic Gather → Iterate → Audit → Resolve → Critical Check → Write → Skill Improvement → Calling Instructions Improvement workflow for creating or updating any structured document (requirements docs, test plans, design specs, agent instructions, etc.) issue-by-issue rather than in bulk. Use whenever the user asks to create, write, draft, or update this kind of document, even without saying "review" or "workflow" explicitly. Defines the review PROCESS only — sections, formatting, and audit criteria come from the document type itself or the user's stated preferences.
---

# Document Workflow

A structured, issue-by-issue process for drafting or updating any document. This skill defines the *process* — the document type's own skill (or the user's stated preferences, if there is no specific skill) defines the *content*: what sections exist, what format the output is, and what to check for during Audit.

Draft work happens in the conversation only — the output file is not created or overwritten until Write (6) confirms it.

Stage transitions require explicit confirmation. When a stage's work is complete, name the next stage and ask the user to confirm before starting it (e.g. "Ready to move to Audit (3)?") — never advance automatically. When a new stage begins, label it at the start of that output as "WORKFLOW STAGE: <Name>" (e.g. "WORKFLOW STAGE: Audit (3)").

The user can move back to any earlier stage at any time to do more work (e.g., return to Iterate (2) after Audit (3) has started) — there is no restriction on revisiting a prior stage. Resume forward from wherever the user left off.

When presenting multiple items for review (the section/content list in Iterate (2), or Audit (3) findings in Resolve (4)), always give a summary first tagging each item New, Update, or Remove, then review items one at a time — never in bulk. Present each item with the following fields, each label on its own line, in all caps, with no colon after the label: CONTEXT, PROBLEM, BEFORE, AFTER, REASON, SIDE-EFFECT, ACTION. If BEFORE or AFTER quotes exact document text, show it in a code block. REASON should include enough context (what prompted the change and why it matters) to stand on its own, not just a one-line justification. SIDE-EFFECT notes any other part of the document this change affects; if present, it is automatically added to the backlog (see Backlog below) rather than requiring separate approval here. ACTION is one of: Approve, Reject, Update, or Park/Later (which adds the item to the backlog). Wait for the user's ACTION before moving to the next item.

**Backlog**: Items marked Park/Later, and SIDE-EFFECT entries recorded on items reviewed during that stage, are added to a running backlog list during Iterate (2) or Resolve (4). Iterate (2) cannot advance to Audit (3), and Resolve (4) cannot advance to Critical Check (5), while any backlog item raised in that stage remains open. Backlog items are re-presented using the same review format (CONTEXT/PROBLEM/BEFORE/AFTER/REASON/SIDE-EFFECT/ACTION) and closed via Approve, Reject, or Update.

## Gather (1)

Establish what needs to be created or changed — ask if not already stated. For a new document: fill in sections one at a time, against the document type's defined section list as-is — don't pause to evaluate whether that section list itself needs changing. For an update to an existing document: gather the specific list of changes or feedback to address.

Don't fabricate specific values (numbers, thresholds, names, dates) the user hasn't given you — flag them as placeholders (e.g. `[TBD — confirm X]`) rather than inventing them.

Whenever it's unclear which section content belongs in, ask rather than deciding unilaterally.

Output of this step: a short numbered summary list (1–2 sentences per item) of the sections/content items to work through in Iterate (2). Show the list and confirm with the user before moving to Iterate (2).

## Iterate (2)

Work with the user until they confirm the document is good (e.g. "this looks good"). No changes to the section list itself during this phase.

If new items are found during this phase, add them to the list rather than resolving them immediately — work through the list in order (FIFO) so nothing is missed or handled out of sequence.

Cannot advance to Audit (3) while backlog items from this stage remain open — see Backlog.

## Audit (3)

Run a full check against the draft. Produce a numbered list where each item contains the issue and a suggested fix. Check against the criteria the document type defines (e.g. a quality checklist, a words-to-avoid list, structural rules) plus these process-level checks that apply regardless of document type:
- **Ambiguity** — any instruction that could be interpreted more than one way
- **Duplication** — the same content appearing in more than one place; identify which location should be the single source of truth and suggest removing/deferring elsewhere
- **Errors** — inconsistent, contradicts other content, or missing something the document type requires
- **References** — every pointer from one part of the document (or its bundled files) to another resolves to something that actually exists

For an update to an already-written document, scope the audit to the changed content and its interactions with the rest of the document, not the full previously-approved baseline.

Present the complete list before making any changes.

## Resolve (4)

Review the Audit (3) findings per the review format defined above. Apply only approved (or user-updated) changes. Before declaring a batch complete, cross-check the count and identifiers of items actually resolved against the original source list (e.g., the input change list or Gather-phase item list) — do not rely on sequential item-by-item narration alone to confirm full coverage, since an item can be silently skipped mid-sequence without the omission being obvious from the running commentary. Items closed via Park/Later count as accounted-for in this cross-check, not as omissions.

Cannot advance to Critical Check (5) while backlog items from this stage remain open — see Backlog.

## Critical Check (5)

Re-run Audit (3) for critical or serious issues only. If any are found, go to Resolve (4). If none, confirm with the user before moving to Write (6).

## Write (6)

Show a summary diff of what will change in the file (or the full draft, for a new document). This requires its own confirmation, separate from the stage-transition confirmation already given to enter this stage — only after the user confirms the write itself, write the file to the document type's defined output location and present it.

If the document type defines version/revision-tracking fields (e.g. a Version number, a Revision History section), update them as part of this step, per that document type's own rules.

## Skill Improvement (7)

Runs only after Write (6) completes. Review this session for any gap in the calling document-type skill itself (or in this workflow skill, if the process itself caused the friction). Tag each finding:
- **[MINOR]** — a one-off preference, wording tweak, or cleanup specific to this document. Note it, but do not propose a SKILL.md change for it.
- **[MAJOR]** — a gap with meaningful, recurring impact across future documents of that type — not just this session's specific case.

Propose SKILL.md changes only for [MAJOR] findings — the specific change, what and why, and which skill file it belongs in — and only edit it if the user approves. If no [MAJOR] findings, say so, with a one-line mention of any [MINOR] items noted, rather than skipping the step silently. Skills are meant to stay universal across all documents of that type; don't propose a change that would make one more restrictive or narrow it to this session's specific case.

## Calling Instructions Improvement (8) — when invoked from separate process instructions

Runs only after Skill Improvement (7) completes, and only when this session is operating within a project that has its own instructions document (e.g. a project set up to develop custom GPT instructions, or a project set up to develop AI skills) — not by searching a repository for one; if no such project instructions document governs this session, skip this step entirely. Review this session's process against that calling document, using the same [MINOR]/[MAJOR] tagging as Skill Improvement (7):
- **[MINOR]** — note it, but do not propose an edit.
- **[MAJOR]** — a reusable process step or clarification worth adding, an existing instruction that caused real ambiguity or friction, or an instruction that proved unnecessary or harmful.

Propose [MAJOR] changes to the user explicitly — what would change and why — and only update if they approve. If no [MAJOR] findings, say so, with a one-line mention of any [MINOR] items noted, rather than skipping the step silently. Skip this step entirely if there is no separate calling instructions document.
