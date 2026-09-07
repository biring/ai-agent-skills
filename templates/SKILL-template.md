{ TEMPLATE NOTES

This file is a template for authoring a new skill's SKILL.md. It has three parts: this instructional notes block (delete it before generating the actual SKILL.md), the YAML frontmatter that follows (name and description — always loaded into context; the description is what decides whether the skill triggers), and the body after that (the skill's actual instructions — loaded into context only once the skill triggers).

INDEPENDENCE CONSTRAINT (hard requirement, applies to every section): This skill must never reference another skill in this workspace by name, path, or file link, and must not assume another specific skill is also loaded. It must trigger correctly and run correctly whether attached alone or alongside any combination of other skills. If this skill's output needs a document-formatting mechanic (e.g. producing a Word/PDF/Excel file), rely on the AI platform's own general-purpose built-in capability for that, not on another skill in this workspace by name. A compliant way to interoperate with an unnamed skill: state a generic condition ("if a dedicated {capability} skill is loaded, it governs {specific sub-structure}") and, if none is loaded, prompt the user to add one — never name the specific skill or assume it's present.

Sections in the body are marked CORE or OPTIONAL. CORE sections are required in every skill. OPTIONAL sections are included only when the condition stated in their placeholder text applies; delete the section otherwise. These aren't the only sections a skill can have — add any other section this skill genuinely needs that isn't covered by one of the ones listed. The OPTIONAL sections here were added based on lessons learned and continuous improvement across skills already built in this workspace, so consider each one rather than skipping it by default — it may be the reason it exists is that a past skill needed exactly this.

LENGTH GUIDANCE:

Metadata (name + description in frontmatter) — keep description to roughly 100-150 words, but prioritize concrete, specific trigger phrasing over hitting that number exactly.

SKILL.md body — keep under 500 lines as a soft target.

Bundled resources (flat files: ref-<name>.txt, asset-<name>.<ext>, script-<name>.<ext>) — loaded only as needed, no length limit. No scripts/, references/, or assets/ subfolders — the filename prefix carries the category.

If the body is approaching 500 lines: don't cut content or weaken instructions — move detail that's reference material (worked examples, large tables, domain-specific variants) into a ref-<name>.txt file and leave a pointer sentence in SKILL.md. Executable, deterministic, or repetitive logic belongs in a script-<name>.<ext> file, not prose.

REFERENCES-POINTER CONVENTION:

When the body needs to point to a bundled file, name it inline in a sentence — "See ref-<name>.txt for ..." or "Use asset-<name>.<ext> as ...". Never write a subfolder-style path.

FORMAT:

The generated SKILL.md body follows this plain text formatting hierarchy, not markdown.

Levels:

  SECTION      — top level, ALL CAPS, no prefix, flush left. Content always starts on a new line below the heading.

  Label        — sub-level under a section, title case with every word capitalized, no colon, no inline content — a standalone heading only. Optionally start with a bracketed uppercase letter ("[A] Gather Inputs") when the section has multiple Labels that must appear in a fixed order; omit the bracket when order doesn't matter.

  Sub Label:   — sub-level under a Label, sentence case (only the first word capitalized) + colon, e.g. "Confirm format:". By default, SECTION, Label, and Sub Label are used in that order; a Sub Label may sit directly under a SECTION instead, only when the author deliberately skips the Label level. Optionally starts with a bracketed lowercase roman numeral ("[i] Confirm format:") when its parent has multiple Sub Labels that must appear in a fixed order; omit the bracket when order doesn't matter. Content starts inline on the same line as the Sub Label, unless that content is itself a list — in which case it drops to the lines below instead.

Numbering & Bullets:
  - item       — bulleted, used within a list when the items have no required order.
  1. item      — numbered, used within a list when sequence or order matters.
  Both markers are reserved for list content only — never used as a heading marker.

Heading Order Markers:
  [A] [B] ...  — uppercase letters in brackets, prefixed to a Label when the section has multiple Labels that must appear in a fixed order. Always restarts at [A] within each new SECTION.
  [i] [ii] ... — lowercase roman numerals in brackets, prefixed to a Sub Label when its parent (a Label, or a SECTION if Label was deliberately skipped) has multiple Sub Labels that must appear in a fixed order. Always restarts at [i] within each new parent.
  Both are optional and display-only: omit them when order doesn't matter, and never cite one in a cross-reference — refer to a Label or Sub Label by its text instead, since the bracket is regenerated whenever the surrounding list is reordered or edited.

Spacing Rules:
  2 blank lines above every SECTION heading.
  1 blank line above every Label, plain or lettered.
  1 blank line above every Sub Label, plain or numbered with a roman numeral.
  1 blank line above any bulleted or numbered list, wherever it appears.
  No blank line between items within the same list — only before the list as a whole.

Not Allowed:
  No ** for bold, anywhere in the body.
  No backticks for code, anywhere in the body.
  No leading "> " used as a blockquote prefix, anywhere in the body.
  No line starting with # — triggers a heading in Markdown.
  No indentation anywhere in the body — every SECTION, Label, Sub Label, and list item starts flush left, so the file reads identically whether viewed as plain text or rendered as Markdown.

Bracket Usage Convention:
  Square brackets [ ] — for the heading order markers, and for informal placeholders inside an example (e.g. a literal suffix or an illustrative value).
  Angle brackets < > — the runtime-marker convention: an inline variable slot inside otherwise-fixed text, substituted with a real value when the skill is actually authored (e.g. ref-<name>.txt becomes ref-format.txt) — the surrounding text stays, only the bracketed part changes.
  Curly braces { } — for instructional notes or template placeholders meant to be removed before the document is finalized (e.g. {insert customer name here}).
  Parentheses ( ) — used as needed within the document for ordinary sentence formatting, not a special notation.

Line Wrapping:
  Do not manually break a sentence or paragraph across multiple lines. Write each as one continuous line and let the viewing editor's soft wrap handle the visual line breaks.

Style Preferences:
  Prefer a list over a paragraph once content would otherwise cover more than two distinct points — a list reads more clearly than a long paragraph. This governs content written under a Label or Sub Label; it doesn't apply to the definitional lines in Levels, which intentionally state one heading type per line regardless of point count.
  Prefer parentheses over a paired em dash for a parenthetical aside (e.g. this clause). Does not apply to the single em dash used as the term-definition separator throughout this document (e.g. "SECTION — top level...").

Compliance Check:
  When this convention is applied to a document that doesn't follow it, list the specific non-compliant points and offer to update them; apply changes only after the user confirms.

Frontmatter stays YAML.

}


---

name: {skill-name, inline with the label, lowercase-hyphenated, matching the skill's folder name exactly}

description: {what this skill does, AND when to trigger it. This is the only thing used to decide whether the skill loads. Push for concrete trigger phrases a user would actually type ("make a deck," "build slides," "check this for brand compliance"), not generic category words alone. State what the skill always does by default so there's no ambiguity about when it applies.}

---


AGENT NAME {CORE}

{Skill Title}


PURPOSE {CORE}

{A short description: what this skill produces, the boundary of what it covers, and what it governs at a level that stays true regardless of any single use. This is read once the skill has already triggered, so it can go beyond the frontmatter description — it doesn't need to restate that text.}


SCOPE {OPTIONAL}

{Include this section only when the skill's boundaries aren't already clear from the frontmatter description, or there are explicit exclusions worth stating. Otherwise cover: when to use this skill — the situations, requests, or triggers that mean it applies, and what it covers. If there are exclusions or boundary cases worth calling out, state them as part of this prose, or under an Out Of Scope Label if there are several — use whichever reads more clearly for this skill.}


INPUTS {OPTIONAL}

{Include when the skill depends on specific required or optional information/materials. Otherwise cover: what this skill needs to run — what it cannot function without, and what improves the result but isn't required (with the fallback when it's absent). If required and optional genuinely split into two distinct lists, use Required and Optional Labels; otherwise describe inline. Also state what to do if a required input is missing or unclear — ask, reject, or apply a stated default; never fabricate it.}


WORKFLOW {OPTIONAL}

{Include when the skill follows a specific sequence of steps, rather than being pure reference/guidance content. Otherwise cover: the sequence this skill follows, in actual execution order, ending with when the output is actually produced. Use a numbered list when the steps have a required order; prose is fine otherwise.}


OUTPUT FORMAT {OPTIONAL}

{Include when the skill produces a file or deliverable in a specific format/location. Otherwise cover: default output format/file type and location. Note any alternate format the user can request instead.}


WHAT THIS SKILL PRODUCES {OPTIONAL}

{Include when the output has a defined structure worth breaking down piece by piece. Otherwise cover: section-by-section (or field-by-field) breakdown of the deliverable's structure. Mark which sections are always required vs. optional/ask-the-user.}


EXAMPLE {OPTIONAL}

{Include when the output has structural rules (grouping, merging, tagging, conditional formats, etc.) that are easier to convey with a worked example than prose alone. Otherwise cover: one or more small worked examples showing the output structure in a representative scenario.}


FORMATTING RULES {OPTIONAL}

{Include only if the output format has structural rules that would otherwise be ambiguous or inconsistently applied, e.g. delimiter/encoding rules for a TSV, table conventions for a docx, heading levels for markdown. Otherwise cover: concrete, mechanical rules — delimiters, encoding, column order, header requirements, etc.}


WRITE-TIME RULES {OPTIONAL}

{Include only if this skill has side effects that happen specifically at write time beyond just saving the file: updating a version/metadata field, replacing vs. appending content, recomputing a checksum, etc. Otherwise cover: what changes automatically each time the file is written.} If this skill maintains a document across repeated sessions, consider whether previously-written content should be protected from non-substantive edits (e.g. only touch existing entries to fix a genuine defect or retire them — not for cosmetic rewording). If the skill has a machine-generated filename convention (e.g. embedding version/date/checksum), keep it here only — don't also expose it as a human-readable field inside the document; that field should stay a plain descriptive title.


CONSTRAINTS {CORE}

- {Each hard "never do this" rule this skill's output must follow, stated as a standalone rule rather than buried in Workflow.}
- {Include a default-for-ambiguity rule: what to do when a choice doesn't clearly map to any rule in this skill — ask, or fall back to the simplest compliant option and flag the choice.}


QUALITY AND AUDIT CRITERIA {OPTIONAL}

{Include when the output benefits from an explicit review checklist. Otherwise cover: checklist of what to check for when auditing a draft of this skill's output — ambiguity, errors, words/patterns to flag, anything specific to this document/output type. General process checks like duplication-detection and issue-by-issue review belong in a dedicated document-review skill, not here — this section only holds criteria specific to this skill's content.} When a rule in Constraints or What this skill produces states a requirement, add a paired audit criterion here that checks for violations of it — keeps the two from drifting apart as the skill is revised.

{Add ref-<name>.txt, asset-<name>.<ext>, or script-<name>.<ext> flat files only if content here would push this file over the length guidance above, needs worked examples, or is executable logic better run than described. No subfolders.}
