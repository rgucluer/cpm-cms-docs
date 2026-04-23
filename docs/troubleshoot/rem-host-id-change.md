## WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! 

This can happen after deleting, and recreating virtual machines. The recorded (old) vm information does not fit with the current vm information. 

Open a new terminal. Edit manually /home/< user name >/.ssh/known_hosts with a text editor. Read the error message, find the related row, delete, save, close.
```bash
...
Offending ED25519 key in /home/< user name >/.ssh/known_hosts:170
...
```
So look at row 170, delete row, save, exit.

To list current line number in nano hit <kbd>CTRL</kbd> + <kbd>C</kbd>

### Last resort (not recomended) ; remove all known hosts
```bash
rm ~/.ssh/known_hosts
```



