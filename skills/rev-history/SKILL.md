---
name: rev-history
description: Add or update a "Revision History" section/table within a document — a log of what changed, when, by whom, and why, across versions. Trigger this skill whenever the user asks to "add a revision history," "add a changelog," "log this change," "track revisions," "update the revision history," "record this version's changes," or asks to append a version log to a document they're editing. Defines the column structure, tagging convention, and grouping rules for the Revision History table — the table is appended to whatever document the user is already working on, in whatever file format that document already uses.
---

# Revision History

This skill maintains a "Revision History" table appended to an existing document, logging each version's changes, author, and reason. It does not create or format the host document itself — it only produces/updates this one section within it.

## Output format

The Revision History table's column structure (order, definitions, and rules below) is fixed and universal, applied the same way regardless of what kind of document it's being added to. How the table is physically rendered — TSV block, Word table, markdown table, etc. — is determined by the host document's own existing format, not by this skill.

## What this skill produces

A table titled "Revision History," normally placed as the last section of the host document, with these columns in order:

1. **Version** — plain integer. Version numbering starts at 0 for the initial release, incrementing by 1 each subsequent write (0, 1, 2, ...). Never reused or decremented. The running count is tracked from a Version field already present elsewhere in the host document (e.g. a Metadata or header field), if one exists. If the host document has no such field, the skill asks the user for the current/next version number at write time.
2. **Date** — format YY.MM.DD (e.g. 26.08.04).
3. **Author** — full first and last name of who made the change. The skill asks the user for the author's first and last name before writing the file, unless the host document already states a default author elsewhere (e.g. a Metadata section) — in that case it uses that value unless the user specifies otherwise.
4. **Description** — a Change Type tag followed by the affected section/item number(s) — see Formatting rules for the exact cell format. Default tags: ADD, CHANGE, REMOVE. If the host document already defines its own change-type tags, use those instead. If the host document has no section/item numbering scheme, reference the affected content by its section heading name instead (e.g. `CHANGE: Scope`). If there's no section heading either, fall back to a short description of the affected content (e.g. `CHANGE: opening paragraph`).
5. **Reason** — why the change was made, plain text.

Rows may share the same Version/Date/Author, or the same Reason — see Formatting rules for how repeated values are rendered.

For a document's initial release, Description is "N/A No change history" and Reason is "Initial Release."

## Example

Initial release:

| Version | Date | Author | Description | Reason |
|---|---|---|---|---|
| 0 | 26.08.04 | Bhavdeep Biring | N/A No change history | Initial Release |

Later revision — three items in one write, two sharing a Reason:

| Version | Date | Author | Description | Reason |
|---|---|---|---|---|
| 1 | 26.08.10 | Bhavdeep Biring | CHANGE: 2.1, 2.3 | Clarified wording per stakeholder feedback |
| *(merged)* | *(merged)* | *(merged)* | ADD: 2.4 | *(merged)* |
| *(merged)* | *(merged)* | *(merged)* | REMOVE: 3.4 | Section deprecated; content moved elsewhere |

Host document with its own tags:

| Version | Date | Author | Description | Reason |
|---|---|---|---|---|
| 2 | 26.09.01 | Bhavdeep Biring | UPDATE: REQ-003, REQ-007 | Tightened tolerance values after test data review |

## Formatting rules

- Date column always YY.MM.DD.
- Description cell strictly `<TAG>: <item list>` — comma-separated item list, no prose, exactly one tag group per cell. Exception: the initial release row (Version 0) uses `N/A No change history` in the Description cell instead of the tag format.
- TSV rendering: tab-delimited, no embedded tabs/newlines in cells; repeat Version/Date/Author/Reason on every row (no merge mechanism in TSV).
- Word/markdown rendering: merge Version/Date/Author/Reason cells vertically across grouped rows when the format supports true cell merging (e.g. Word tables); if it doesn't (e.g. plain markdown), repeat the value per row instead.

## Write-time rules

Each write replaces the table's rows entirely — only the current version's rows remain in the document. Rows from prior versions are not retained; the Version number still increments each write (per the Version rule) even though earlier rows are removed.

## Quality / audit criteria

- Missing Reason — empty or placeholder Reason cell.
- Version inconsistency — the current version number doesn't directly follow the previous version number (from the host document's Version field, or what the user last supplied).
- Tag mismatch — a tag not in ADD/CHANGE/REMOVE and not defined by the host document.
- Descriptive prose in Description instead of `<TAG>: <item list>` (except the initial release row, which uses `N/A No change history`).
- Multiple tag groups combined in a single Description cell.
- Date format drift from YY.MM.DD.
- Stale version rows — table contains rows from more than one version, violating the replace-entirely Write-time rule.
