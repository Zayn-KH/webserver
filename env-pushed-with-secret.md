# How to do after push .env, .env.production, .env.dev that contains secret by mistake?
# Make sure nobody forgot to push the code before doing this process
---
Please be serious this process is dangerous but we have way to solve it 
---
# STAGE 1
1. Let's try to remove by added .env to .gitignore
```
# env production file
.env.production
```

2. After added to .gitignore we must push the code first
3. After pushed that .env file still on the git even the git commit history
4. Let's remove the git cached

```
git rm -rf --cached .env.production
```
And then just push the code.
5. After remove cache the .env.production file will remove from the git but still exist all of history in the commit
6. Let's go to last step with the command please be carefully
```
git filter-branch --index-filter "git rm -rf --cached --ignore-unmatch .env.production" HEAD
```
After above command let's force push
```
git push --force
```

# Congrats your env already remove from the cached and history commit
# But how to solve if your team mistaking using that branch but forgot to push the code before doing that process
# They cannot push the code after this process. 
# And cannot even merge the pull request after added new branch.

# STAGE 2

1. Just make the code safe by adding new branch with that unpush code.
2. Let's push that new branch with the code.
3. Let's try with cherry pick code.
4. Let's checkout the branch that we just process from the Stage 1 above
5. Cherry pick the code from the commit of that new branch
6. After that let's push the cherry picked code.

# Congrats now everything solved successfully