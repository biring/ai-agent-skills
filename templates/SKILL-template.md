=== TEMPLATE NOTES =================================================

Delete this block when generating the actual SKILL.md.

CORE sections are required in every skill.

OPTIONAL sections are included only when the noted condition applies; delete the section otherwise.

LENGTH GUIDANCE:

Metadata (name + description in frontmatter) — always loaded into context; keep description to roughly 100-150 words, but prioritize concrete, specific trigger phrasing over hitting that number exactly.

SKILL.md body — loaded into context only when the skill triggers; keep under 500 lines as a soft target.

Bundled resources (flat files: ref-<name>.txt, asset-<name>.<ext>, script-<name>.<ext>) — loaded only as needed, no length limit. No scripts/, references/, or assets/ subfolders — the filename prefix carries the category.

If the body is approaching 500 lines: don't cut content or weaken instructions — move detail that's reference material (worked examples, large tables, domain-specific variants) into a ref-<name>.txt file and leave a pointer sentence in SKILL.md. Executable, deterministic, or repetitive logic belongs in a script-<name>.<ext> file, not prose.

REFERENCES-POINTER CONVENTION: when the body needs to point to a bundled file, name it inline in a sentence — "See ref-<name>.txt for ..." or "Use asset-<name>.<ext> as ...". Never write a subfolder-style path.

FORMAT: the generated SKILL.md body uses the plain-text convention, not markdown. SECTION NAME is all capitals, no colon, content starts on the next line, two blank lines before every section. "Label:" starts at column 0 with one blank line before it; its bullets use the ▪︎ character at column 0. "◇ Sub Label:" is indented 4 spaces with one blank line before it; prose content goes inline on the same line regardless of length, and only drops to indented lines below when the content is itself a bulleted or lettered list. Lettered steps (a. b. c.) only ever nest under a ◇ Sub Label. Frontmatter stays YAML. No #, ##, **, - bullets, backticks, [ ], or > anywhere in the body. Hex colors are written without a # prefix.

Any content enclosed in angle brackets is an authoring instruction or hint for the template author. Do not include it in the generated SKILL.md; remove it entirely.

=====================================================================


---

name: 

< 
- skill-name is inline with the lable.
- lowercase-hyphenated, 
- matches the skill's folder name exactly >

description: <what this skill does, AND when to trigger it. This is the only thing used to decide whether the skill loads. Push for concrete trigger phrases a user would actually type ("make a deck," "build slides," "check this for brand compliance"), not generic category words alone. State what the skill always does by default so there's no ambiguity about when it applies.>

---

AGENT NAME: <Skill Title> <CORE>

DESCRIPTION: <One or two sentences: what this skill produces and the boundary of what it covers.>

<-- INDEPENDENCE CONSTRAINT (hard requirement): This skill must never reference another skill in this workspace by name, path, or file link, and must not assume another specific skill is also loaded. It must trigger correctly and run correctly whether attached alone or alongside any combination of other skills. If this skill's output needs a document-formatting mechanic (e.g. producing a Word/PDF/Excel file), rely on the AI platform's own general-purpose built-in capability for that, not on another skill in this workspace by name. A compliant way to interoperate with an unnamed skill: state a generic condition ("if a dedicated <capability> skill is loaded, it governs <specific sub-structure>") and, if none is loaded, prompt the user to add one — never name the specific skill or assume it's present. -->


PURPOSE <CORE>

<One to three sentences: what this skill governs or produces, at a level that stays true regardless of any single use of it.>


SCOPE <CORE>

<When to use this skill — the situations, requests, or triggers that mean this skill applies. What it covers.>

Out Of Scope:

▪︎ <Something this skill deliberately does not do, even though it sounds related.>

▪︎ <Repeat for each explicit exclusion. If a boundary case exists — content adjacent to scope that could be mistaken for in-scope — state it here and say what governs it instead.>


INPUTS <CORE>

Required:
▪︎ <Each piece of information or material this skill cannot function without.>

Optional:
▪︎ <Each piece of information that improves the result but isn't required, and what the fallback is when it's absent.>

If a required input is missing or unclear:
▪︎ <What to do instead of guessing — ask, reject, or apply a stated default. Never fabricate a required input.>


PROCESS <CORE>

◇ Steps:
    a. <First step, in actual execution order.>
    b. <Next step.>
    <Continue lettering through the full build order this skill follows, ending with when the output file is actually written.>


OUTPUT FORMAT <CORE>

<Default output format/file type and location. Note any alternate format the user can request instead.>


WHAT THIS SKILL PRODUCES <CORE>

<Section-by-section (or field-by-field) breakdown of the deliverable's structure. Mark which sections are always required vs. optional/ask-the-user.>


EXAMPLE <OPTIONAL — include when the output has structural rules (grouping, merging, tagging, conditional formats, etc.) that are easier to convey with a worked example than prose alone.>

<One or more small worked examples showing the output structure in a representative scenario.>


FORMATTING RULES <OPTIONAL — include only if the output format has structural rules that would otherwise be ambiguous or inconsistently applied, e.g. delimiter/encoding rules for a TSV, table conventions for a docx, heading levels for markdown.>

<Concrete, mechanical rules — delimiters, encoding, column order, header requirements, etc.>


WRITE-TIME RULES <OPTIONAL — include only if this skill has side effects that happen specifically at write time beyond just saving the file: updating a version/metadata field, replacing vs. appending content, recomputing a checksum, etc.>

<What changes automatically each time the file is written.> If this skill maintains a document across repeated sessions, consider whether previously-written content should be protected from non-substantive edits (e.g. only touch existing entries to fix a genuine defect or retire them — not for cosmetic rewording). If the skill has a machine-generated filename convention (e.g. embedding version/date/checksum), keep it here only — don't also expose it as a human-readable field inside the document; that field should stay a plain descriptive title.


CONSTRAINTS <CORE>

▪︎ <Each hard "never do this" rule this skill's output must follow, stated as a standalone rule rather than buried in Process.>
▪︎ <Include a default-for-ambiguity rule: what to do when a choice doesn't clearly map to any rule in this skill — ask, or fall back to the simplest compliant option and flag the choice.>


QUALITY AND AUDIT CRITERIA <CORE>

<Checklist of what to check for when auditing a draft of this skill's output — ambiguity, errors, words/patterns to flag, anything specific to this document/output type. General process checks like duplication-detection and issue-by-issue review belong in a workflow skill, not here — this section only holds criteria specific to this skill's content.> When a rule in Constraints or What this skill produces states a requirement, add a paired audit criterion here that checks for violations of it — keeps the two from drifting apart as the skill is revised.

<!-- Add ref-<name>.txt, asset-<name>.<ext>, or script-<name>.<ext> flat files only if content here would push this file over the length guidance above, needs worked examples, or is executable logic better run than described. No subfolders. -->
