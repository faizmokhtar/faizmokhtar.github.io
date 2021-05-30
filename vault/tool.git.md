---
id: f6ceb740-2750-4244-950f-a384298bd226
title: Git
desc: ''
updated: 1621222166302
created: 1618842424323
---

## Git Cheatsheet

### Squashing 4 commits
```
git checkout $BRANCH_NAME
git rebase -i $BRANCH_NAME~4 $BRANCH_NAME

# at interactive screen
# then choose the fixup for commit: 2/3/4

git push -u origin +$BRANCH_NAME
```

### Setting global default branch name to `main`

```
git config --global init.defaultBranch main
```