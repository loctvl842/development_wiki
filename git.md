# Git

## Remove a file completely from the repository

```sh
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch file_to_remove.ext' --prune-empty --tag-name-filter cat -- --all
```

After running this command, Git will rewrite the history of all branches and tags, removing any commits that included the specified file. Be aware that this operation is destructive and should be used with caution, especially if the repository is shared with others.

Once you've executed this command, you'll need to force-push the changes to update the remote repository
```sh
git push --force
```
