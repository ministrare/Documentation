# Personal Documentation
## Git / Remove

### Index
- [Removing a file added in the most recent unpushed commit](#Removing-a-file-added-in-the-most-recent-unpushed-commit)
- [](./)
- [](./)
- [](./)
- [](./)

### Removing a file added in the most recent unpushed commit
If the file was added with your most recent commit, and you have not pushed to GitHub, you can delete the file and amend the commit:

1. Open Git Bash.
1. Change the current working directory to your local repository.
1. To remove the file, enter git rm --cached:
   ```git
    $ git rm --cached giant_file
    
    # Stage our giant file for removal, but leave it on disk
   ```
1. Commit this change using `--amend -CHEAD`:
   ```git
    $ git commit --amend -CHEAD
    
    # Amend the previous commit with your change
    # Simply making a new commit won't work, as you need
    # to remove the file from the unpushed history as well
    ```
Push your commits to GitHub:
    ```git
    $ git push
    # Push our rewritten, smaller commit    
    ```