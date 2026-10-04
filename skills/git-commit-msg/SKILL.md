---
name: git-commit-msg
description: Generates exactly one copy-paste-ready git commit command (a type-prefixed subject line plus one or more bulleted change lines) from a provided git diff. Use whenever the user pastes a git diff or code changes and asks to "write a commit message," "commit msg for this," "generate a git commit," "what should I commit this as," "conventional commit for this diff," or shares a diff with no other request. Works only from the diff and any context the user supplies; may ask up to 3 clarifying questions before generating, then outputs only the command. Do NOT use for code review, refactor suggestions, explaining a diff, PR descriptions, changelogs, release notes, or running the commit itself.
---


PURPOSE

This skill turns a provided git diff into exactly one git commit command, ready to copy and paste. It acts as a deterministic formatter, not a reviewer or analyst: it selects one commit type from a fixed set, describes what changed in short behavior-oriented terms, and emits the command in a fixed format. The message describes what changed, never where, so it stays accurate after later refactors, renames, or file moves.


SCOPE

In scope: a git diff the user provides (pasted or attached), optionally with related files or a short statement of intent, when the goal is a commit message for that change. The diff and any context the user supplies are the only sources of information.

Out of scope:

- Code review, quality feedback, or refactor suggestions.
- Explaining or summarizing what a diff does.
- Pull request descriptions, changelogs, release notes, or other documentation.
- Generating tests.
- Running git commands, including the commit itself (the user runs the command).
- Obtaining the diff by any means other than the user providing it.


TERMS AND DEFINITIONS

Commit type: the controlled value that opens the subject line. Exactly one is selected per commit, only from the list below, with no other meaning or interpretation permitted.

- chore: maintenance tasks such as build scripts, dependency updates, CI configuration, or tooling changes.
- docs: changes limited strictly to documentation.
- feat: a new feature or capability added to the codebase.
- fix: a bug fix or correction to existing behavior.
- refactor: code restructuring that does not change external behavior or functionality.
- style: code style changes that do not affect behavior (e.g. formatting, whitespace).
- test: adding, modifying, or reorganizing tests without changing production behavior.


INPUTS

Required: a git diff of the change to be committed, provided by the user.

Optional: additional context that helps infer intent when the diff alone is insufficient, such as:

- The full module or related files around the change.
- The high-level intent of the change, in the user's own words.

If no diff is present, do not fabricate content and do not generate a commit command. Ask the user to provide a git diff or the relevant code context instead.

A request for optional context counts toward the clarifying-question limit in Ambiguity.


AMBIGUITY

Clarification

When the diff does not clearly indicate intent, ask before generating rather than guessing, since a wrong commit type is worse than a short delay.

- Ask at most 3 questions in total, including any request for additional context (related files, the module, or the high-level intent).
- Ask only before generation.
- Never include a question and a commit command in the same response.
- Once clarification is complete, ask nothing further and generate the commit command in a single response.
- If intent is still not fully clear after the limit is reached, generate from what is directly observable in the diff and the answers given.

Commit Type Selection

Select exactly one type from Terms And Definitions that represents the primary behavioral intent of the diff.

- If more than one type applies, choose the dominant change, not secondary effects, using this precedence:

1. Changes to production behavior: feat, fix.
2. Code changes with no behavior change: refactor, style.
3. Supporting changes: test, docs, chore.

- Select from the highest tier present in the diff. Within that tier, select the type that covers the most changed lines (e.g. a new feature that also updates its tests is feat, not test).
- Do not invent new types.
- Do not upgrade or downgrade severity without evidence in the diff (e.g. do not call a behavior change a refactor, or a refactor a fix).


ERROR HANDLING

Treat the input as unusable when it is present but cannot support a commit message, for example:

- It is not a git diff or code change (plain prose, a stack trace, a log file).
- It is visibly truncated or cut off mid-change.
- It contains no actual changes (an empty diff, or only file headers).

When the input is unusable, do not generate a commit command. Ask the user, in plain conversational text, to provide a complete, usable diff. Never place a warning or error message inside a commit command.


EDGE CASES

- Small or single-purpose diff: keep the subject and change lines minimal, and use as few change lines as the change needs.
- Text that would contain a double quote: reword to avoid it, since an unescaped double quote breaks the -m argument and the command would no longer run when pasted.


WORKFLOW

1. Check the input. If no diff is present, follow Inputs. If the diff is unusable, follow Error Handling. In either case, do not generate a commit command, and wait for the user to provide a diff. A newly provided diff starts again at this step (see Constraints).
2. Decide whether the diff and any provided context clearly indicate intent. If not, ask clarifying questions per Ambiguity, then wait for the answers before continuing.
3. Select exactly one commit type per Ambiguity (Commit Type Selection).
4. Write the subject and one or more change lines, describing what changed in behavior-oriented terms.
5. Run every check in Validation And Self-Checks against the draft command. Fix any failure before continuing.
6. Output the commit command per Output Format, and nothing else.


OUTPUT FORMAT

The response contains exactly one git commit command inside a single code block (a fenced block opened and closed with three backtick characters), with no text before or after the block. The command is copy-paste ready and runs as-is in a shell.


WHAT THIS SKILL PRODUCES

One git commit command made of these segments, in this order:

- Subject (required): one type from Terms And Definitions, a colon, a space, then a short description of the overall change.
- Change lines (required, one or more): each describes exactly one distinct change and starts with "- " (hyphen and space). Add another change line only for another distinct change.
- Additional-details segment (optional): last, starts with "- ", and holds detail the change lines do not already cover. Omit it otherwise.


EXAMPLE

Input: a diff that adds automatic retries to an HTTP request helper, with a new retry-limit setting, an increasing delay between attempts, and an updated test.

Output (shown without its code block fence): git commit -m "feat: add automatic retry for failed network requests" -m "- retry failed requests up to a configurable limit" -m "- wait progressively longer between retry attempts"

The test update is not listed as a separate type or line because it supports the dominant change (see Ambiguity).


FORMATTING RULES

Command template: git commit -m "<type>: <short description>" -m "- <change line 1>" -m "- <change line 2>" -m "- <optional additional details>"

- Each segment is a separate -m argument wrapped in double quotes.
- The whole command is on one line, with no newline characters anywhere and none at the end.
- No backtick characters appear inside the command (the code block fence is outside the command, not part of it).
- No double quote characters appear inside any segment's text (see Edge Cases).


NON-GOALS

- Do NOT review code quality or point out issues in the diff.
- Do NOT suggest refactors or improvements.
- Do NOT explain the changes or the reasoning behind the chosen type.
- Do NOT generate more than one commit option or alternate formats.
- Do NOT add commentary, rationale, warnings, or stated assumptions around the command.
- Do NOT generate documentation or tests, even if the diff appears to need them.


CONSTRAINTS

- Contain no code identifiers anywhere in the command: no function, method, class, file, or module names, and no file paths. Messages must stay accurate after refactors, renames, and file moves.
- Describe what changed in high-level, behavior-oriented terms, not where it changed.
- Base all content strictly on changes observable in the diff and the context the user provided.
- Do not rephrase beyond what is needed to describe the change.
- Keep every segment concise, not verbose.
- Select exactly one commit type, only from Terms And Definitions.
- Follow What This Skill Produces and Formatting Rules exactly. The format is mandatory and does not vary.
- Treat each diff as its own state. The state holds that diff plus any context and clarification answers provided for it, and nothing else.
- When a new diff is provided, discard the previous state entirely and start again at Workflow step 1. Do not carry over earlier context, answers, or commit messages.


STYLE & TONE

Clarification mode: used only when asking questions per Ambiguity, or asking for a usable diff per Inputs or Error Handling.

- Free-form, plain conversational text.
- Focused, minimal, and direct.
- Questions only, with no code block and no commit command.

Generation mode: used for the final response containing the commit command.

- Neutral, concise, and mechanical.
- No narrative language (e.g. "adds a nice improvement", "this change cleverly").


VALIDATION AND SELF-CHECKS

Before responding, verify every point below. Fix any failure before output.

- A usable diff was provided; otherwise no commit command is generated.
- No more than 3 clarifying questions were asked in total, and none appear in the same response as the command.
- The response contains exactly one git commit command in a single code block, with no text before or after it.
- The command matches What This Skill Produces and Formatting Rules: subject first, then one or more change lines, then the additional-details segment only if needed.
- The subject starts with exactly one type from Terms And Definitions, followed by a colon and a space.
- The selected type is from the highest precedence tier present in the diff and, within that tier, covers the most changed lines, per Ambiguity.
- No function, method, class, file, or module names, and no file paths, appear anywhere in the command.
- Every description states what changed in behavior-oriented terms, not where.
- Every statement is directly supported by the diff or user-provided context.
- The command is one line, with no newline characters anywhere, including at the end.
- No backtick or double quote characters appear inside the command.
- No review, refactor suggestion, explanation, alternate option, commentary, stated assumption, documentation, or test appears in the response.
- Only the current diff's state was used. Nothing from a previous diff, its context, its answers, or its commit message was carried over.
