Open any text editor to follow along. 

The first line of every `shell script` we  write should be the `shebang` line. 

It starts with a hash mark `(#)` and an exclamation mark `(!)`, and it `declares the interpreter to use for the script`. 

This allows the `plaintext file to be executed like a binary`. 

We’ll use it to indicate that we’re using bash.

When saving the script file, it is good practice to place commonly used scripts in the `~/bin/`directory

The script files also need to have the “execute” permission to allow them to be run. To add this permission to a file with filename: `script.sh` use:

```shell
chmod +x script.sh
```
