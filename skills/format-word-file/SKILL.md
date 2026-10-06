---
name: format-word-file
description: Applies a generic Word document formatting standard (page setup, typography, spacing, tables, headers/footers) whenever a Word document (.docx or .dotx) is being created, edited, or checked for formatting compliance. Trigger for "format this Word doc," "apply the style guide to this document," "check this document's formatting," "clean up the formatting," "make this look professional," or any mention of margins, fonts, heading styles, or table style in a Word document, even without those exact words. Governs visual and structural formatting only, not the document's content, wording, or section outline, and not a company-specific branded template (a dedicated brand-template skill, if loaded, takes precedence over this one). Distinct from a document-type skill such as a requirements-document skill, which defines what content and structure a specific kind of document must contain; this skill only formats whatever content and structure already exists, regardless of document type.
---


PURPOSE

This skill defines and enforces a consistent Word document formatting standard, so a document looks and reads professionally and consistently regardless of who authored it. It governs the visual and structural mechanics of a document (page setup, typography, spacing, tables, headers/footers, and document-control/revision-tracking elements), not its subject matter, content, or document type.


SCOPE

In scope: a .docx or .dotx document being created, edited, or checked, when it should follow a generic Word formatting standard. Covers page setup, typography, spacing, tables, headers/footers, and document-control/revision-tracking elements, when present in the content.

Out of scope: a document already governed by its own brand-specific template or style skill (that skill's rules take precedence); any section the user has flagged as plain text (defers to a dedicated plain-text formatting skill if one is loaded, otherwise left unformatted and the user is asked). This skill never infers on its own that a document should follow this convention or that a section is plain text; both require an explicit request.


PRECONDITIONS

Before producing or modifying a Word document, confirm a Word (.docx) document-creation and editing capability is available in the current environment. If it is not, tell the user this skill cannot generate or edit a Word file here, and ask whether to wait until the capability is available or produce the content in another format instead (do not fabricate a workaround). If the capability becomes available later in the same session, resume rather than treating the earlier unavailability as permanent.


INPUTS

Required: content to format, either an existing .docx to reformat or check, or content already organized into sections that needs to be laid out as a Word document. All actual text, data, and metadata field values (author, title, date, version, and similar) come from the user or the source material; this skill applies formatting to whatever content already exists and never supplies or fills in content itself.

Optional: none beyond what's covered under Required. If the document has no discoverable title (see What This Skill Produces, Typography), ask the user for one rather than inventing it.

If a required input is missing or unclear: if given undifferentiated prose with no section breakdown at all, ask the user for a rough section breakdown rather than inventing one (deciding a document's outline or content is out of scope for this skill, see Non-Goals).


ERROR HANDLING

If editing an existing document where the same nominal element already carries inconsistent formatting in different places (two different colors used for what are otherwise both Heading 1 paragraphs, or inconsistent table border styles across similar tables), flag the inconsistency and ask the user which styling should become the standard, rather than silently picking one.

If the source .docx cannot be opened or parsed, tell the user rather than attempting to guess its structure or contents.


WORKFLOW

1. Confirm the Preconditions capability is available and the input satisfies Inputs (either an existing .docx to check or reformat, or content already organized into sections). If not, resolve per those sections rather than guessing.
2. Determine whether this is a new document or an edit or check of an existing one.
3. New document: apply this skill's formatting standard (Formatting Rules, What This Skill Produces) directly to the given content, since there is no existing formatting to reconcile.
4. Existing document: compare it against every rule in Formatting Rules and What This Skill Produces, and list every non-compliant point found, no matter how minor. If none are found, confirm the document is already compliant and make no changes.
5. Present the result to the user before touching the file: for a new document, a text-based structure preview (sections, heading levels, tables); for an existing document, the full list of non-compliant points from step 4.
6. Apply a change only after the user confirms it for that specific point. If the user grants an exception for a specific rule, document, or session, stop flagging that rule within that scope, but continue flagging everything else.
7. Run Validation And Self-Checks.
8. Write the file only after checks pass.


OUTPUT FORMAT

Output is a .docx file, produced using the platform's own Word document creation and editing capability (see Preconditions). When editing an existing document, save back to the same file (or a copy, if the user asks to preserve the original) rather than renaming it. When creating a new document, derive the filename from the document's title if the content states one, in simple lowercase-hyphenated form; otherwise ask the user for a filename. This skill does not append a version number, date, or checksum to the filename on its own; versioning is a content decision outside this skill's scope (see Non-Goals), not a formatting one.


WHAT THIS SKILL PRODUCES

Page setup: Letter size (8.5 x 11 in), portrait, 1 inch margins on all sides.

Typography: font family Aptos throughout, applied as follows.

- Document title: bold, 18pt, centered. Sourced from an existing Title-styled paragraph, a first-line heading used as the de facto title, the filename, or one the user has stated; if none of these give a title, ask the user for one rather than inventing it.
- Heading 1: bold, color 365F91, 16pt.
- Heading 2: bold, color 4F81BD, 12pt.
- Heading 3: bold, color 1F4D78, 10.5pt.
- Heading 4: bold, italic, color 4F81BD, 12pt (body size).
- Body text: 12pt, single line spacing, left-aligned.

Tables:

- Borders: single black line, 0.5pt, on all outer and inner edges.
- Header row, if the table has one: shaded light gray (D9D9D9), text bold, 10pt.
- Body cells: regular weight, 10pt.
- Column widths: sized to content in a newly created table. In an existing table, leave current column widths unchanged - apply only the border, shading, and font rules above.

Headers, footers, and page numbering:

- Header: the document title (same source and text as the Typography Title rule) is repeated in the header, left-aligned, 8pt, gray (999999), matching the footer text style. If the content specifies a status or classification banner (for example "Preliminary — For Engineering Review"), it appears on a line below the title (or alone, if there is no title), centered, bold, red (C00000), 9pt. The header has a thin gray (999999) bottom border.
- Footer: left-aligned block for any footer text the content specifies (for example author, version, date), 8pt, with a thin gray (999999) top border.
- Page numbering: "Page X of Y" is always included, right-aligned in the footer line, generated with Word's native page-number and total-pages fields, never typed as static text.

Section numbering: headings use Word's built-in automatic outline or multilevel list numbering. Each level's generated number is styled to match that heading level's color and bold weight (Heading 1 number: 365F91, bold; Heading 2 number: 4F81BD, bold; Heading 3 number: 1F4D78, bold; Heading 4 number: 4F81BD, bold), followed by a space then the heading text, with no dash or other separator character.

List style: no specific rule; use Word's native default bullet or numbered list formatting when a list appears.

Callout box, for highlighting a single key statement when the content calls for one: single-cell table, full text width, centered, light blue fill (EAF2F8), text bold, 10.5pt, no visible border.

Inline lead-in label, for a short label introducing a paragraph without promoting it to a full heading: bold, same size and color as body text, followed by a period, then the paragraph text continues inline (not on a new line).


EXAMPLE

A short document with a title, two Heading 1 sections (one plain prose, one containing a table), and a footer.

Title: "Quarterly Test Summary", bold, 18pt, centered, also repeated in the header, left-aligned, 8pt, gray.

Heading 1, automatically numbered ("1", the number bold and colored 365F91): "Purpose". Body paragraph below it in 12pt Aptos, single-spaced.

Heading 1, automatically numbered ("2", the number bold and colored 365F91): "Results". Below it, a table with a header row (Metric, Value) shaded D9D9D9, bold 10pt, and two body rows in regular 10pt, single black 0.5pt borders throughout.

Footer: left side blank, no footer text supplied. Right side "Page 1 of 1" via Word's native page-number fields, thin gray top border.


FORMATTING RULES

- Page: Letter, portrait, 1 inch margins.
- Font: Aptos throughout.
- Title: bold, 18pt, centered.
- Heading 1: bold, color 365F91, 16pt.
- Heading 2: bold, color 4F81BD, 12pt.
- Heading 3: bold, color 1F4D78, 10.5pt.
- Heading 4: bold, italic, color 4F81BD, 12pt.
- Body: 12pt, single-spaced, left-aligned.
- Section numbering: Word's automatic outline numbering, each level's number colored and bolded to match its heading level, no separator character.
- Lists: Word's native default style.
- Table borders: single black, 0.5pt, all outer and inner edges.
- Table header row: shaded D9D9D9, bold, 10pt.
- Table body cells: regular weight, 10pt.
- Header: document title left-aligned, 8pt, gray 999999; status or classification header banner, if present, on a line below, centered, bold, red C00000, 9pt; gray 999999 bottom border.
- Footer: left-side text 8pt; "Page X of Y" always right-aligned via native page fields; gray 999999 top border.
- Callout box: centered single-cell table, light blue fill EAF2F8, bold text, 10.5pt, no visible border.
- Inline lead-in label: bold, same size/color as body text, followed by a period, then inline paragraph text.


NON-GOALS

- Do not write, invent, or alter a document's content, wording, data, or metadata values (author, title, date, version, and similar): only its formatting.
- Do not decide a document's section outline or structure: that comes from the user or a document-type skill.
- Do not decide whether a Document Control, Revision History, or status-banner section exists: only how to style one if the content already includes it.
- Do not invent a formatting rule beyond what this skill defines: if a formatting question isn't covered here, ask rather than improvising one.
- Do not silently reformat a document: every non-compliant point is flagged and confirmed before it is changed, per Workflow.


CONSTRAINTS

- Every heading level used maps to one of the defined styles (Title, Heading 1 through 4, Body): never an ad hoc font, size, or color combination outside those definitions.
- This skill defines heading styles only through Heading 4: content needing deeper nesting is restructured to fit within Title plus Heading 1 through 4, rather than inventing a fifth heading style.
- Table borders, shading, and text weight match the defined table style exactly: no per-table variation without an explicit user request.
- Page numbering, when present, is always generated via Word's native page-number fields, never typed as static text.
- A non-compliant point found while checking an existing document is always flagged, even if it seems minor or the user is likely to reject the fix.
- A fix is applied only after the user confirms it for that specific point.
- An exception the user grants applies only to the rule, document, or session it was granted for: never assumed to extend further.


VALIDATION AND SELF-CHECKS

- No content, wording, data, or metadata value was written, invented, or altered: only formatting changed.
- No document outline or section structure was decided by this skill: matches what the user or content specified.
- Every Document Control, Revision History, or status-banner section styled was already present in the content: none was added or removed.
- Every formatting choice traces to a rule in Formatting Rules or What This Skill Produces: nothing uncovered was improvised, it was asked about instead.
- Every non-compliant point found during a check was flagged, and no fix was applied without the user's confirmation for that specific point.
- The document is not governed by a brand-specific template or style skill that should have taken precedence instead.
- No section flagged as plain text was reformatted using this skill's own rules.
- Every heading uses one of the defined styles (Title, Heading 1 through 4, Body): no ad hoc combination.
- No heading style beyond Heading 4 was invented; deeper nesting was restructured to fit within Title plus Heading 1 through 4.
- Table borders, shading, and text weight match the defined table style exactly.
- Page numbers, if present, use Word's native fields, not typed text.
- Any exception applied was actually granted by the user and scoped only to what it was granted for.
