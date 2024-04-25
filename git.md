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

## Stat changes in the working directory

Show a brief summary of changes in the repository, including the files that were modified, added, or deleted, along with the number of lines changed in each file

```sh
git log --stat
```

## Skip tracking changes for file

```sh
git update-index --skip-worktree file_to_skip.ext
```

To undo this operation and start tracking changes to the file again, use the following command:
```sh
git update-index --no-skip-worktree file_to_skip.ext
```

To list all files that are skipped, use the following command:
```sh
git ls-files -v | grep '^S'
```

The command marks a file to be skipped in future operations, particularly useful for ignoring changes to tracked files.

