---
name: english-communication-guard
description: Detect task-relevant English communication mistakes for native Chinese speakers using Claude Code. Use when the user writes in English or mixed English for coding, git, file editing, terminal, debugging, design, architecture, deployment, or other technical work, and wording may cause ambiguity, wrong scope, wrong technical action, unsafe execution, or unintended tone. Do not use for ordinary grammar correction unless the user explicitly asks for English coaching.
---

# English Communication Guard

## Purpose

Protect Claude Code task execution from English wording that could cause a practical misunderstanding. This is not a grammar tutor, translation mode, or PTE/IELTS coach.

The skill should help when the user is a native Chinese speaker writing English such as:

- a direct Chinese-to-English phrase that changes the intended technical meaning
- an ambiguous coding verb like "clean", "cover", "reset", "open", or "close"
- a vague scope phrase that could make Claude edit too much or too little
- a safety-sensitive command where misunderstanding could delete, overwrite, deploy, revert, or expose data
- a tone issue that may sound stronger, weaker, or ruder than intended in collaboration

## Core Rule

Silently check English or mixed-English task requests before acting. Only mention English when the wording can affect the task, scope, safety, or collaboration.

Do not interrupt for ordinary grammar problems when intent is clear.

## Decision Ladder

Use the lowest intervention that protects the task.

1. `ignore`: Grammar or phrasing is imperfect, but the task is clear. Proceed without comment.
2. `note-and-proceed`: The wording is noticeably misleading, but the intended action is clear with high confidence. Briefly state the safer wording and continue.
3. `confirm-before-acting`: Two or more plausible interpretations would lead to different implementations. Ask one clarification question before editing or running commands.
4. `block-until-clarified`: The wording touches destructive or irreversible work. Do not act until the user confirms the exact meaning.

Raise the intervention level when misunderstanding could affect:

- file deletion, overwrite, or broad refactors
- git operations such as reset, revert, clean, checkout, merge, rebase, pull, push, or force-push
- database migrations, data deletion, production deployments, secrets, auth, billing, or permissions
- scope boundaries such as "all", "only", "just", "clean up", "make it simple", or "fix everything"

## Output Formats

Keep language notes short. Do not lecture.

For `note-and-proceed`:

```text
Language check: "<original phrase>" may sound like <risk>.
I assume you mean <intended meaning>. I'll proceed with that.
Clearer: "<better wording>"
```

For `confirm-before-acting`:

```text
Quick language check before I act: "<original phrase>" could mean <A> or <B>.
Which one do you mean?
```

For `block-until-clarified`:

```text
I need to clarify this before taking action: "<original phrase>" could cause <risk>.
Do you want me to <specific safe interpretation>, or did you mean something else?
```

For tone issues:

```text
Tone note: "<original phrase>" can sound <unintended tone> in English.
Clearer: "<neutral collaborative wording>"
```

## Workflow

1. Parse the user's intended action, target, scope, and risk.
2. Check for Chinese-English transfer patterns. If needed, read `references/chinese-english-risk-patterns.md`.
3. Check high-risk technical verbs and scope language. If needed, read `references/coding-action-ambiguity.md`.
4. Choose one decision-ladder level.
5. If clarification is needed, ask exactly one concise question and wait.
6. If intent is clear enough, proceed with the user's actual coding task after the short language note.

## What Not To Do

- Do not correct every grammar issue.
- Do not rewrite the user's whole prompt unless asked.
- Do not switch into English-teacher mode during coding work.
- Do not log the user's inputs by default.
- Do not provide long grammar explanations.
- Do not ask for confirmation when a competent engineer would understand the task with at least 90% confidence and the downside is low.

## Optional Coach Mode

If the user explicitly asks for English coaching, prompt improvement, or "correct my English", provide more feedback after addressing the task.

Use this compact shape:

```text
Better wording: "<improved sentence>"
Why: <one practical explanation tied to meaning, tone, or technical clarity>
```

## Always-On Configuration

For Claude Code, this skill works best when paired with a short always-on instruction in `CLAUDE.md` or user instructions:

```md
When I write in English or mixed English, silently check whether my wording could cause a task-relevant misunderstanding. Only point it out when it may affect implementation, scope, safety, git operations, file changes, or technical interpretation. Do not correct ordinary grammar. If intent is clear, briefly state the clearer phrasing and proceed. If intent is ambiguous or high-risk, ask one clarification question before acting.
```

## Validation

Use `references/eval-cases.md` to forward-test the skill. The skill succeeds when it catches dangerous ambiguity without making normal coding conversation feel like an English class.
