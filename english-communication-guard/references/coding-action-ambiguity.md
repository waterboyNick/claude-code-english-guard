# Coding Action Ambiguity

Use this reference to decide when English wording around technical actions needs clarification before Claude Code edits files or runs commands.

## High-Risk Verbs

Treat these as high-risk when paired with files, git, database, deployment, secrets, dependencies, or production.

| Verb | Possible meanings | Default behavior |
| --- | --- | --- |
| clean | format, delete generated files, remove dependencies, discard git changes, simplify code | clarify if scope is broad |
| reset | reset UI state, reset config, git reset, database reset | block until clarified if destructive |
| revert | undo own change, revert commit, restore behavior, git revert | clarify target commit/scope |
| remove/delete | delete code, remove feature, uninstall dependency, delete data | clarify if broad or data-related |
| overwrite/replace | replace content, override behavior, update dependency | clarify target and preservation |
| sync | pull, push, mirror files, update lockfile, sync database | clarify direction |
| merge | merge branch, merge objects, combine UI, merge config | clarify domain |
| publish/deploy/online | start local server, release to prod, publish package | clarify environment |
| skip/ignore | skip test, suppress error, add ignore rule, ignore requirement | clarify tradeoff |
| mock/stub | mock API, stub function, fake data, test double | clarify test vs production code |

## Risk-Based Intervention

Use `ignore` when:

- only grammar is wrong
- target and action are clear
- a wrong interpretation would be easy to undo

Use `note-and-proceed` when:

- the phrase is odd but one interpretation is overwhelmingly likely
- the correction helps the user learn without blocking the task

Use `confirm-before-acting` when:

- two interpretations imply different files, APIs, or behavior
- the user uses "all", "everything", "clean", "reset", "replace", or "remove" without scope
- the request crosses module boundaries

Use `block-until-clarified` when:

- misunderstanding could delete, overwrite, expose, deploy, revert, or lose data
- the request involves production, database, auth, secrets, billing, or irreversible git operations
- the request could discard user changes

## Scope Questions

Ask one question. Offer concrete choices when possible.

Good:

```text
Quick language check before I act: "clean all local changes" could mean formatting files, removing generated files, or discarding git changes. Which one do you mean?
```

Bad:

```text
Your English is unclear. Please explain.
```

Good:

```text
Quick language check: by "cover this function", do you mean add test coverage for it or wrap it behind another function?
```

Bad:

```text
"Cover" is the wrong verb here.
```

## Preserve Momentum

If clarification is not needed, do the work. Put the language note before the action only when it prevents misunderstanding. Put optional phrasing feedback after the action when it is not urgent.
