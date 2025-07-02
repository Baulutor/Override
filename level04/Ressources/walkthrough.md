# level04

We have a fork that launch a child, this child make the command get without checking bound.

Because it's a child process to find the offset this time we need to use gdb with a particular option named `set follow-fork-mode child`.

Using the offset detection (wiremask buffer overflow), we find an offset of 156.

## Return attack

With that, we have the return address of the child, and we can use that.

Since now, we only use shellcode to get execute it, but this time there is a check if the child exec something.


### Call of system()

Because of the check of the exec, we have to call system() with /bin/sh.
To find it we use gdb this way :

    b main
    run < <("whatever")
    p system (here we get the address of system)
    info proc map (display address of different library including libc)
    find <libc_start>, <libc_end>, "/bin/sh" (get the address of /bin/sh)

Here we have all the things needed, we just need to make the command.

There is a little something too, when we call system() there is the first 4 byte right after that is the return value of system, we don't need that so we just make a padding with any character and then put our /bin/sh that system call.
