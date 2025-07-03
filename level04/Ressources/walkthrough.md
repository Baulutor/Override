# level04

We have a fork that launches a child. This child runs the `gets` command without checking bounds.

Because it's a child process, to find the offset this time we need to use GDB with a particular option named `set follow-fork-mode child`.

Using offset detection (wiremask buffer overflow), we find an offset of 156.

## Return attack

With that, we have the return address of the child, and we can use that.

Until now, we only used shellcode to get it executed, but this time there is a check if the child executes something.

### Call of system()

Because of the execution check, we have to call `system()` with `/bin/sh`.  
To find it, we use GDB this way:

```bash
    b main
    run < <("whatever")
    p system          # here we get the address of system
    info proc map     # displays addresses of different libraries including libc
    find <libc_start>, <libc_end>, "/bin/sh"   # gets the address of /bin/sh
```

Here we have all the needed components — we just need to build the command.

There is a little something too: when we call system(), the first 4 bytes right after are the return value of system.
We don't need that, so we just add padding with any characters, and then put our /bin/sh that system will call.


