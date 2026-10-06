---
name: workflow-instructions
description: Generic Gather → Iterate → Audit → Resolve → Critical Check → Write → Process Improvement workflow for creating or updating any structured document (requirements docs, test plans, design specs, agent instructions, etc.) issue-by-issue rather than in bulk. Use whenever the user asks to create, write, draft, update, edit, rename, or restructure this kind of document (including changes that span several files), even without saying "review" or "workflow" explicitly. Defines the review PROCESS only — sections, formatting, and audit criteria come from the document type itself or the user's stated preferences.
---


WORKFLOW INSTRUCTIONS

A structured, issue-by-issue process for drafting or updating any document. This skill defines the process — the document type's own skill (or the user's stated preferences, if there is no specific skill) defines the content: what sections exist, what format the output is, and what to check for during Audit.

Draft work happens in the conversation only — the output file is not created or overwritten until Write (6) confirms it.

Stage transitions require explicit confirmation:

- When a stage's work is complete, name the next stage and ask the user to confirm before starting it (e.g. "Ready to move to Audit (3)?").
- Never advance automatically.
- When a new stage begins, label it at the start of that output as "WORKFLOW STAGE: <Name>" (e.g. "WORKFLOW STAGE: Audit (3)").

The user can move back to any earlier stage at any time to do more work (e.g., return to Iterate (2) after Audit (3) has started) — there is no restriction on revisiting a prior stage. Resume forward from wherever the user left off.

Review format: when presenting multiple items for review (the section/content list in Iterate (2), or Audit (3) findings in Resolve (4)):

1. Order the list so that any item another item depends on comes first, then number the items sequentially in that order. If two or more items depend on each other, merge them into one item.
2. Give a summary first, tagging each item New, Update, or Remove.
3. Review items one at a time, strictly in numbered order — never in bulk, and never out of the numbered sequence.
4. Present each item with the following fields, each label on its own line, in all caps, with no colon after the label: CONTEXT, PROBLEM, BEFORE, AFTER, REASON, SIDE-EFFECT, ACTION.
5. Wait for the user's ACTION before moving to the next item.

Each field follows these rules:

- If BEFORE or AFTER quotes exact document text, show it in a code block.
- Copy BEFORE text directly from the raw file content and confirm it matches exactly before presenting it; never reconstruct it from memory or a rendered view, since an inaccurate BEFORE can set a wrong premise for the change.
- If the text was changed by an earlier approved item not yet written, copy BEFORE from the current draft instead, state "Approved draft (not yet written)" on the line above its code block, and confirm it matches exactly.
- REASON should include enough context (what prompted the change and why it matters) to stand on its own, not just a one-line justification.
- SIDE-EFFECT notes any other part of the document this change affects; if present, it is automatically added to the backlog (see Backlog below) rather than requiring separate approval here.
- ACTION is one of: Approve, Reject, Update, or Backlog.
- For an item tagged New, state PROBLEM as N/A (this is new content) and leave BEFORE empty.

Backlog: a running list kept during Iterate (2) and Resolve (4).

- Items marked Backlog, and SIDE-EFFECT entries recorded on items reviewed during that stage, are added to the list.
- Iterate (2) cannot advance to Audit (3), and Resolve (4) cannot advance to Critical Check (5), while any backlog item raised in that stage remains open.
- Backlog items are re-presented using the same review format (CONTEXT/PROBLEM/BEFORE/AFTER/REASON/SIDE-EFFECT/ACTION) and closed via Approve, Reject, or Update.


GATHER (1)

Establish what needs to be created or changed — ask if not already stated.

- For a new document: fill in sections one at a time, against the document type's defined section list as-is. Don't pause to evaluate whether that section list itself needs changing.
- For an update to an existing document: gather the specific list of changes or feedback to address.

Don't fabricate specific values (numbers, thresholds, names, dates) the user hasn't given you — flag them as placeholders (e.g. [TBD — confirm X]) rather than inventing them.

Whenever it's unclear which section content belongs in, ask rather than deciding unilaterally.

Output of this step: a short numbered summary list (1–2 sentences per item) of the sections/content items to work through in Iterate (2), ordered and numbered per Review format. Show the list and confirm with the user before moving to Iterate (2).


ITERATE (2)

Work with the user until they confirm the document is good (e.g. "this looks good"). No changes to the document type's defined section list itself during this phase.

If new items are found during this phase, add them to the list rather than resolving them immediately — work through the list in order (FIFO) so nothing is missed or handled out of sequence. Exception: if a new item is a dependency of an item not yet approved (including the item currently under review), insert it before that item, renumber the remaining unapproved items, pause the dependent item, and show the updated list before continuing.

Cannot advance to Audit (3) while backlog items from this stage remain open — see Backlog.


AUDIT (3)

Run a full check against the draft. Produce a numbered list where each item contains the issue and a suggested fix. Check against the criteria the document type defines (e.g. a quality checklist, a words-to-avoid list, structural rules) plus these process-level checks that apply regardless of document type:

- Ambiguity: any instruction that could be interpreted more than one way
- Duplication: the same content appearing in more than one place; identify which location should be the single source of truth and suggest removing/deferring elsewhere
- Errors: inconsistent, contradicts other content, or missing something the document type requires
- References: every pointer from one part of the document (or its bundled files) to another resolves to something that actually exists
- Readability: any single sentence that joins more than one rule or condition and should be split into short clauses or numbered sub-parts
- Tracking fields: any tracking field (e.g. Version, Last updated, Revision History) in the document, or in the template used to create, update, or maintain it, that the planned changes don't update

For a document that instructs an agent or defines a process, also check:

- Reachability: any state without a defined next action, or any rule that requires returning to a completed step without being optional or having a stated exit condition
- Determinism: any rule requiring calculation, ordering, comparison, reconciliation, or exhaustive checking that is described in prose only, without an explicit procedure

For an update to an already-written document, scope the audit to the changed content and its interactions with the rest of the document, not the full previously-approved baseline.

Present the complete list before making any changes.

If no issues are found, say so and ask to move to Critical Check (5).


RESOLVE (4)

Review the Audit (3) findings per the review format defined above. Apply only approved (or user-updated) changes.

Before declaring a batch complete, cross-check coverage against the original source list (e.g., the input change list or Gather-phase item list). Do not rely on sequential item-by-item narration alone, since an item can be silently skipped mid-sequence without the omission being obvious from the running commentary.

1. List every item identifier from the source list.
2. Record each item's outcome: Approved, Rejected, Updated, or Backlog.
3. Confirm the count of outcomes equals the count of source items, and name any item with no outcome.

Items closed via Backlog count as accounted-for in this cross-check, not as omissions.

Cannot advance to Critical Check (5) while backlog items from this stage remain open — see Backlog.


CRITICAL CHECK (5)

Re-run Audit (3) for critical or serious issues only (an issue that would make the document wrong, contradictory, or unusable if written now, not a wording or style preference).

For an update to an existing file, also apply all approved changes to a working copy outside the output location, and treat any of these as a critical issue:

- a BEFORE text not found exactly once
- a diff against the original showing anything other than the approved changes
- a change in encoding or line endings

If any critical issues are found, go to Resolve (4). If none, confirm with the user before moving to Write (6).


WRITE (6)

Show a summary diff of what will change in the file (or the full draft, for a new document). This requires its own confirmation, separate from the stage-transition confirmation already given to enter this stage — only after the user confirms the write itself, write the file to the document type's defined output location and present it.


PROCESS IMPROVEMENT (7)

Runs only after Write (6) completes. Review this session for gaps in each of the following that applies:

- Skills: every skill used in this session, including the document-type skill, this workflow skill, and any other skill loaded and applied
- Templates: any template used to create, update, or maintain the document
- Project instructions: the project's own instructions document, if one governs this session (identify it from the project itself, not by searching a repository for one)

Do not report cosmetic or low-impact items (e.g. wording tweaks, one-off preferences).

Tag each finding (MAJOR if both apply):

- MINOR: caught late by the workflow (e.g. at Critical Check (5) instead of Audit (3))
- MAJOR: found only by user probing (would have gone undetected), has recurring impact, or caused real ambiguity or friction

Then:

- Propose changes only for MAJOR findings, grouped by file name (omit a group with no findings), each stating the specific change and what and why.
- Only edit a file if the user approves.
- If no MAJOR findings, say so, with a one-line mention of any MINOR items noted, rather than skipping the step silently.
- Skills and templates are meant to stay universal across all documents of their type; don't propose a change that would make one more restrictive or narrow it to this session's specific case.


The workflow is complete after Process Improvement (7).