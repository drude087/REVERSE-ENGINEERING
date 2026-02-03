
# REVERSE (PICO-CTF)

## Date
03-Feb-2026

## Initial Assessment
The application prompts the user to enter the password to unlock this file
## Code Execution
The binary was executed locally and prompted for a string input. Random values resulted in a failure message, indicating a direct comparison check.

```bash
$ ./ret
Enter the password to unlock this file: something
You entered: something
Access denied
```
<img src="IMAGES\EXECUTING THE FILE REVERSE.png" width="600">

## File Type
```bash
file ret 
ret: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=32195c65c0c8ca5bd239fa824d4d79231cca5f78, for GNU/Linux 3.2.0, not stripped
```
<img src="IMAGES\FILE TYPE REVERSE.png" width="3600" height="40">
It is not stripped which means we can see the function names and other details of the code easily

## Ltrace
```bash
ltrace ./ret
printf("Enter the password to unlock thi"...)                                                    = 40
__isoc99_scanf(0x56379e09c031, 0x7fff95217ee0, 0, 0Enter the password to unlock this file: something
)                                             = 1
printf("You entered: %s\n", "something"You entered: something
)                                                         = 23
strcmp("something", "picoCTF{3lf_r3v3r5ing_succe55ful"...)                                       = 3
puts("Access denied"Access denied
)                                                                            = 14
+++ exited (status 0) +++

```
<img src="IMAGES\LTRACE REVERSE.png" width="600">

we can see that it is comparing the enterted value to ```picoCTF{3lf_r3v3r5ing_succe55ful```

## Retrying with data we got
```bash
./ret 
Enter the password to unlock this file: picoCTF{3lf_r3v3r5ing_succe55ful
You entered: picoCTF{3lf_r3v3r5ing_succe55ful
Access denied
```
<img src="IMAGES\retrying reverse.png" width="600">

We still got access denied. Lets open up the binary in Gidra to see whats going on and what the full data is

## Ghidra
### After analysis move to 'what is the password' section
<img src="IMAGES\GHIDRA REVERSE 1.png" width="600">

### Open it with Decompile option
<img src="IMAGES\GHIDRA CODE REVERSE.png" width="600">

### Extract the code
<img src="IMAGES\CODE REVERSE.png" width="600">

## Code
```c
undefined8 main(void)
{
    int iVar1;
    long in_FS_OFFSET;
    char local_68[48];
    char local_38[40];
    long local_10;

    local_10 = *(long *)(in_FS_OFFSET + 0x28);
    builtin_strncpy(local_38,
        "picoCTF{3lf_r3v3r5ing_succe55ful_c83965d}", 0x28);

    printf("Enter the password to unlock this file: ");
    __isoc99_scanf(&DAT_00102031, local_68);
    printf("You entered: %s\n", local_68);

    iVar1 = strcmp(local_68, local_38);
    if (iVar1 == 0) {
        puts("Password correct, please see flag:");
        puts("picoCTF{3lf_r3v3r5ing_succe55ful_c83965de}");
    } else {
        puts("Access denied");
    }

    if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
        __stack_chk_fail();
    }
    return 0;
}
```
we can see that it is using string compare to check if the entered password is same as the one hardcoded in the binary. The value is ```picoCTF{3lf_r3v3r5ing_succe55ful_c83965d```

You can get the flag from the code or paste the value when executing to get it

## Flag
```bash
./ret
Enter the password to unlock this file: picoCTF{3lf_r3v3r5ing_succe55ful_c83965d
You entered: picoCTF{3lf_r3v3r5ing_succe55ful_c83965d
Password correct, please see flag: picoCTF{3lf_r3v3r5ing_succe55ful_c83965de}
picoCTF{3lf_r3v3r5ing_succe55ful_c83965d
```

<img src="IMAGES\FLAG REVERSE.png" width="600">

## Conclusion
The challenge can be solved trivially by reverse engineering due to the presence of hardcoded validation logic in the binary. No dynamic checks, obfuscation, or server-side validation mechanisms are used, making the flag easily retrievable through static analysis rather than exploitation or guessing.