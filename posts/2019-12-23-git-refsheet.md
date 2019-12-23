* Create & checkout new branch `git checkout -b <branch_name>`
* Push new branch to origin `git push -u origin <branch_name>`

## Amend
### Amend autor
`git commit --amend --author="Your Name <account@provider.com>"`

## Undo
### Undo (discard) local changes in file
`git checkout -- <filename>`

### Undo unpushed commit
```
git reset HEAD~
# do stuff
git add ...
git commit -c ORIG_HEAD
```
