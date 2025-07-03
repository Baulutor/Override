# Level08 Exploit Guide

## Objective

Gain access to the `.pass` file located in another user's home directory, even without having direct read permission.

---

## Binary Behavior

After decompiling the binary, we observe the following behavior:

- It expects **one argument**: the name of the file to back up.
- It uses a custom function `log_wrapper()` to log the start and end of the backup.
- Here's the backup process:

  1. Opens a log file at `./backups/.log`.
  2. Logs the start of the backup.
  3. Opens the file provided as an argument.
  4. Prepares the destination path: `./backups/`.
  5. Copies the file content byte by byte.
  6. Logs the end of the backup.

The destination path is constructed like this in code:

```c
strncpy(local_78, "./backups/", 11);
strncat(local_78, filename, ...);
```

If we pass an argument like:

    /home/users/level09/.pass
The final backup path becomes:

```bash
./backups//home/users/level09/.pass
```
This means the program opens the file from the absolute path, then writes the copied content to the same structure under ./backups/.

## Exploitation
The vulnerability lies in the lack of input sanitization. The program directly appends user input to the backup directory path, allowing directory traversal and arbitrary file reading.

### Steps to Exploit

    cd /tmp
    mkdir -p ./backups//home/users/level09/
    ~/level08 home/users/level09/.pass
    cat /tmp/backups/home/users/level09/.pass

## Result:

The program copies the contents of the target .pass file into our local ./backups/ directory. Since we own this structure, we can now read the file freely.