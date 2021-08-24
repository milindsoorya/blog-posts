## 8 underrated git commands every programmer should know (not the usual pull, push, add, commit)


> I will be adding more useful git tips, if you find this helpful I suggest bookmarking this page. 🔖

## 1. Rename a local branch
useful when you mess up the branch name with some typos.
```
// Note: no need to include < >, separate words with -
git branch -m <new_name>
eg:- git branch -m new-new-branch
``` 

## 2. Change the upstream branch
use this to push the local branch to a new remote branch.
```
git push origin -u <new_name>
``` 

## 3. Makes local branch same as remote
At times we may make many changes to our local branch and end up with something that is worse than what we started with, don't worry everyone has been there 😅. This command will be helpful in those scenarios.
```
// replace staging with the branch you want to reset to
git reset --hard origin/staging
```

## 4. Delete the most recent commit, keeping the work you've done
I am amazed that not too many people know of this command, this will help to recover from those stupid typos that creep into our commits.
```
git reset --soft HEAD~1
```

## 5. Delete the most recent commit, destroying the work you've done
Use this command when you know you really messed up. don't fret it's part of the journey.🎯
```
git reset --hard HEAD~1
```

## 6. Stash your work
Stash command is used to temporarily work on another branch without committing our current work.
```
git stash
```

## 7. Recover stash by going into that branch and
```
git stash apply
```
please note that `git stash apply` won't delete the stashed item from the stash list. If you want to recover stash and drop it from the stash list use `git stash pop`.

## 8. Go back to a previous commit, undo a rebase
It is quite natural to mess up a rebase, these commands will hopefully save you. use reflog command to find the head number of the required commit you want to reset to.
```
// Find the head at that point
git reflog 

// Replace 5 with the head number, please be extra careful not to 
// give wrong wrong head number
git reset --hard "HEAD@{5}"
```
---

👉🏼 checkout my website,  [milindsoorya.site](https://milindsoorya.site/)  for more updates and getting in touch. cheers.

Thank you very much for reading, liking and commenting on my articles. As you know, writing quality articles takes time and effort. Therefore, if you have enjoyed my article or if it was helpful please support me by  [buying me a coffee](https://www.buymeacoffee.com/milindsoorya) ☕ 😇.

