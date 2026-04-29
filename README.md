# Claude Code English Communication Guard

A focused Claude Code skill for native Chinese speakers who work with Claude Code in English.

It does not act as a general grammar teacher. It watches for English wording that could make Claude Code misunderstand a technical task, edit the wrong scope, run the wrong command, or take a risky action.

## Skill Name

`english-communication-guard`

## What It Catches

- Chinese-to-English phrasing that changes the technical meaning
- ambiguous coding verbs such as `clean`, `cover`, `reset`, `open`, `close`, `sync`, and `replace`
- vague scope such as "all", "everything", "make it simple", or "fix all warnings"
- safety-sensitive ambiguity around git, file deletion, database, deployment, auth, secrets, and config
- tone that may sound unintentionally harsh in collaboration

## What It Does Not Do

- It does not correct every grammar issue.
- It does not rewrite every prompt.
- It does not log your inputs by default.
- It does not turn coding work into an English lesson.

## Recommended Always-On Instruction

For best results, add this to your `CLAUDE.md` or Claude Code user instructions:

```md
When I write in English or mixed English, silently check whether my wording could cause a task-relevant misunderstanding. Only point it out when it may affect implementation, scope, safety, git operations, file changes, or technical interpretation. Do not correct ordinary grammar. If intent is clear, briefly state the clearer phrasing and proceed. If intent is ambiguous or high-risk, ask one clarification question before acting.
```

## Installation

Copy the skill folder into your Claude Code skills directory:

```bash
cp -r english-communication-guard ~/.claude/skills/
```

Then restart Claude Code or reload skills.

## Example

User:

```text
clean all local changes
```

Claude should not act immediately:

```text
I need to clarify this before taking action: "clean all local changes" could mean formatting files, removing generated files, or discarding git changes.
Do you want me to format/remove generated files, or discard git changes?
```

User:

```text
wash the login bug
```

Claude can proceed if the context is clear:

```text
Language check: "wash the login bug" may sound like cleaning, not fixing.
I assume you mean fix the login bug. I'll proceed with that.
Clearer: "Fix the login bug."
```
