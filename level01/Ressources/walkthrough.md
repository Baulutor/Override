# level01

There are 2 inputs needed at this level:  
one is for the username, and one is for the password.

The username `dat_will` is clearly written in the `int verify_user_name(void)` function.

## Return Address Attack

For the password, we can overwrite `local_54` in:

```c
fgets(local_54, 100, stdin)
```

because the buffer local_54 is 64 bytes, and the limit for fgets is 100.
So we can overwrite the return address with the address of the buffer where we placed our shellcode, in order to execute a shell.