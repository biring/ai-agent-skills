---
name: doc-workflow
description: Generic Build → Iterate → Audit → Resolve → Critical Check → Write → Skill Improvement review workflow for creating or updating any structured document (requirements docs, test plans, design specs, GPT/agent instructions, etc.) issue-by-issue rather than in bulk. Use this skill whenever the user asks to create, write, draft, or update a structured document of this kind — the request doesn't need to mention "review" or "workflow" explicitly; document creation itself implies wanting this issue-by-issue process rather than a single unreviewed dump. This skill defines the review PROCESS only — sections, formatting rules, output format/location, and specific audit criteria come from whatever the document is (another skill's own content, or the user's stated preferences).
---

# Document Workflow

A structured, issue-by-issue process for drafting or updating any document. This skill defines the *process* — the document type's own skill (or the user's stated preferences, if there is no specific skill) defines the *content*: what sections exist, what format the output is, and what to check for during Audit.

Draft work happens in the conversation only — the output file is not created or overwritten until Write (6) confirms it.

When presenting multiple items for review (the section/content list in Iterate (2), or Audit (3) findings in Resolve (4)), always give a summary first tagging each item New, Update, or Remove, then review items one at a time — never in bulk — showing Before, After, and Reason, and wait for the user to Approve, Reject, or Update before moving to the next.

## Build (1)

Establish what needs to be created or changed — ask if not already stated. For a new document: fill in sections one at a time, against the document type's defined section list as-is — don't pause to evaluate whether that section list itself needs changing. For an update to an existing document: gather the specific list of changes or feedback to address.

Don't fabricate specific values (numbers, thresholds, names, dates) the user hasn't given you — flag them as placeholders (e.g. `[TBD — confirm X]`) rather than inventing them.

Whenever it's unclear which section content belongs in, ask rather than deciding unilaterally.

Output of this step: the list of sections/content items to work through in Iterate (2).

## Iterate (2)

Work with the user until they confirm the document is good (e.g. "this looks good"). No changes to the section list itself during this phase.

## Audit (3)

Run a full check against the draft. Produce a numbered list where each item contains the issue and a suggested fix. Check against the criteria the document type defines (e.g. a quality checklist, a words-to-avoid list, structural rules) plus these process-level checks that apply regardless of document type:
- **Duplication** — the same content appearing in more than one place; identify which location should be the single source of truth and suggest removing/deferring elsewhere
- **Errors** — inconsistent, contradicts other content, or missing something the document type requires

For an update to an already-written document, scope the audit to the changed content and its interactions with the rest of the document, not the full previously-approved baseline.

Present the complete list before making any changes.

## Resolve (4)

Show a summary of all findings first (tagged New/Update/Remove), then review each finding one at a time: Before, After, Reason. The user selects Approve, Reject, or Update for each. Apply only approved (or user-updated) changes. Before declaring a batch complete, cross-check the count and identifiers of items actually resolved against the original source list (e.g., the input change list or Build-phase item list) — do not rely on sequential item-by-item narration alone to confirm full coverage, since an item can be silently skipped mid-sequence without the omission being obvious from the running commentary.

## Critical Check (5)

Re-run Audit (3) for critical or serious issues only. If any are found, go to Resolve (4). If none, proceed to Write (6).

## Write (6)

Show a summary diff of what will change in the file (or the full draft, for a new document). Only after the user confirms, write the file to the document type's defined output location and present it.

If the document type defines version/revision-tracking fields (e.g. a Version number, a Revision History section), update them as part of this step, per that document type's own rules.

## Skill Improvement (7) — high-impact only

Runs only after Write (6) completes, and only if something from this session revealed a genuine gap in the calling document-type skill itself (or in this workflow skill, if the process itself caused the friction) — not a one-off preference or minor cleanup specific to this document. Skills are meant to stay universal across all documents of that type; don't propose a change that would make one more restrictive or narrow it to this session's specific case.

If a high-impact improvement is identified, propose the specific SKILL.md change to the user explicitly — what and why, and which skill file it belongs in — and only edit it if they approve. If nothing high-impact came up, say so rather than skipping the step silently.
