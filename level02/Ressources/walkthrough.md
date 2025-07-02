# level02

We saw a `system("/bin/sh");` at the end of the function.
So we need to go there but we need to put the same password as the one they compared to.

2 importants informations:
    - there is a printf() without format
    - the password is in a local variable 

## Dump the stack

Since the password is in a local variable, it is located on the stack.
And as we already see we can use the `%p` in the printf, to dump the stack.

We dump the stack sufficiently to show the password. than do it in little endian and finally put it in the binary to launch /bin/sh