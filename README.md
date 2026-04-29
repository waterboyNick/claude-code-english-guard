# Claude Code English Communication Guard

A focused Claude Code skill for native Chinese speakers who work with Claude Code in English.

It distinguishes between grammar/expression errors and semantic/task ambiguity.

- If the English is grammatically wrong but the intent is clear, Claude should first show a short correction, then continue.
- If the English is semantically ambiguous or risky, Claude should clarify before acting.

## Skill Name

`english-communication-guard`

## What It Catches

- grammar and expression errors in English or mixed English
- Chinese-to-English phrasing that changes the technical meaning
- ambiguous coding verbs such as `clean`, `cover`, `reset`, `open`, `close`, `sync`, and `replace`
- vague scope such as "all", "everything", "make it simple", or "fix all warnings"
- safety-sensitive ambiguity around git, file deletion, database, deployment, auth, secrets, and config
- tone that may sound unintentionally harsh in collaboration

## What It Does Not Do

- It does not rewrite every prompt.
- It does not log your inputs by default.
- It does not turn coding work into an English lesson.
- It does not list every tiny error unless you ask for a full correction.

## Recommended Always-On Instruction

For best results, add this to your `CLAUDE.md` or Claude Code user instructions:

```md
When I write in English or mixed English, classify my wording as grammar/expression error, semantic/task ambiguity, both, or clean.

If it is a grammar/expression error and my intent is clear, first show a short "Grammar check" with the corrected wording and one brief reason, then continue with my actual request.

If it is semantic/task ambiguity that may affect implementation, scope, safety, git operations, file changes, database, deployment, or technical interpretation, ask one clarification question before acting. If it is high-risk or destructive, do not act until clarified.

If both grammar error and semantic ambiguity exist, prioritize safety and task correctness; clarify the ambiguous action first, and include corrected wording only if it helps.
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
fuck!why you not use this skill last time when i told you the same prompt
```

Claude should correct the English first, then answer:

```text
Grammar check:
- "why you not use" -> "why didn't you use" (past-tense questions need "didn't + base verb")
- "i" -> "I" (the pronoun is capitalized)
- "told you the same prompt" -> "gave you the same prompt" (you give/send/use a prompt)

You're right about the underlying issue...
```

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
