# level02

We saw a `system("/bin/sh");` at the end of the function.  
So we need to go there, but we also need to enter the same password that is being compared.

Two important pieces of information:

- There is a `printf()` without a format specification.
- The password is stored in a local variable.

## Dump the Stack

Since the password is in a local variable, it is located on the stack.  
And as we've already seen, we can use `%p` in the `printf` to dump the stack.

We dump the stack sufficiently to reveal the password, then convert it to Little Endian, and finally input it into the binary to launch `/bin/sh`.
