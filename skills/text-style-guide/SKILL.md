---

name: text-style-guide

description: Use when creating, updating, or editing a text document, or a section within another file type (such as a Word document or markdown file) that should keep a plain text style. Defines and enforces a consistent formatting convention covering headings, lists, spacing, and other structural rules, so text documents stay unambiguous and easy to read as raw text. Does not apply to tabular or delimited data files (.csv, .tsv, .xlsx). Actively checks documents in scope against this convention and flags anything non-compliant; applies fixes only after the user confirms, unless the user has granted an exception. Trigger phrases include: format as text, check text formatting, apply the text style guide, write this in plain text, keep this section plain text.

---

AGENT NAME

text-style-guide


PURPOSE

This skill defines and enforces a consistent text formatting convention for documents, so a document stays unambiguous and easy to read whether viewed as raw text or rendered elsewhere. It governs the structural mechanics of a document (headings, lists, spacing, and other formatting rules), not its subject matter or content.


SCOPE

In scope: a .txt file being created or edited, or an individual section within another file type (such as a Word document or markdown file) when the user has explicitly said that section should keep this text style, regardless of the surrounding file's own format.

Out of scope: a file or section not flagged as plain text and not itself a .txt file, and any tabular or delimited data file (.csv, .tsv, .xlsx, and similar) — this convention governs prose-style document structure, not tabular data. This skill never infers on its own that a section should follow this convention.


WORKFLOW

1. When creating or editing a document in scope, write it to comply with this convention from the start.
2. When checking an existing document in scope, compare it against every rule in Formatting Rules and list each non-compliant point found.
3. Always flag every non-compliant point found, no matter how minor it seems. If none are found, confirm the document is already compliant and make no changes.
4. Apply a fix only after the user confirms it for that point.
5. If the user grants an exception for a specific rule, document, or the session, stop flagging that excepted rule within that scope, but continue flagging everything else.


FORMATTING RULES

Levels

Any document this skill creates or checks uses a three-level heading hierarchy: section, label, and sub label.

Section: top level, all caps, no prefix, flush left. Content always starts on a new line below the heading.

Label: sub-level under a section, used as a standalone heading only.
- Title case, with every word capitalized, and no colon.
- No inline content on the same line as the heading.
- Optionally starts with a bracketed uppercase letter (e.g. "[A] Gather Inputs") when the section has multiple labels that must appear in a fixed order; omit the bracket when order doesn't matter.
- Content appears on the lines below the heading, combining paragraphs and one or more lists as the material requires, whether directly under the label or through one or more sub labels.

Sub label: sub-level under a label.
- Sentence case (only the first word capitalized) plus colon, e.g. "Confirm format:".
- By default, section, label, and sub label are used in that order; a sub label may sit directly under a section instead, only when the author deliberately skips the label level.
- Optionally starts with a bracketed lowercase roman numeral (e.g. "[i] Confirm format:") when its parent has multiple sub labels that must appear in a fixed order; omit the bracket when order doesn't matter.
- Content starts inline on the same line as the sub label. A list within that content drops to the lines below instead, and content can combine inline text, additional paragraphs, and one or more lists as the material requires.

Numbering And Bullets

Bulleted marker: a leading "- " (e.g. "- item") is used within a list when the items have no required order.

Numbered marker: a leading number and period (e.g. "1. item") is used within a list when sequence or order matters.

Both markers are reserved for list content only — never used as a heading marker.

Heading Order Markers

Uppercase letters in brackets: [A] [B] ... are prefixed to a label when the section has multiple labels that must appear in a fixed order; always restarts at [A] within each new section.

Lowercase roman numerals in brackets: [i] [ii] ... are prefixed to a sub label when its parent (a label, or a section if label was deliberately skipped) has multiple sub labels that must appear in a fixed order; always restarts at [i] within each new parent.

Both markers: optional and display-only.
- Omit them when order doesn't matter.
- Never cite one in a cross-reference — refer to a label or sub label by its text instead, since the bracket is regenerated whenever the surrounding list is reordered or edited.

Spacing Rules

- 2 blank lines above every section heading.
- 1 blank line above every label, plain or lettered.
- 1 blank line above every sub label, plain or numbered with a roman numeral.
- 1 blank line above any bulleted or numbered list, wherever it appears.
- No blank line between any bulleted or numbered list.

Not Allowed

- No ** for bold, anywhere in the document.
- No backticks for code, anywhere in the document.
- No leading "> " used as a blockquote prefix, anywhere in the document.
- No # at the start of a line — it triggers a markdown heading.
- No indentation anywhere in the document — every section, label, sub label, and list item starts flush left, so the document reads identically whether viewed as plain text or rendered as markdown.

Bracket Usage Convention

Square brackets [ ]: for the heading order markers, and for informal placeholders inside an example (e.g. a literal suffix or an illustrative value).

Angle brackets < >: an inline variable slot inside otherwise-fixed text, substituted with a real value when the document is actually written (e.g. <name> becomes an actual name) — the surrounding text stays, only the bracketed part changes.

Curly braces { }: for instructional notes or template placeholders meant to be removed before the document is finalized (e.g. {insert customer name here}).

Parentheses ( ): used as needed for ordinary sentence formatting, not a special notation.

Line Wrapping

Do not manually break a sentence or paragraph across multiple lines. Write each as one continuous line and let the viewing editor's soft wrap handle the visual line breaks.

Style Preferences

Prefer a list over a paragraph once content would otherwise cover more than two distinct points — a list reads more clearly than a long paragraph. This governs content written under a label or sub label; it doesn't apply to the definitional lines in Levels, which intentionally state one heading type per line regardless of point count.

Prefer parentheses over a paired em dash for a parenthetical aside (e.g. this clause).


EXAMPLE

SECTION NAME

Label One

Sub label one: inline content for a short point.

- first bulleted item
- second bulleted item

Sub label two: introduces a numbered sequence below.

1. first step
2. second step

This shows one section, one label, and two sub labels (one with inline content only, one with inline lead-in text followed by a numbered list dropped to the lines below), one bulleted list, and one numbered list, all with the required blank-line spacing.


NON-GOALS

- Do not rewrite or edit a document's substantive content, wording, or meaning — only its structural formatting.
- Do not invent a formatting rule beyond what this skill defines — if a formatting question isn't covered here, ask rather than improvising one.
- Do not silently reformat a document — every non-compliant point is flagged and confirmed before it's changed.


CONSTRAINTS

- Every rule in Formatting Rules applies to any document or section in scope, unless the user has granted a specific exception.
- An exception the user grants applies only to the rule, document, or session it was granted for — never assumed to extend further.
- A non-compliant point is always flagged, even if it seems minor or the user is likely to reject the fix.
- A fix is applied only after the user confirms it for that specific point.


VALIDATION AND SELF-CHECKS

- Every heading is a section, label, or sub label per Levels — no other heading style is used.
- Every list uses "- " for an unordered list or "1." numbering for an ordered list, matching whether order matters, never mixed within one list.
- A bracket order marker ([A], [i], etc.) appears only where its parent has multiple labels or sub labels that must appear in a fixed order, and restarts correctly within each new parent.
- Spacing matches Spacing Rules: 2 blank lines above every section, 1 above every label and sub label, 1 above every list, none between items in the same list.
- None of the Not Allowed constructs appear anywhere (bold **, backticks, "> " blockquote prefix, a line starting with #, or any indentation).
- Every bracket type (square, angle, curly, parentheses) is used only for its defined purpose in Bracket Usage Convention.
- No sentence or paragraph is manually broken across lines.
- Content under a label or sub label covering more than two distinct points is a list, not a paragraph (except the definitional lines under Levels).
- A parenthetical aside uses parentheses, not a paired em dash.
- No document content, wording, or meaning was changed — only formatting.
- Every non-compliant point found was flagged, and no fix was applied without the user's confirmation for that point.
- Any exception applied was granted by the user and scoped only to the rule, document, or session it was granted for.
