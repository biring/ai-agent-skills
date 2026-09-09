{


OVERVIEW

This file is a template for authoring a new skill's SKILL.md. It has three parts, listed below.

- This instructional notes block (delete it before generating the actual SKILL.md).
- The YAML frontmatter that follows (name and description — always loaded into context; the description is what decides whether the skill triggers).
- The body after that (the skill's actual instructions — loaded into context only once the skill triggers).


INDEPENDENCE CONSTRAINT

This is a hard requirement that applies to every section.

- This skill must never reference another skill in this workspace by name, path, or file link, and must not assume another specific skill is also loaded.
- It must trigger correctly and run correctly whether attached alone or alongside any combination of other skills.
- If this skill's output needs a document-formatting mechanic (e.g. producing a Word/PDF/Excel file), rely on the AI platform's own general-purpose built-in capability for that, not on another skill in this workspace by name.
- A compliant way to interoperate with an unnamed skill: state a generic condition ("if a dedicated {capability} skill is loaded, it governs {specific sub-structure}") and, if none is loaded, prompt the user to add one — never name the specific skill or assume it's present.


CORE AND OPTIONAL SECTIONS

Sections in the body are marked CORE or OPTIONAL.

- CORE sections are required in every skill.
- OPTIONAL sections are included only when the condition stated in their placeholder text applies; delete the section otherwise.
- These aren't the only sections a skill can have — add any other section this skill genuinely needs that isn't covered by one of the ones listed.
- The OPTIONAL sections here were added based on lessons learned and continuous improvement across skills already built in this workspace, so consider each one rather than skipping it by default — it may be the reason it exists is that a past skill needed exactly this.
- Section order in the body does not affect how the skill runs. The order below is a suggested default flow for readability, not a requirement — reorder sections if a different sequence reads more clearly for a particular skill.
- Add a section only when its content genuinely earns a standalone heading. If a concept only needs a line or two, fold it into the most relevant existing section instead of creating a new one to hold it — a template that grows a section for every small idea gets harder to use, not more complete. Apply the same judgment in the other direction too: if an existing section is quietly covering two distinct concerns (e.g. an imperative rule mixed with a scope boundary), split it into two rather than leaving them combined. Neither merging nor splitting is a mechanical rule — decide case by case which reads more clearly and stays easiest to maintain.


LENGTH GUIDANCE

Metadata: (name + description in frontmatter) keep description to roughly 100-150 words, but prioritize concrete, specific trigger phrasing over hitting that number exactly.

Skill body: the SKILL.md body should stay under 500 lines as a soft target.

Bundled resources: flat files (ref-<name>.txt, asset-<name>.<ext>, script-<name>.<ext>) are loaded only as needed, with no length limit. No scripts/, references/, or assets/ subfolders — the filename prefix carries the category.

If approaching the limit: if the body is approaching 500 lines, don't cut content or weaken instructions — move detail that's reference material (worked examples, large tables, domain-specific variants) into a ref-<name>.txt file and leave a pointer sentence in SKILL.md. Executable, deterministic, or repetitive logic belongs in a script-<name>.<ext> file, not prose.


REFERENCES-POINTER CONVENTION

When the body needs to point to a bundled file, name it inline in a sentence — "See ref-<name>.txt for ..." or "Use asset-<name>.<ext> as ...". Never write a subfolder-style path.


FORMAT

The generated SKILL.md body follows the plain text formatting convention defined by the text-style-guide skill (headings, lists, spacing, and other structural rules) — not markdown.

If the text-style-guide skill is loaded in this workspace, use it to format the generated SKILL.md body. If it is not loaded, ask the user to add it rather than improvising a convention here.

This note is deleted along with the rest of this instructional-notes block before the actual SKILL.md is generated, so naming text-style-guide here does not violate the Independence Constraint above — the generated skill's own body still never names another skill.

}


---

name: {skill-name, inline with the label, lowercase-hyphenated, matching the skill's folder name exactly}

description: {what this skill does, AND when to trigger it. This is the only thing used to decide whether the skill loads. Push for concrete trigger phrases a user would actually type ("make a deck," "build slides," "check this for brand compliance"), not generic category words alone. State what the skill always does by default so there's no ambiguity about when it applies. If another skill in this workspace covers a similar-sounding request, state the distinction here (not only in SCOPE) — this field is what actually decides triggering; SCOPE only helps once the skill has already loaded.}

---


PURPOSE {CORE}

{A short description: what this skill produces, the boundary of what it covers, and what it governs at a level that stays true regardless of any single use. This is read once the skill has already triggered, so it can go beyond the frontmatter description — it doesn't need to restate that text.}


SCOPE {OPTIONAL}

{Include this section only when the skill's boundaries aren't already clear from the frontmatter description. Otherwise cover: when to use this skill — the situations, requests, or triggers that mean it applies — and, just as deliberately, when NOT to use it, especially requests that sound similar but should route to a different skill or to default behavior instead. State exclusions as part of this prose, or under an Out Of Scope Label if there are several. Scope covers whether this skill applies at all; task prohibitions once it does apply belong in Non-Goals instead.}


TERMS AND DEFINITIONS {OPTIONAL}

{Include when this skill uses terms whose specific meaning affects behavior and isn't obvious from plain English, or has a closed vocabulary (a fixed set of allowed values, e.g. status labels or classification categories) — state which values are allowed and when to select each. If a term is also used in a bundled ref-<name>.txt file, define it once here and have the ref file reference it rather than redefining it. List entries alphabetically, one per line, as "word or phrase: meaning".}


PRECONDITIONS {OPTIONAL}

{Include when this skill cannot function without a specific capability, tool, or bundled file being available (e.g. code execution, network/web access, a particular file-format reader, a bundled ref-<name>.txt or script-<name>.<ext> this skill depends on). State what to verify before doing any other work, and what to tell the user if a precondition isn't met. Capabilities can become available mid-session (the user connects a tool or grants access), so state whether to wait/retry once available rather than assuming a permanent failure. Distinct from the INDEPENDENCE CONSTRAINT: this covers general capabilities and this skill's own bundled files, never a dependency on another named skill in this workspace.}


INPUTS {OPTIONAL}

{Include when the skill depends on specific required or optional information/materials. Otherwise cover: what this skill needs to run — what it cannot function without, and what improves the result but isn't required (with the fallback when it's absent). If required and optional genuinely split into two distinct lists, use Required and Optional Labels; otherwise describe inline. Also state what to do if a required input is missing or unclear — ask, reject, or apply a stated default; never fabricate it. If the skill proceeds without an optional input that would have improved the result, disclose that via Judgment Disclosure rather than silently omitting it.}


AMBIGUITY {OPTIONAL}

{Include when this skill's task involves judgment calls about intent or scope, not just missing input (INPUTS already covers missing/unclear input). State the default assumption to make when intent or scope is ambiguous, and whether to proceed on that assumption or ask — pick one and say when the other applies. If classifying something into categories requires judgment, state what clearly falls on each side, then give the tie-break rule for what's left. If the default assumption applied should be disclosed to the user, note it via Judgment Disclosure rather than inventing a separate disclosure mechanism.}


ERROR HANDLING {OPTIONAL}

{Include when input can be present but bad — malformed, partial, truncated, or internally conflicting (e.g. two files that disagree, a table missing expected rows) — a different problem than missing/unclear input, which INPUTS already covers. State how to detect the bad state, and whether to proceed with a flagged caveat, ask the user to fix it, or refuse to generate output.}


EDGE CASES {OPTIONAL}

{Include when input or expected output can hit a rare or boundary scenario that is technically valid, not ambiguous (see Ambiguity) and not bad (see Error Handling) — e.g. an empty input, a single-item input, or a value exactly at a defined limit. State the defined behavior for each case identified.}


WORKFLOW {OPTIONAL}

{Include when the skill follows a specific sequence of steps, rather than being pure reference/guidance content. Otherwise cover: the sequence this skill follows, in actual execution order, ending with when the output is actually produced. If a step can't proceed until a condition is met (a prior step's approval, a check passing), state that condition explicitly rather than leaving it implied by step order. Covers only this skill's own execution sequence — not a specific in-output computation or reconciliation procedure (see Algorithm / Required Check for that). Use a numbered list when the steps have a required order; prose is fine otherwise.}


ALGORITHM / REQUIRED CHECK {OPTIONAL}

{Include when correct output requires calculation, ordering, comparison, reconciliation, a state transition, or an exhaustive check — not just a stated rule. A rule states WHAT must be true; this section states HOW to establish that it is true: the inputs, the step-by-step check or transformation sequence, the completion condition, the failure condition, and what triggers recomputation. Distinct from WORKFLOW, which covers this skill's own overall execution sequence, not a specific in-output computation or reconciliation procedure. Do not assume the underlying model will derive a correct procedure from a prose rule alone.}


STABLE-CONTENT CRITERIA {OPTIONAL}

{Include only if this skill updates or maintains existing content rather than generating fresh output each run. State the criteria under which existing content already complies and must NOT be rewritten or regenerated (a "stable" state), and the exact fixed phrase or action to take in that case instead of rewriting (e.g. "leave unchanged" or a specific note to output).}


OUTPUT FORMAT {OPTIONAL}

{Include when the skill produces a file or deliverable in a specific format/location. Otherwise cover: default output format/file type and location, and any alternate format the user can request instead. Covers only the container — what type of file and where it goes — not its content (see What This Skill Produces) or its mechanical encoding (see Formatting Rules).}


WHAT THIS SKILL PRODUCES {OPTIONAL}

{Include when the output has a defined structure worth breaking down piece by piece. Otherwise cover: section-by-section (or field-by-field) breakdown of the deliverable's content, marking which pieces are always required vs. optional/ask-the-user. Covers only what content goes in the output — not the file type/location (see Output Format) or how that content is mechanically encoded (see Formatting Rules).}


EXAMPLE {OPTIONAL}

{Include when the output has structural rules (grouping, merging, tagging, conditional formats, etc.) that are easier to convey with a worked example than prose alone. Otherwise cover: one or more small worked examples showing the output structure in a representative scenario.}


FORMATTING RULES {OPTIONAL}

{Include only if the output format has structural rules that would otherwise be ambiguous or inconsistently applied, e.g. delimiter/encoding rules for a TSV, table conventions for a docx, heading levels for markdown. Otherwise cover: concrete, mechanical encoding rules — delimiters, encoding, column order, header requirements — for content already defined in What This Skill Produces. Not the file type/location (see Output Format) or which content exists (see What This Skill Produces).}


WRITE-TIME RULES {OPTIONAL}

{Include only if this skill has side effects that happen specifically at write time beyond just saving the file: updating a version/metadata field, replacing vs. appending content, recomputing a checksum, etc. Otherwise cover: what changes automatically each time the file is written. If this skill updates existing content rather than generating fresh output, see Stable-Content Criteria for what must not be regenerated. If the skill has a machine-generated filename convention (e.g. embedding version/date/checksum), keep it here only — don't also expose it as a human-readable field inside the document; that field should stay a plain descriptive title. Covers only what changes at the moment of writing — not the file's type/location (see Output Format) or a name derived for something inside the output's content (see Naming Conventions).}


NAMING CONVENTIONS {OPTIONAL}

{Include only when the output requires a name deterministically derived from other data (e.g. a generated identifier, a derived label, a name built from an existing field) — not a fixed set of allowed values (see Terms and Definitions for that), and not the output file's own filename (see Write-Time Rules for that). Give the exact derivation rule plus one worked example.}


NON-GOALS {CORE}

{Adjacent or tempting tasks this skill must never perform, even if related or requested — its scope boundaries. Use "Do NOT ..." form.}


CONSTRAINTS {CORE}

{State each hard rule this skill's output/content must always satisfy, as a standalone rule rather than buried in Workflow.}


INVARIANTS {OPTIONAL}

{Include when this skill involves two related-but-different things that could be conflated, letting bad output pass silently (e.g. "entry fidelity does not imply temporal completeness"). State each as a standalone fact to keep straight, not as something to derive from the rules in Constraints — an invariant is a fact, not an instruction.}


RESPONSE FORMATTING {OPTIONAL}

{Include when this skill's chat responses need formatting rules beyond the platform's own default (see Style And Tone for voice, not mechanics). State whether to use markdown (headers, bold, tables, code blocks) or plain text, and any rule for lists, numbers, units, or dates. Distinct from Formatting Rules, which covers the mechanical structure of a produced file/deliverable, not the chat response itself.}


STYLE & TONE {OPTIONAL}

{Include when this skill's output (chat responses, or any report/deliverable) needs a defined voice beyond the platform's own default. Otherwise cover: whether output should be factual and direct with no hedging, concise, formal/technical or casual, whether to quantify with numbers/units/thresholds rather than vague descriptors, and whether findings or issues should be flagged explicitly rather than softened. State only where this skill's needs diverge from or add to the platform's own default style.}


VALIDATION AND SELF-CHECKS {CORE}

{A checklist to run against the draft immediately before finalizing output — the last gate before responding, not a separate review pass. Cover: content-specific checks (ambiguity, errors, words/patterns to flag, anything specific to this skill's output type), plus a check for every rule stated in Non-Goals or Constraints so violations aren't missed. General process checks like duplication-detection and issue-by-issue review belong in a dedicated document-review skill, not here — this section only holds criteria specific to this skill's own content. When a rule in Constraints or What This Skill Produces states a requirement, add a paired check here so the two don't drift apart as the skill is revised.}


JUDGMENT DISCLOSURE {OPTIONAL}

{Include when this skill makes any judgment the user can't see — a default applied under ambiguity (see Ambiguity), excluded content, or input that was missing, declined, or unusable. Define exactly one location in the output where such disclosures appear (e.g. a fixed closing line or section), so every other section that says "note this" or "disclose this" points to that same place rather than each inventing its own. Formatters and summarizers need this too — classifying and condensing both involve judgment.}
