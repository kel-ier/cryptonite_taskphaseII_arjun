# Trivial Flag Transfer Protocol


**Flag:** picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

How you approached the challenge:

- step 1
    Looked up how to open a pcapng file, downloaded wireshark 

- step 2
    Firgured out how to use wireshark enough to access the files hidden

- step 3
    read the instructions, tried deciphering using cyberchef and then tried the most common ciphers on cryptii

- step 4 
    read more about .deb files and how to extract them 

- step 5
    used ar to extract the files 

- Step 6
    used tar to extract the control file, and read the contents and installed steghide

- step 7 
    ```steghide extract -sf picture3.bmp```
- step 8
    Used 'DUEDILIGENCE' as passphrase for all three pictures


What you learned through solving this challenge:

1. Basic understanding of Wireshark 
2. Better analysis 


References

- [Wireshark](https://www.reddit.com/r/wireshark/comments/6ndvpq/extract_tftp_file_from_pcapng/)
- [.deb](https://www.cyberciti.biz/faq/how-to-extract-a-deb-file-without-opening-it-on-debian-or-ubuntu-linux/)
- [.bmp](https://coderanch.com/t/111069/os/bmp-file)




# tunn3l v1s10n


**Flag:** picoCTF{qu1t3_a_v13w_2020}

How you approached the challenge:

- step 1
    Checked the file using file and exiftool

- step 2 
    File extension was bmp, so I chanegd it to that but couldn't open the file 

- step 3
    read about different operations that can be done on bmp files

- step 4
    tried to understand the meaning of the bits that are stored

- step 5
    changed different bits messing around, tried setting the height to equal the width and got the flaf


What you learned through solving this challenge:

1. understanding of the size for different parts of bmp files


References

- [BMP1](https://www.ece.ualberta.ca/~elliott/ee552/studentAppNotes/2003_w/misc/bmp_file_format/bmp_file_format.htm)
- [BMP2](https://en.wikipedia.org/wiki/BMP_file_format)
-  [editor](https://hexed.it/)
