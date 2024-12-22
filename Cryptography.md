# Vigenere

**Flag:** picoCTF{D0NT_US3_V1G3N3R3_C1PH3R_d85729g7}

How you approached the challenge:

- Ran the cipher through cryptii using vignere and changed the variant to Beaufort cipher

Other incorrect methods you tried:

- before I changed the variant, I thought I had to repeteadly decode



# Caesar

**Flag:** picoCTF{crossingtherubicondjneoach}

How you approached the challenge:

- Used the decoder and kept changing the shift value until I got something remotely english





# miniRSA

**Flag:** picoCTF{n33d_a_lArg3r_e_606ce004}

Solving this challenge involved various steps of learning along with a lot of other tangents;
I began by reading about how rsa encryptions are done, about the two types of keys and how the message is padded and then encrypted. 
Writng the actual equation down and then solving for m, we get m = eth root of(c+in). We know that m is only very slightly larger than n^e so i must be small. Now I had to find a way to get the root of such a large number. While looking into small e, I came across this code segment: 
```
 from gmpy2 import iroot
m = iroot(ct, e)
```
This basically solved the problem. Looking into iroot I found that it returns and integer and a boolean value, the boolean value representing whether the returned integer is the true root

I wrote the following python script to find the flag:

```
from gmpy2 import iroot
from Crypto.Util.number import long_to_bytes
n = 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673
c = 2205316413931134031074603746928247799030155221252519872649649212867614751848436763801274360463406171277838056821437115883619169702963504606017565783537203207707757768473109845162808575425972525116337319108047893250549462147185741761825125
m = 0
e=3

for i in range(100):
    m,trueroot = iroot(c+i*n,e)
    if (trueroot):
        print (long_to_bytes(m))
        exit(0)

```
this produced the flag as the output

## Wrong methods:

- I tried to factorise the number using an online factoriser in order to find the actual value of the private key but it was too large
- Could not figure out how to unpad the message once I received it

## What I learned:

- Basics of RSA encryption
- Python scripting

## References:

- [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))
- [small e](https://ir0nstone.gitbook.io/crypto/rsa/public-exponent-attacks/small-e)
- [gmpy2](https://readthedocs.org/projects/gmpy2/downloads/pdf/latest/)



# C3

**Flag:** picoCTF{adlibs}

When we start we are presented with this python script:
```
import sys
chars = ""
from fileinput import input
for line in input():
  chars += line

lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""

prev = 0
for char in chars:
  cur = lookup1.index(char)
  out += lookup2[(cur - prev) % 40]
  prev = cur

sys.stdout.write(out)

```

the actual conversion part is done in the for loop and begins with finding the index of the character from the ciphertext in lookup1, however lookup1 has no capital letters as are present in the text, so we negin be switching lookup 1 and 2.
```

for char in chars:
  cur = lookup1.index(char)
  out += lookup2[(cur - prev) % 40]
  prev = cur

```

It seemed like I was just reversing the program here so I switched the - for +, and tried switching around the assignments
```
for char in chars:
  cur = lookup1.index(char)
  new = lookup2[(cur + prev) % 40]
  out += new
  prev = lookup2.index(new)

```
When fed the ciphertext, it produced another python script with the tags python2 and self input
```
#asciiorder
#fortychars
#selfinput
#pythontwo

chars = ""
from fileinput import input
for line in input():
    chars += line
b = 1 / 1

for i in range(len(chars)):
    if i == b * b * b:
        print chars[i] #prints
        b += 1 / 1

```
I ran the following commands:

```
cat ciphertext | python3 convert.py >> c3.py
cat c3.py | python2 c3.py

```
and got the flag as the output

## Where I went wrong

- I made many mistakes while trying to reverse the encoder, going on many different tangents


# Custom Encryption 

**Flag:** picoCTF{custom_d2cr0pt6d_019c831c}

The challenge provided a python script that would encrypt a message in two steps. It has the encrypt function and the 
def dynamic_xor_encrypt function:
```
def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key*311)))
    return cipher

def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text

```

So I reveresed the two functions in steps to get these:
```
def decrypt(ciphertext,key):
  plaintext = ""
  for num in ciphertext:
   plaintext+=chr(int(num/key/311))
  return plaintext
  
def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    return cipher_text
```

the test function would generate a key based on two prime number p,q and two random numbers a,b. However, in the cipher text we were provided with a value for a and b. I then switched around the calling of the functions to get this:
```
def test():
    test_key = "trudeau"
    p = 97
    g = 31
    if not is_prime(p) and not is_prime(g):
        print("Enter prime numbers")
        return
    a = 88 #randint(p-10, p)
    b = 26 #randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    b_key = generator(u, b, p)
    shared_key = None
    if key == b_key:
        shared_key = key
    else:
        print("Invalid key")
        return
    
    cipher =  [97965, 185045, 740180, 946995, 1012305, 21770, 827260, 751065, 718410, 457170, 0, 903455, 228585, 54425, 740180, 0, 239470, 936110, 10885, 674870, 261240, 293895, 65310, 65310, 185045, 65310, 283010, 555135, 348320, 533365, 283010, 76195, 130620, 185045]
    semi_cipher = decrypt(cipher,shared_key)
    plain = dynamic_xor_decrypt(semi_cipher,test_key)
    print(f'plain is: {plain[::-1]}')
```
this provided the flag when run.

# Challenges face:
- made quite a few errors while reversing the program

# What I learned:
- the usage of __name__ == __main__ in python
