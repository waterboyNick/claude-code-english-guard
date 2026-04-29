# Grammar And Expression Patterns

Use this reference when the user's English is understandable but grammatically wrong, unnatural, or tonally off.

Keep feedback short. Show the corrected sentence first, then one practical reason. Do not turn coding work into a grammar lesson.

## Common Grammar Corrections

| User wording | Better wording | Why |
| --- | --- | --- |
| why you not use | why didn't you use | English past-tense questions need `did/didn't + base verb` |
| why you don't use last time | why didn't you use last time | `last time` points to the past |
| i | I | The first-person pronoun is capitalized |
| help me to refactoring | help me refactor / help me with refactoring | After `help`, use the base verb; after `with`, use `-ing` |
| I want add test | I want to add a test | `want` takes `to + verb`; singular count nouns need an article |
| this api return 500 | this API returns 500 | Third-person singular present verbs take `-s` |
| can you explain me | can you explain this to me | `explain` takes the thing first, then `to + person` |
| told you the same prompt | gave you the same prompt | You usually give/send/use a prompt, not tell a prompt |
| discuss about | discuss / talk about | `discuss` is transitive and does not take `about` |
| depend on with | depend on | `depend` takes `on`, not `with` |

## Expression Corrections

| User wording | Better wording | Why |
| --- | --- | --- |
| make it online | deploy it / start the local server / make it public | The intended technical action is unclear |
| open the feature | enable the feature | Features are usually enabled/disabled |
| close the log | turn off the log / disable logging | Logging is usually enabled/disabled |
| wash the bug | fix the bug | Bugs are fixed, not washed |
| very emergency | urgent | `urgent` is the natural adjective |
| how to say | how should I say this? | Use a full question when asking for phrasing |

## Tone Corrections

Only mention tone when the message is meta-communication, collaboration, review feedback, or emotionally strong.

| User wording | Better wording | Why |
| --- | --- | --- |
| fuck! why you not use this skill | I'm frustrated. Why didn't you use this skill? | The original is aggressive and grammatically wrong |
| why you didn't do it | What blocked this? / Why wasn't this done? | The original can sound accusatory |
| you must give me code now | Please implement it now | More collaborative |
| this is wrong | This behavior looks wrong because... | More specific and easier to act on |

## Output Rule

For ordinary task requests, correct at most 1 issue unless the error is prominent.

For meta-communication or emotionally strong messages, correct up to 3 issues because the user is communicating about communication, not only asking for code.

Example:

```text
Grammar check:
- "why you not use" -> "why didn't you use" (past-tense questions need "didn't + base verb")
- "i" -> "I" (capitalize the pronoun)
- "told you the same prompt" -> "gave you the same prompt" (you give/send/use a prompt)

You're right about the underlying issue...
```
