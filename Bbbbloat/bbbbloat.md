
# Bbbbloat (PICO-CTF)

## Date
04-Feb-2026

## Initial Assessment
The application prompts the user to enter its favorite number
## Code Execution
The binary was executed locally and prompted for a int input. Random values resulted in a failure message, indicating a direct comparison check.

```bash
$ ./bbbbloat
What's my favorite number? 4756
Sorry that's not it!
```
<img src="IMAGES\execution.png" width="600">

## File Type
```bash
file bbbbloat 
bbbbloat: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=7f6577a0ee7bbb89247a73c7de24236acb749cee, for GNU/Linux 3.2.0, stripped                                                              
```
<img src="IMAGES\file type.png" width="600" height="20">
It is stripped which means we can't see the code details clearly

## Ltrace
```bash
ltrace ./bbbbloat
printf("What's my favorite number? ")                                                                                = 27
__isoc99_scanf(0x55ae740ce020, 0x7ffe0b38f830, 0xd2c49, 0xd2c49What's my favorite number? 56
)                                                     = 1
puts("Sorry, that's not it!"Sorry, that's not it!
)                                                                                        = 22
+++ exited (status 0) +++
```
<img src="IMAGES\ltrace.png" width="600">

## Strace
```bash
 strace ./bbbbloat
execve("./bbbbloat", ["./bbbbloat"], 0x7ffc941ea3b0 /* 56 vars */) = 0
brk(NULL)                               = 0x56062f246000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5fbe391000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=96555, ...}) = 0
mmap(NULL, 96555, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f5fbe379000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0p\236\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 840, 64) = 840
fstat(3, {st_mode=S_IFREG|0755, st_size=2003408, ...}) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 840, 64) = 840
mmap(NULL, 2055800, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f5fbe183000
mmap(0x7f5fbe1ab000, 1462272, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x28000) = 0x7f5fbe1ab000
mmap(0x7f5fbe310000, 352256, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x18d000) = 0x7f5fbe310000
mmap(0x7f5fbe366000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1e2000) = 0x7f5fbe366000
mmap(0x7f5fbe36c000, 52856, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f5fbe36c000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f5fbe180000
arch_prctl(ARCH_SET_FS, 0x7f5fbe180740) = 0
set_tid_address(0x7f5fbe180a10)         = 14320
set_robust_list(0x7f5fbe180a20, 24)     = 0
rseq(0x7f5fbe180680, 0x20, 0, 0x53053053) = 0
mprotect(0x7f5fbe366000, 16384, PROT_READ) = 0
mprotect(0x5605f4ead000, 4096, PROT_READ) = 0
mprotect(0x7f5fbe3cf000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7f5fbe379000, 96555)           = 0
fstat(1, {st_mode=S_IFCHR|0600, st_rdev=makedev(0x88, 0), ...}) = 0
getrandom("\xa5\x6c\x90\x76\x7f\x41\x21\xad", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x56062f246000
brk(0x56062f267000)                     = 0x56062f267000
fstat(0, {st_mode=S_IFCHR|0600, st_rdev=makedev(0x88, 0), ...}) = 0
write(1, "What's my favorite number? ", 27What's my favorite number? ) = 27
read(056
, "56\n", 1024)                   = 3
write(1, "Sorry, that's not it!\n", 22Sorry, that's not it!
) = 22
lseek(0, -1, SEEK_CUR)                  = -1 ESPIPE (Illegal seek)
exit_group(0)                           = ?
+++ exited with 0 +++
```
<img src="IMAGES\strace.png" width="600">
Lets open up the binary in Gidra to see whats going on and what the full data is

## Ghidra
### After analysis move to 'What's my favorite number?' section
<img src="IMAGES\gidra1.png" width="600">

### Open it with Decompile option
<img src="IMAGES\gidracode.png" width="600">

## Extract the code
### Code
```c
undefined8 FUN_00101307(void)

{
  long in_FS_OFFSET;
  int local_48;
  undefined4 local_44;
  char *local_40;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined8 local_20;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_38 = 0x4c75257240343a41;
  local_30 = 0x3062396630664634;
  local_28 = 0x65623066635f3d33;
  local_20 = 0x4e326560623535;
  local_44 = 0xd2c49;
  printf("What\'s my favorite number? ");
  local_44 = 0xd2c49;
  __isoc99_scanf(&DAT_00102020,&local_48);
  local_44 = 0xd2c49;
  if (local_48 == 0x86187) {
    local_44 = 0xd2c49;
    local_40 = (char *)FUN_00101249(0,&local_38);
    fputs(local_40,stdout);
    putchar(10);
    free(local_40);
  }
  else {
    puts("Sorry, that\'s not it!");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```
we can see that it is storing our value and checking it to an hexadecimal value using if condition. Now we just need to convert the hexadecimal value its checking to decimal value to get our number

<img src="IMAGES\binary.png" width="600">

The decemal number for 0x86187 is 549255

## Flag
```bash
./bbbbloat       
What's my favorite number? 549255
picoCTF{cu7_7h3_bl047_36dd316a}

```

<img src="IMAGES\output.png" width="600">

## Conclusion


The binary prompts the user to guess a favorite number and reveals the flag on correct input. Static analysis using Ghidra shows that the program directly compares the user input against a hardcoded hexadecimal constant (0x86187). Converting this value to decimal and supplying it as input satisfies the condition and triggers the flag output. Since the correct value is embedded directly in the binary and no dynamic or protected validation is performed, the challenge can be solved trivially through reverse engineering rather than guessing or exploitation.

