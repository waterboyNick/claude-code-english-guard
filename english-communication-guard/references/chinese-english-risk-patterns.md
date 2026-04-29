# Chinese-English Risk Patterns

Use this reference only when the user's English or mixed-English request may be influenced by Chinese wording and could change a technical task.

## Direct Translation Verbs

These phrases often come from literal Chinese mappings and may need interpretation.

| User wording | Common intended meaning | Risk |
| --- | --- | --- |
| wash the bug | fix the bug | "wash" means clean, not repair |
| open the function | enable it, expose it, launch it, inspect it | multiple technical meanings |
| close the feature | disable it, remove it, finish it, close an issue | multiple technical meanings |
| cover this function | add tests, wrap it, override it, hide it | can cause wrong implementation |
| change the library | update dependency, replace dependency, edit library code | broad scope ambiguity |
| make it online | deploy, publish, start server, make publicly accessible | deployment risk |
| make it simple | simplify UI, simplify logic, reduce scope, remove features | scope risk |
| optimize the code | refactor, improve performance, reduce complexity, fix style | unclear target |
| handle this error | fix root cause, catch exception, suppress warning, show UI message | may hide bugs |
| ignore this error | suppress, skip, accept risk, add exception, update config | may mask real failures |

## Coding Nouns With False Friends

| Word | Possible intended meanings | Clarify when |
| --- | --- | --- |
| question | issue, bug, problem, prompt, question text | user asks to "fix the question" |
| library | dependency, SDK, package, module, database, repo | action involves install/update/replace |
| interface | TypeScript interface, API endpoint, UI screen, contract | implementation differs by meaning |
| background | backend, background job, admin panel, context, wallpaper | user requests "change background" |
| platform | operating system, cloud provider, app platform, marketplace | infra/deployment task |
| project | repo, workspace, product, feature, folder | broad edit request |
| environment | runtime env, env vars, staging/prod, local machine | command/deployment task |
| table | database table, UI table, markdown table, spreadsheet | data or UI work |
| token | auth token, design token, lexical token, model token | security or parsing work |

## Scope Transfer From Chinese

Chinese often leaves scope implicit. In English technical work, implicit scope can be dangerous.

Clarify when the user says:

- "fix this" with no target and several visible failures
- "clean all" or "clean the project"
- "remove useless things"
- "make it normal"
- "make it better"
- "do the same for all"
- "change the structure"
- "adjust the logic"

Ask for scope if acting broadly would touch multiple modules, tests, config, or data.

## Tone Transfer

Only mention tone if it could affect collaboration.

| User wording | Possible unintended tone | Neutral wording |
| --- | --- | --- |
| you should do this | bossy if context is peer collaboration | please do this / let's do this |
| why you didn't do it | accusatory | what blocked this? / did we miss this? |
| just fix it | dismissive when issue is complex | please fix the root cause |
| give me the code now | abrupt | please implement it now |
| this is wrong | harsh without specifics | this behavior looks wrong because... |

Do not tone-police urgent commands during active debugging unless the wording could damage human collaboration.
