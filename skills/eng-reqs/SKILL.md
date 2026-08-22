---
name: eng-reqs
description: Create, write, or update engineering requirements documents (also called needs documents or requirement specifications) covering hardware, software, firmware, electrical, mechanical, or test fixture requirements. Use this skill whenever the user says "write requirements," "create requirements," "update requirements," "create a needs document," "create a requirement specification," or asks for a requirements doc / RD / spec / requirements Word document for a product, subsystem, or test fixture — even if they don't use those exact words. Defines the sections, requirements table columns, and quality criteria for a requirements document — output as tab-separated (.tsv) by default, or as a Word (.docx) table if the user asks for a Word document.
---

# Engineering Requirements Document

## Output format

Default output is a single tab-separated values (.tsv) file — see "TSV formatting rules" below. If the user asks for a Word document instead, produce the same sections and the same Requirements/Revision History tables as real Word tables (not markdown-in-docx); consult a docx-creation skill if one is available for the mechanics of building the file. The section list, column list, and all content rules below apply the same way regardless of output format — only the file mechanics differ.

## What this skill produces

Narrative sections are written as labeled sections (TSV: one section-label row per section, content in the row(s) below it; docx: a heading per section). The Requirements section is always a proper table (TSV table or Word table, per Output format above). At minimum, sections appear in this order:

1. **Metadata** — Document Title (format: `<Type> Requirements Specification`, e.g. "Hardware Requirements Specification," "Product Requirements Specification," "Application Requirements Specification" — `<Type>` reflects the document's domain), Version, Timestamp (format: `MMM DD, YYYY. HH:MM`), Author, Status. Ask the user if they want additional metadata fields (e.g. Approver, Project/Program, Related Documents) before finalizing.
2. **Purpose** — why this document/requirement set exists. Stays solution-free, consistent with this skill's Need/Requirement/Design Specification distinction (see Definitions): describe the underlying need driving the document, not the specific implementation (e.g., avoid naming the technology stack, specific UI mechanisms, or other design-specification-level detail).
3. **Scope** — what is and is not covered. Also states plainly that any requirement flagged `OBSOLETE` is retained for traceability only and must not be treated as an active requirement or implemented against — see Definitions for what `OBSOLETE` means. Like Purpose, stays solution-free — describe what's covered in terms of the underlying need, not implementation detail.
4. **Definitions** — glossary of terms/acronyms used in the document. Must include an entry for the `OBSOLETE` flag itself: it means the requirement is retired and retained for traceability only, and that a retired ID is never reused, even if the requirement is later reinstated. Must also include an entry for `SHALL`: the modal verb marking a mandatory, testable, binding obligation — the reason it's written in ALL CAPS in the Requirement text under the Defined-term capitalization rule. Should also note that `Given`, `When`, and `Then` are Gherkin-style structural markers (precondition / trigger / consequent marker) rather than glossary terms with domain-specific meaning — they organize the sentence but aren't "defined" the way SHALL and other glossary terms are, which is why they appear as plain text rather than ALL CAPS. A field named in a requirement but absent from Definitions carries its ordinary meaning — Definitions holds only terms carrying meaning specific to this document. Should also draw the three-way distinction so writers and reviewers apply it consistently: **Need** (what a stakeholder wants, solution-independent, informal — not itself a requirement), **Requirement** (a quantified, verifiable, solution-domain statement — design *input*, what this doc captures), **Design Specification** (states *how* the need/requirement is met — design *output*, explicitly out of scope for this document).
5. **Tags** — defines the category values used in the Requirements table's Tags column (default set: Functional, Performance, User Interface, Environmental — extend or adjust per the user's domain, e.g. add Safety, Manufacturability, Reliability for hardware-heavy docs).
6. **Requirements** — a table with columns, in this order: ID, Name, Requirement, Rationale, Verification Method, Importance, Tags, Notes. See "Writing each column" below for how to fill each one. Confirm with the user if they want different or additional columns (e.g. Priority, Owner, Traceability ID).
7. **Revision History** — a table with columns, in this order: Version, Date, Author, Change Type, Requirement ID, Reason for Change. One row per changed requirement — multiple rows share the same Version/Date/Author when several requirements change together in one revision. Change Type is `ADD`, `REMOVE`, or `UPDATE` (`REMOVE` always means the requirement was flagged `OBSOLETE`, never deleted). Reason for Change explains why that specific requirement was added, retired, or updated. If a dedicated revision-history skill is not already loaded, ask the user if they'd like to add one before finalizing this table. If such a skill is loaded, it governs the table's column set, date format, grouping, and retention rules described above — this skill's own change-type tags (`ADD`, `REMOVE`, `UPDATE`) remain the document's authoritative tags regardless.

The user may ask for additional sections (Assumptions, Constraints, References, etc.) — include these as requested, but the seven above are the floor for any requirements doc this skill produces.

For TSV output: keep the whole document plain text / tab-separated — no embedded tables outside the TSV structure itself.

## Writing each column

- **ID** — `REQ-XXX`, sequential (REQ-001, REQ-002, ...). Once assigned, an ID is permanent — never reused, even for a requirement later marked `OBSOLETE` and even if that same need is reinstated later (give it a new ID instead).
- **Name** — a short label for the requirement (a few words, e.g. "Power ON"), distinct from the full Requirement text — used for quick scanning/reference.
- **Requirement** — one requirement per row, singular and testable. Core pattern: `<subject: system/element/part> SHALL <verb> <object> <key attribute>`. Prepend `Given <precondition>` and/or `When <trigger>` only when a real precondition or trigger event exists — don't force them onto static constraints, environmental/boundary conditions, or design constraints that have no natural trigger. Where a Given or When clause is present, mark the obligation with `Then` (e.g. `Given <precondition>, Then <subject> SHALL ...`); where neither is present, omit `Then` and open directly with the subject — a `Then` with nothing before it is a consequent with no antecedent. If a sentence contains "and," check whether it's actually two requirements that should be split into two rows.
- **Defined-term capitalization** — any term defined in the Definitions section is written in ALL CAPS every time it appears in the Requirement text (e.g. if "PAYLOAD" is defined, the requirement uses "PAYLOAD," not "payload") — signals the word carries the document's specific defined meaning rather than its everyday sense. Two exceptions: a term whose written form already contains fixed capitals from outside this document (e.g. mixed-case acronyms like CoE, R&D, P/N) keeps that form rather than being forced to ALL CAPS; and a term that is a literal value matched against an external source (e.g. Go, No-Go validated against a lookup sheet) keeps its exact matched casing. Capitalizing either would make the document disagree with the system it specifies.
- **Rationale** — states only the need driving the requirement — nothing else. No background context, speculation, theoretical justification, or links/references. Not a restatement of the requirement. Never contains SHALL — SHALL marks a binding obligation and belongs only in the Requirement column.
- **Verification Method** — how it will be confirmed: Test, Analysis, Inspection, Simulation, or Demonstration. Tie the method to the requirement's nature: functional/behavioral requirements need an accurate behavior description and are typically verified by Test or Demonstration; performance requirements need unambiguous quantities with tolerances/limits and are typically verified by Test or Analysis.
- **Importance** — one of `High`, `Medium`, `Low`:
  - **High** — if missing, an essential value is missing; the product would not be usable or acquirable, or it could lead to a bad decision. High-importance requirements are used to define CTQ (Critical to Quality).
  - **Medium** — if missing, the essential value can still be provided, though its absence is annoying.
  - **Low** — everything not High or Medium.
- **Tags** — pulled from the categories defined in the Tags section (e.g. Functional, Performance, User Interface, Environmental). A requirement can carry more than one tag if it genuinely spans categories — separate multiple tags with a comma inside the cell.
- **Notes** — short, current-state facts. Empty preferred over populated. Only add a Note when high-impact. For retired requirements, records the version the requirement was obsoleted in (see Obsolete handling below). Never contains SHALL — SHALL marks a binding obligation and belongs only in the Requirement column.

## Words to flag during Audit

When auditing a requirement (see "Audit criteria for this document type" below), check it against these categories. For each hit, the suggested fix is to replace with a specific, measurable term — or, for the last two, to move/split the content:

- **Vague/undefinable** — user-friendly, versatile, flexible, efficient, high performance, modern, as possible, improved
- **Wishful thinking** — 100% reliable, never fail, handle all unexpected failures, please all users, upgradable to all future situations
- **Speculation** — usually, generally, often, normally, typically
- **Possibility language** — may, might, should, ought, could, perhaps, probably
- **Let-out clauses** (outside a proper Given/When conditional) — if, but, except, unless, although
- **Non-atomic conjunctions** — and, or, with, also — signal the row should likely be split into separate requirements
- **Plan/schedule leakage** — dates, project phases, milestones, development activities — these belong in a project plan, not a requirement; flag and suggest removing
- **Solution leakage** — naming specific technology, materials, components, or implementation details — that's a design specification (HOW), not a requirement (WHAT); flag and suggest moving to a design doc or generalizing to the underlying need

## TSV formatting rules (TSV output only)

- Requirements and Revision History header rows follow the column order given in "What this skill produces" above, tab-separated — don't restate that order elsewhere in the doc or let it drift.
- Use actual tab characters as delimiters, not spaces or multiple tabs. Keep cell content free of embedded tabs/newlines so the file stays parseable (e.g. in Excel or a script) — use semicolons or line-internal punctuation instead.
- Narrative sections (Metadata, Purpose, Scope, Definitions, Tags) go above the Requirements table, each under its own section label row.

## Obsolete handling (all formats)

**Obsolete requirements are never deleted from the table.** A requirement is retired by replacing its Requirement cell with `OBSOLETE` (the full cell content, not a prefix on the original text) and clearing the Name, Rationale, Verification Method, Importance, and Tags cells for that row. Notes records only the version the requirement was obsoleted in — the reason for the change belongs in that version's Revision History entry (Reason for Change), not in Notes. The row keeps three filled cells: ID, Requirement (`OBSOLETE`), and Notes (version). That ID is retired permanently — if the same need comes back later, give it a new ID and reference the old one, rather than reusing or un-flagging the retired one.

## Audit criteria for this document type

When auditing this document, check each requirement for:
- **Necessary** — could it be removed while the goal is still met? Is there no real stakeholder behind it? Can it never be confirmed? Any of these → flag as not necessary.
- **Solution-free** — states WHAT is required, not HOW it's met (see "Words to flag" — solution leakage)
- **Atomic** — a single statement (see "Words to flag" — non-atomic conjunctions)
- **Given/When/Then required when applicable** — a requirement has a genuine precondition or trigger event but the Requirement text doesn't include the corresponding Given or When clause. Flag and add the missing clause. The exception for static constraints, environmental/boundary conditions, and design constraints with no natural trigger still applies — don't flag those.
- **Dangling Then** — a Requirement's text opens with Then but includes no preceding Given or When clause. Then is the consequent of a conditional; without an antecedent it's grammatically incomplete. Flag and remove Then, opening the sentence with the subject instead.
- **Correct** — stated without unexplained technical jargon or acronyms not covered in Definitions
- **Complete** — no missing information (units, conditions, thresholds)
- **Ambiguity** — any requirement or statement readable more than one way (see "Words to flag" — vague/undefinable, possibility language, let-out clauses, speculation)
- **Feasible** — achievable within the stated project/technical constraints
- **Verifiable** — confirmable by Test, Analysis, Inspection, Simulation, or Demonstration
- **Untagged/mismatched** — a requirement's Tags don't match a defined entry in the Tags section
- **Importance missing or unjustified** — no Importance value set, or set to High without a Rationale that supports essential/CTQ-level impact if missing
- **Duplication** — the same requirement, constraint, or definition appearing in more than one place; identify which location should be the single source of truth and suggest removing/deferring elsewhere
- **Cross-reference** — a Requirement ID is cited only in the Revision History table's Requirement ID column and in Notes — never anywhere else in the document. A citation elsewhere creates silent drift: if the cited requirement is later obsoleted, renumbered, or rewritten, the citing text goes stale with nothing to flag it. Flag any Requirement ID found outside those two locations and recommend restating the cited rule in plain terms instead.
- **Defined-term capitalization** — a term defined in Definitions is used in Requirement text without ALL CAPS, or an ALL CAPS word appears in Requirement text that isn't a defined term or a structural keyword (`SHALL`) — flag either direction (see "Writing each column" — Defined-term capitalization). A term covered by one of the two exceptions above (fixed external capitals, or a string-matched literal value) is not flagged for missing ALL CAPS.
- **SHALL outside Requirement** — the word SHALL appears anywhere other than the Requirement column (e.g. Rationale, Notes, Name, Verification Method, Tags). SHALL marks a binding, testable obligation and is reserved for the Requirement column only — flag any other occurrence and recommend removing or rephrasing without it.
- **Notes-only obligations** — a rule that determines behavior appears only in a Notes cell. Notes carry no obligation, so a build following it is doing so unprompted. Flag and recommend either moving it into Requirement text or deleting it as commentary.
- **Rationale scope** — a Rationale cell contains anything beyond the need driving the requirement: background context, speculation, theoretical justification, or a link/reference. Flag and recommend trimming to the need only.
- **Derived figures** — a Rationale or Notes field states a specific count, dollar amount, or percentage derived from a particular data sample (e.g., "affects 61 rows," "$676k excluded," "84 ideas"). These numbers describe one snapshot of one dataset, not a lasting property of the requirement, and go stale as soon as the underlying data changes. Flag any such figure and recommend describing the pattern or category instead (e.g., "a purchase line whose rows are all excluded removes that line entirely from spend totals" rather than citing the specific dollar figure one sample workbook produced).
- **Errors** — inconsistent, contradicts another requirement, or missing Verification Method

## Write-time rules

Output location: `/mnt/user-data/outputs/`, presented with `present_files` — same location regardless of whether the output is .tsv or .docx. Name the output file `req-<type/name>-v<number>-<yymmdd>-<checksum>` — checksum is a 32-bit CRC (CRC-32) of the file's content, written as 8 hex digits, recomputed each time the file is written; this applies whether the file is TSV or docx. This filename-style value is not repeated as a field inside the document.

As part of writing: update the Version and Timestamp fields in Metadata (Timestamp in `MMM DD, YYYY. HH:MM`), recompute the checksum in the output file name, and append one Revision History row per changed requirement (same Version/Date/Author across all rows from this write): Change Type (`ADD`, `REMOVE`, `UPDATE`), Requirement ID, and Reason for Change specific to that requirement.

No cell in an existing requirement's row is modified for cosmetic cleanup, rewording, or semantic polish. A cell is only changed to fix an actual defect (it is wrong, ambiguous, non-testable, or otherwise breaks one of this skill's rules) or as part of retiring the requirement (`OBSOLETE`). Any other suggested improvement is noted, not applied to the existing row.
