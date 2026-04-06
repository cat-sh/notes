# Good Git Commit Practices

- Keep commit messages short (50 chars or less for titles, 72 chars wrapped lines for the body)

- Write in the imperative mode: "Fix", "Add", "Change" instead of "Fixed", "Added", "Changed"

- Don't end the summary with a point

- If it's difficult to summarise what the commit does in the title, it is better to split in several commits with `git add -p`

- `git add -p` lets you choose change by change, what to add to the next commit (y=add, n=don’t add)

- `git checkout -- filename` cancels changes in tracked files

- Don't forget to branch!

- Ignore a previously committed file - delete file and add `.gitignore` rule for it. `git rm --cached debug.log` using the `--cached` option keeps the local file but deletes from remote


### Further Reading:

[Writing good commit messages](https://github.com/erlang/otp/wiki/writing-good-commit-messages)