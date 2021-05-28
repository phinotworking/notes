The following should quickly delete all files in a folder.
This works for a CMD prompt (not PowerShell)
```
RMDIR /Q/S node_modules
```

* /Q -- Quiet mode, won't prompt for confirmation to delete folders.
* /S -- Run the operation on all folders of the selected path.
* foldername -- The absolute path or relative folder name, e.g. o:/backup/test1 or test1

### References
https://www.ghacks.net/2017/07/18/how-to-delete-large-folders-in-windows-super-fast/
