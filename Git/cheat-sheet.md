# Git Cheat Sheet
**show remote repositories plus url (--verbose)**  
`git remote -v`  

**change remote url**  
`git remote set-url origin <url>`  

**interactive rebase**  
for cleaning up all commits  
`git rebase -i --root master`    

**neater logs**  
`git log --pretty=oneline`  
`git log --oneline` (--pretty=oneline --abbrev-commit)  

**stashing and unstashing changes**  
`git stash`  
`git stash pop`  

**checkout a previous commit**  
`git checkout <commit>`  

**push to a particular branch**  
`git push <remote-name> <local-branch-name>:<remote-branch-name>`  

**configure mergetool**  
`git config merge.tool <mergetool>` 

**fix tracked files**  
use when adding files to .gitignore after they've already been committed  
```
git rm -r --cached .  
git add .  
git commit -m "fixed untracked files"
```     

**overwrite master with another branch**
```
git checkout master  
git reset --hard <other_branch>
```

**add more to your last commit without changing commit msg**
```
git commit . --amend --no-edit
```
