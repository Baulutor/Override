# level01

there is 2 inputs needed in this level.
One is for the username and one is for the password 

The username `dat_will` is cleary write in the `int verify_user_name(void)`

## Return address attack

For the password we can overwrite the local_54 in `fgets(local_54,100,stdin)`

because the buffer of local_54 is 64 and the limit  of the fgets is 100.
So I can overwrite the return address with the address of the fgets where I put my shellcode to execute the shell.
