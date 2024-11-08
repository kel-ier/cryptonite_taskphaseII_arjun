# GDB baby step 1

**Flag:** picoCTF{549698}

How you approached the challenge:

- step 1
sudo apt install gdb

- step 2
gdb debugger0_a

-step 3
info functions
```
0x0000000000001000  _init
0x0000000000001030  __cxa_finalize@plt
0x0000000000001040  _start
0x0000000000001070  deregister_tm_clones
0x00000000000010a0  register_tm_clones
0x00000000000010e0  __do_global_dtors_aux
0x0000000000001120  frame_dummy
0x0000000000001129  main
0x0000000000001140  __libc_csu_init
0x00000000000011b0  __libc_csu_fini
0x00000000000011b8  _fini
```
-step 4
disassmble main

```
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
```

-step 5
Converted 0x86342 to decimal

What you learned through solving this challenge:

1. Basics of GDB 
2. Concept of deassembling  
Other incorrect methods you tried:

- tried using info registers to get any data present inside %eax
- got stuck trying to understand how to find the data moved into the registers 


References

- [reference 1](https://www.tutorialspoint.com/gnu_debugger/index.htm)
- [reference 2](https://www.geeksforgeeks.org/gdb-step-by-step-introduction/)
- Reference 3 : help command on GDB