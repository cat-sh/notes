# Setting up .gitignore

Three different ignore rules:

- Shared `.gitignore` at the root of your repository
- Personal rules at `.git/info/exclude`
- Global ignore rules defined with `core.excludesFile`

I'm setting up a .gitignore on my home directory for my global ignore rules.

```
$ touch ~/.gitignore
$ git config --global core.excludesFile ~/.gitignore
```

The contents of `~/.gitignore`:

```
.DS_Store
thumbs.db
*.log
```

## Debugging .gitignore files

```
$ git check-ignore -v debug.log
gitignore:3:*.log  debug.log
```

The output shows:

`<file containing the pattern> : <line number of the pattern> : <pattern>`


### Further Reading

[Atlassian - Git ignore](https://www.atlassian.com/git/tutorials/saving-changes/gitignore)
