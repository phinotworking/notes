## Insert a new date using Notepad++

Being inspired by http://todotxt.org, I wanted to keep it simple and just use Notepad++ to manage my todo.txt, instead of a separate app.
When marking a task as done, I would like to insert the current date into there.

1. Plugins -> Plugin Admin
2. Install the **PythonScript** plugin.
3. Create a new script to insert date into there
4. Settings -> Shortcut Mapper
5. Map Control+Alt+D to the `adddate.py` file

Create a new python file (addDate.py) and put it into the plug-ins folder

``` python
import time

editor.addText ('x ' + time.strftime ('%Y-%m-%d') + ' ')
```

### References
https://luciano.defalcoalfano.it/blog/show/notepad-add-a-macro-to-insert-current-date-and-time-in-text
