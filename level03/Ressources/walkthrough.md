# level03

In this function `int decrypt(EVP_PKEY_CTX *ctx, uchar *out, size_t *outlen, uchar *in, size_t inlen)`

we see /bin/sh if the condition ` if (!strcmp((const char *)v4, "Congratulations!") ` is true. 
So we have to put something with the result to Congratulations.

## What is XOR?

XOR is a binary operation often used in encryption. The cool trick with XOR is that it’s reversible. So, if we know the original and the encrypted value, we can get the key.           
    Encrypted:  Q } | u ` s f g ~ s f { } | a 3. 
    Expected:   C o n g r a t u l a t i o n s ! 
    
By XORing the first character of each string:
    'Q' (0x51) ^ 'C' (0x43) = 0x12 = 18
    which is the same for all characters
    
### Exemple 

    Q is 01010001 in binary 
    C is 01000011 in binary 
    Xor: 00010010 = 18 in decimal
    
## Reverse XOR

We have now the key (18) we should pass 18, but for it, we need to send here:
result = decrypt(a2 - a1);

since `a2 = 322424845`
we need to input `322424827` to get `322424845–322424827 = 18`

Congratulation! We pass the condition and start `system("/bin/sh");`