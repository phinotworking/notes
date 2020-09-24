## Different ways to ignore a file
1. If the file is NOT already managed by Git
The `.git/info/exclude` file has the same format as any `.gitignore` file. Another option is to set core.excludesFile to the name of a file containing global patterns.

2. If the file is already tracked by Git - and you want to ignore any changes.
ie, you ALREADY have unstaged changes you must run the following after editing your ignore-patterns
```
git update-index --assume-unchanged <file-list>
```

### References
https://stackoverflow.com/questions/1753070/how-do-i-configure-git-to-ignore-some-files-locally
