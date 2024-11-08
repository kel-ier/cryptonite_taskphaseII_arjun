# format string 0

**Flag:** picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_a1d85b3e}

How you approached the challenge:

- Step 1
    read through the code multiple times to fully understand each function 
-step 2
    ran the file through wsl 
-Step 3
    created a file.txt in the directory with text hello inside//not sure how relevant this is but the code seemed to ask for it 
-step 4
    Bufsize is set to 32, in the first function it runs successfully only if the input is greater than 2*32 but must also be on the menu, gr%114dcheese has a '%' symbol meaning whatever is after acts as an access specifier, numbers before d implies it is 114 chaacters long(read in a book about c)
-step 5
    tried the other options successively for the servebob function until i received the flag 

![screenshot](./screenshot.png)

What you learned through solving this challenge:

- Better analysis of code

Other incorrect methods you tried:
- tried inputting random values > 64 in order to bypass the first function as I did not remember %114d
