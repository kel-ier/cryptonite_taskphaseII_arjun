# Trivial Flag Transfer Protocol


**Flag:** picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

How you approached the challenge:

- step 1:
    Looked up how to open a pcapng file, downloaded wireshark 

- step 2:
    Firgured out how to use wireshark enough to access the files hidden

- step 3:
    read the instructions, tried deciphering using cyberchef and then tried the most common ciphers on cryptii

- step 4:
    read more about .deb files and how to extract them 

- step 5:
    used ar to extract the files 

- Step 6:
    used tar to extract the control file, and read the contents and installed steghide

- step 7 :
    ```steghide extract -sf picture3.bmp```
- step 8:
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

- step 1: Checked the file using file and exiftool

- step 2: File extension was bmp, so I chanegd it to that but couldn't open the file 

- step 3:
    read about different operations that can be done on bmp files

- step 4:
    tried to understand the meaning of the bits that are stored

- step 5:
    changed different bits messing around, tried setting the height to equal the width and got the flaf


What you learned through solving this challenge:

1. understanding of the size for different parts of bmp files


References

- [BMP1](https://www.ece.ualberta.ca/~elliott/ee552/studentAppNotes/2003_w/misc/bmp_file_format/bmp_file_format.htm)
- [BMP2](https://en.wikipedia.org/wiki/BMP_file_format)
-  [editor](https://hexed.it/)




# m00nwalk

**Flag:** picoCTF{beep_boop_im_in_space}

How you approached the challenge:

- step 1:
    looked online to find different results related to the sending of images from space and came across sstv

- step 2:
    downloaded qsstv 

- step 3:
    used pavucontrol to make sure the input to qsstv was null

- step 4:
    set mode to auto

- step 5:
    ran the audio file using paplay 
        ```paplay -d virtual-cable message.wav```


What you learned through solving this challenge:

1. usage of qsstv

Other incorrect methods you tried:

- tried using an online sstv decoder
- couldn't get the file to work with qsstv
- mode was set to manual and didn't work
- auto slant was off

References

- [qsstv](https://ourcodeworld.com/articles/read/956/how-to-convert-decode-a-slow-scan-television-transmissions-sstv-audio-file-to-images-using-qsstv-in-ubuntu-18-04)

# m00nwalk2

**Flag:** picoCTF{the_answer_lies_hidden_in_plain_sight}

How you approached the challenge:

- step 1:
    ran qsstv on the message and got the same image as previous challenge

- step 2:
    used it on first clue to get
  
   ![image](https://github.com/user-attachments/assets/8a963542-aa95-4752-82ad-cbbec4f887a9)


- step 3:
    clue 2:
  
  ![image](https://github.com/user-attachments/assets/baae6bbe-1fc2-4e6f-9b1f-8d5bd8750689)



- step 4:
    clue 3:
  
  ![image](https://github.com/user-attachments/assets/c1dbc10b-b837-4efe-92d3-6460672215c5)



- step 5:
    looked up Alan eliasen the future boy and got the following result:
  
   ![image](https://github.com/user-attachments/assets/6d868c1b-7c6c-4706-a37f-b7b27c891195)



- step 6:
    The password mentioned STEGosaurus which reminded me of steghide which I used for previous challenge

- step 7:
    used steghide on the message with the password mentioned in first clue and got the flag


Other incorrect methods you tried:

- tried to use steghide on the image generated 
