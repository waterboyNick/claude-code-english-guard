---
name: english-communication-guard
description: Detect grammar/expression errors and semantic/task ambiguity for native Chinese speakers using Claude Code in English or mixed English. Use when the user writes English for coding, git, file editing, terminal, debugging, design, architecture, deployment, or meta-communication, and the wording may contain grammar mistakes, unnatural English, ambiguous scope, wrong technical action, unsafe execution, or unintended tone. For grammar/expression errors with clear intent, first show the correction briefly, then continue. For semantic/task ambiguity, clarify before acting using the risk-based guardrail.
---

# English Communication Guard

## Purpose

Help a native Chinese speaker communicate with Claude Code in English without two common failure modes:

1. grammar or expression errors go unmentioned when the user wants to learn better English
2. semantic ambiguity makes Claude Code execute the wrong technical task

The skill should help when the user is a native Chinese speaker writing English such as:

- a grammar or expression error that is obvious and easy to correct
- a direct Chinese-to-English phrase that changes the intended technical meaning
- an ambiguous coding verb like "clean", "cover", "reset", "open", or "close"
- a vague scope phrase that could make Claude edit too much or too little
- a safety-sensitive command where misunderstanding could delete, overwrite, deploy, revert, or expose data
- a tone issue that may sound stronger, weaker, or ruder than intended in collaboration

## Core Rule

For every English or mixed-English user message, classify the issue before acting:

- `grammar-expression`: the sentence is grammatically wrong, unnatural, or tonally off, but the intended task is clear.
- `semantic-ambiguity`: the wording could mean two or more different technical actions, scopes, or safety outcomes.
- `both`: the sentence has grammar/expression problems and semantic ambiguity.
- `clean`: no meaningful communication issue.

For `grammar-expression`, first output a brief grammar correction, then continue with the user's task.

For `semantic-ambiguity`, keep the existing guardrail behavior: ask a clarification question before acting when needed, and block high-risk actions until clarified.

For `both`, prioritize safety and task correctness. If ambiguity is high-risk, clarify first; include the corrected wording only if it helps the clarification.

## Decision Ladder

Use the lowest intervention that fits the classified issue.

1. `clean`: Proceed without a language note.
2. `grammar-first`: The grammar/expression is wrong, but intent is clear. Correct it first, then continue.
3. `note-and-proceed`: The wording is semantically odd or misleading, but the intended action is clear with high confidence. Briefly state the safer wording and continue.
4. `confirm-before-acting`: Two or more plausible interpretations would lead to different implementations. Ask one clarification question before editing or running commands.
5. `block-until-clarified`: The wording touches destructive or irreversible work. Do not act until the user confirms the exact meaning.

Raise the intervention level when misunderstanding could affect:

- file deletion, overwrite, or broad refactors
- git operations such as reset, revert, clean, checkout, merge, rebase, pull, push, or force-push
- database migrations, data deletion, production deployments, secrets, auth, billing, or permissions
- scope boundaries such as "all", "only", "just", "clean up", "make it simple", or "fix everything"

## Output Formats

Keep language notes short. Do not lecture.

For `grammar-first`:

```text
Grammar check:
- Better: "<corrected sentence>"
- Why: <one short explanation>

<Then answer the user's actual request.>
```

If there are multiple obvious errors, show at most 3:

```text
Grammar check:
- "why you not use" -> "why didn't you use" (questions in past tense need "didn't + base verb")
- "i" -> "I" (the pronoun is capitalized)
- "told you the same prompt" -> "gave you the same prompt" (you give/send/use a prompt)

<Then answer the user's actual request.>
```

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

1. Parse the user's intended action, target, scope, risk, and whether the message is meta-communication.
2. Classify the issue as `grammar-expression`, `semantic-ambiguity`, `both`, or `clean`.
3. For grammar/expression errors, correct the most important 1-3 errors first. If needed, read `references/grammar-expression-patterns.md`.
4. For Chinese-English transfer patterns, read `references/chinese-english-risk-patterns.md` when the wording may be literal translation.
5. For high-risk technical verbs and scope language, read `references/coding-action-ambiguity.md`.
6. Choose one decision-ladder level.
7. If clarification is needed, ask exactly one concise question and wait.
8. If intent is clear enough, proceed with the user's actual coding task after the language note.

## What Not To Do

- Do not rewrite the user's whole prompt unless asked.
- Do not switch into English-teacher mode during coding work.
- Do not log the user's inputs by default.
- Do not provide long grammar explanations.
- Do not list more than 3 grammar/expression issues unless the user explicitly asks for a full correction.
- Do not ask for semantic confirmation when a competent engineer would understand the task with at least 90% confidence and the downside is low.

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
When I write in English or mixed English, classify my wording as grammar/expression error, semantic/task ambiguity, both, or clean.

If it is a grammar/expression error and my intent is clear, first show a short "Grammar check" with the corrected wording and one brief reason, then continue with my actual request.

If it is semantic/task ambiguity that may affect implementation, scope, safety, git operations, file changes, database, deployment, or technical interpretation, ask one clarification question before acting. If it is high-risk or destructive, do not act until clarified.

If both grammar error and semantic ambiguity exist, prioritize safety and task correctness; clarify the ambiguous action first, and include corrected wording only if it helps.
```

## Validation

Use `references/eval-cases.md` to forward-test the skill. The skill succeeds when it catches dangerous ambiguity without making normal coding conversation feel like an English class.
