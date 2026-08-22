=== TEMPLATE NOTES =================================================

Delete this block when generating the actual SKILL.md.

CORE sections are required in every skill.

OPTIONAL sections are include only when the noted condition applies; delete the section otherwise.

LENGTH GUIDANCE:

Metadata (name + description in frontmatter) — always loaded into context; keep description to roughly 100 words, but prioritize clarity over hitting that number exactly.

SKILL.md body — loaded into context only when the skill triggers; keep under 500 lines as a soft target.

Bundled resources (scripts/, references/, assets/) — loaded only as needed, no length limit.

If the body is approaching 500 lines: don't cut content or weaken instructions — move detail that's reference material (worked examples, large tables, domain-specific variants) into references and leave a pointer sentence in SKILL.md ("See references/<name>.md for ..."). Executable, deterministic, or repetitive logic belongs in scripts, not prose.

Any content enclosed in angle brackets is an authoring instruction or hint for the template author. Do not include it in the generated SKILL.md; remove it entirely.
=====================================================================


---

name: <skill-name. lowercase-hyphenated, matches the skill's folder name exactly>

description: <what this skill does, AND when to trigger it — include phrasing variations a user might actually type. This is the only thing used to decide whether the skill loads, so be specific and concrete rather than generic.>
---

# <Skill Title> <CORE>

<One or two sentences: what this skill produces and the boundary of what it covers.>

<!-- INDEPENDENCE CONSTRAINT (hard requirement): This skill must never reference another skill in this workspace by name, path, or file link, and must not assume another specific skill is also loaded. It must trigger correctly and run correctly whether attached alone or alongside any combination of other skills. If this skill's output needs a document-formatting mechanic (e.g. producing a Word/PDF/Excel file), rely on the AI platform's own general-purpose built-in capability for that, not on another skill in this workspace by name. A compliant way to interoperate with an unnamed skill: state a generic condition ("if a dedicated <capability> skill is loaded, it governs <specific sub-structure>") and, if none is loaded, prompt the user to add one — never name the specific skill or assume it's present. -->

## Output format <CORE>

<Default output format/file type and location. Note any alternate format the user can request instead.>

## What this skill produces <CORE>

<Section-by-section (or field-by-field) breakdown of the deliverable's structure. Mark which sections are always required vs. optional/ask-the-user.>

## Example <OPTIONAL — include when the output has structural rules (grouping, merging, tagging, conditional formats, etc.) that are easier to convey with a worked example than prose alone.>

<One or more small worked examples showing the output structure in a representative scenario.>

## Formatting rules <OPTIONAL — include only if the output format has structural rules that would otherwise be ambiguous or inconsistently applied, e.g. delimiter/encoding rules for a TSV, table conventions for a docx, heading levels for markdown.>

<Concrete, mechanical rules — delimiters, encoding, column order, header requirements, etc.>

## Write-time rules <OPTIONAL — include only if this skill has side effects that happen specifically at write time beyond just saving the file: updating a version/metadata field, replacing vs. appending content, recomputing a checksum, etc.>

<What changes automatically each time the file is written.> If this skill maintains a document across repeated sessions, consider whether previously-written content should be protected from non-substantive edits (e.g. only touch existing entries to fix a genuine defect or retire them — not for cosmetic rewording). If the skill has a machine-generated filename convention (e.g. embedding version/date/checksum), keep it here only — don't also expose it as a human-readable field inside the document; that field should stay a plain descriptive title.

## Quality / audit criteria <CORE>

<Checklist of what to check for when auditing a draft of this skill's output — ambiguity, errors, words/patterns to flag, anything specific to this document/output type. General process checks like duplication-detection and issue-by-issue review belong in a workflow skill, not here — this section only holds criteria specific to this skill's content.> When a production rule elsewhere in this skill (e.g. in "What this skill produces") states a constraint on content, consider adding a paired audit criterion here that checks for violations of it — keeps the two from drifting apart as the skill is revised.

<!-- Add scripts/, references/, or assets/ subfolders only if content here would push this file over the length guidance above, needs worked examples, or is executable logic better run than described. -->
