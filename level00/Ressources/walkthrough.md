# level00

We start slowly — we just have to avoid entering this condition: `if (local_14[0] != 0x149c)`, so that we reach the next condition, where `/bin/sh` is executed.

So we need to input the value `0x149c` in decimal, which is `5276`, and then run:

    cat /home/users/level01/.pass