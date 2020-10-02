## Checkout
```bash
git checkout -b <branch_name>  # create & checkout new branch 
```

## Push
```
git push -u origin <branch_name>  # push new branch to origin 
```

## Amend
```bash
git commit --amend --author="Your Name <account@provider.com>"  # amend author
```

## Undo
```bash
git checkout -- path/to/file  # discard local changes in file
git reset HEAD~  # undo unpushed commit
```

## Delete
```bash
git push ${remote_name} --delete ${branch_name}  # delete remote branch
git branch -d ${branch_name}  # delete local, merged branch
git branch -D ${branch_name}  # force delete local (unmereged) branch
```
