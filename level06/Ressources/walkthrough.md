# Level06

This level is a **crypted** password that we need to get.

The `auth()` function is the one we are going to use to print what the result is.

Since the function uses the login to generate the password, we just need to print what the result of the encryption is with a given login, and it works.

There is just this, which indicates that the login must be at least 6 characters long:

```c
if ((int)sVar1 < 6) {
    uVar2 = 1;
}
```
### Exemple

    login :             cccccc
    password will be :  6232799