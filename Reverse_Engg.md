# GDB baby step 1

**Flag:** picoCTF{549698}

How you approached the challenge:

- step 1
sudo apt install gdb

- step 2
gdb debugger0_a

- step 3
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
- step 4
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



# vault-door-3

**Flag:** picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}

How you approached the challenge:

- step 1:
   read through the java file to understand 


- step 2:
   The cheeckPassword() function creates a new string and then assigns data to it through the 'password' string

- Step 3:
   initially I considered working through it manually since we do that a lot already but then I realized I wasn't limited to that and wrote code on c to run the loops

- Step 4:
  ![Code on C](https://github.com/user-attachments/assets/2270bb2d-74a0-4e5b-a91d-48fb165152f1)


- Output 
   ```jU5t_a_s1mpl3_an4gr4m_4_u_c79a21```


What you learned through solving this challenge:

1. Finally feel like I'm starting to think more in terms of whats right

Other incorrect methods you tried:
- Tried using string formatting similar to the previous challenge 
- tried running the code by itself with


#ARMssembly 1

**Flag:** picoCTF{0000001B}

I went through the entire script in order to understand what was happening, starting with the function:

![image](https://github.com/user-attachments/assets/f19507c2-5859-4ce1-aa2a-7777ea81334a)

then moved to main to finish the last part:

![image](https://github.com/user-attachments/assets/c728ad74-48a1-4db4-8bfa-6567bf42d05f)

What you learned through this challenge:
1. Basics of assembly including registers and movement through the stack

References:
- [ARMsembbly 0](https://www.youtube.com/watch?v=1d-6Hv1c39c)
- [WZR](https://developer.arm.com/documentation/dui0801/l/Overview-of-AArch64-state/Registers-in-AArch64-state)
- [lsl](https://www.studocu.com/en-us/document/massachusetts-institute-of-technology/computer-system-architecture/shift-and-rotate-instructions/54580778)
- [sdiv](https://developer.arm.com/documentation/dui0473/m/arm-and-thumb-instructions/sdiv)



