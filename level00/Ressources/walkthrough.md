# level00

We start slowly, we just have to not enter this condition:  `if (local_14[0] != 0x149c)`
to go to the else condition, where is executed /bin/sh.
So we have to put the value `0x149c` in decimal which is : `5276`

and than cat /home/users/level01/.pass