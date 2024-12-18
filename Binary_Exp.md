# format string 0

**Flag:** picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_a1d85b3e}

How you approached the challenge:

- Step 1
    read through the code multiple times to fully understand each function
  
- Step 2
    ran the file through wsl 
    
- Step 3
    created a file.txt in the directory with text hello inside//not sure how relevant this is but the code seemed to ask for it 
    
- Step 4
    Bufsize is set to 32, in the first function it runs successfully only if the input is greater than 2*32 but must also be on the menu, gr%114dcheese has a '%' symbol meaning whatever is after acts as an access specifier, numbers before d implies it is 114 chaacters long(read in a book about c)
    
- Step 5
    tried the other options successively for the servebob function until i received the flag 


What you learned through solving this challenge:

- Better analysis of code

Other incorrect methods you tried:
- tried inputting random values > 64 in order to bypass the first function as I did not remember %114d


# Buffer Overflow 0

**Flag:** picoCTF{ov3rfl0ws_ar3nt_that_bad_ef01832d}

How you approached the challenge:

- Went off of instinct on a whim tried to overload the string because thats what was needed last time

- Once the input was accepted, the vuln() function is called. Since the function is named vuln, I assumed thats where the key is

- when vuln() is called, a string of size 16 is created, so I tried giving an input greater than 16 characters

What you learned through solving this challenge:

- Deeper understanding of the vulnerability of gets()

- Tried understanding the signal() function


References

- [Signal()](https://www.tutorialspoint.com/c_standard_library/c_function_signal.htm)
- [gets()](https://www.man7.org/linux/man-pages/man3/gets.3.html)




# Flag Leak 
**Flag:** picoCTF{L34k1ng_Fl4g_0ff_St4ck_11a2b52a}

How you approached the challenge:

- step 1
    started off by trying different format specifiers to get different outputs 

- step 2
    using %x gave me different values vor different positions like %1x,%2x

- step 3
    Used %$s repeatedly to try to get the flag

- step 4
    ran a for loop:
    ```for i in {0..99}; do echo "%${i}\$s" | nc saturn.picoctf.net 52796 ; done```
    to get the flag



What you learned through solving this challenge:

1. format string attacks
2. for loops in bash


Other incorrect methods you tried:
- Tried overflowing the stack
- Had to find out how to use varoables in for loops

References

- [Format String attacks](https://ctf101.org/binary-exploitation/what-is-a-format-string-vulnerability/)
- [For loops](https://www.redhat.com/en/blog/bash-scripting-loops)


