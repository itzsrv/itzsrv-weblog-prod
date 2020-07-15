---
title: Git Commit Message Conventions
date: 2020-05-30
tags: ['note']
draft: false
---


Following a convention for writing Git commit messages makes it easy for fellow contributors to understand and navigating through git history becomes simple (for an example, ignoring all style changes).

Format of the commit message:

```
<type>[optional scope]: <subject>

[optional body]

[optional footer(s)]
```

For an example:

```git
feat: add page travel
^--^ ^--------------^
|    |
|    +-> subject in present tense.
|
+-------> Type: chore, docs, feat, fix, refactor, style, or test.
```

Allowed type values:
>
feat: (new feature for the user, not a new feature for build script)
>
fix: (bug fix for the user, not a fix to a build script)
>
docs: (changes to the documentation)
>
style: (formatting, missing semi colons, etc; no production code change)
>
refactor: (refactoring production code, eg. renaming a variable)
>
test: (adding missing tests, refactoring tests; no production code change)
>
chore: (updating grunt tasks etc; no production code change)


Example scope values:
>
init
>
runner
>
watcher
>
config
>
web-server
>
proxy

The scope can be empty (e.g. if the change is a global or difficult to assign to a single component), in that case the parentheses are omitted.