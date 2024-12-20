# Cookies
**Flag:** picoCTF{3v3ry1_l0v3s_c00k135_bb3b3535}

How you approached the challenge:


- Step 1:
    opened the link provided and checked for cookies 
    
- Step 2:
    Used inspect element to check for cookies //similar to the OASIS ctf
  
- step 3:
    changed the value of the cookie prrogressively, refreshing each time 
    
- step 4:
    reached 18, flag appeared on screen


What you learned through solving this challenge:

1. Manipulation of cookie data through inspect element

Other incorrect methods you tried:

- tried using the name of the cookie as the input //realized the value differs when I used snickerdoodle as the input for the question
References
- [reference 1](https://www.geeksforgeeks.org/view-edit-and-delete-cookies-in-microsoft-edge-browser/)


# Forbidden Paths

**Flag:** picoCTF{7h3_p47h_70_5ucc355_e5a6fcbc}

How you approached the challenge:
- Step 1:
    The searchbar directly asked for filename rather than anything else, such as book name, hinting that the actual file was being utilised directly

- Step 2: 
    Tried using filepaths and I could access the three books like that

- Step 3: 
    used '..' to move back through the path and then /flag.txt to open the flag file


What you learned through solving this challenge:
1. Looked further into storage of files in a website
Other incorrect methods you tried:
- Tried directly accessing the file in the source
- Tried finding the file somewhere within the books


# SOAP

**Flag:** picoCTF{XML_3xtern@l_3nt1t1ty_e5f02dbf}

How you approached the challenge:

- step 1
    Opened the website on burpsuite

- step 2
    sent the request to the repeater 

- ste 3:
    Tried different payload commands to see if it worked

- step 4: 
    modifed the data with 
    ```<?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
    <stockCheck><productId>&xxe;</productId></stockCheck>```

- Step 5:
    received the flag in the response


What you learned through solving this challenge:

1. XXE 
2. Modifying the script through burpsuite

Other incorrect methods you tried:

- tried changing the request path to /etc/passwd
- response wasn't received for most attempts until I switched from wifi to mobile network

References

- [xxe](https://portswigger.net/web-security/xxe)
- [payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XXE%20Injection/README.md)

