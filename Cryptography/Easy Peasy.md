# Cryptography
## Easy Peasy - 40 Points
## Question
> A one-time pad is unbreakable, but can you manage to recover the flag? (Wrap with picoCTF{}).Source code of otp.py
```text
#!/usr/bin/python3 -u
import os.path

KEY_FILE = "key"
KEY_LEN = 50000
FLAG_FILE = "flag"


def startup(key_location):
        flag = open(FLAG_FILE).read()
        kf = open(KEY_FILE, "rb").read()

        start = key_location
        stop = key_location + len(flag)

        key = kf[start:stop]
        key_location = stop

        result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), flag, key))
        print("This is the encrypted flag!\n{}\n".format("".join(result)))

        return key_location

def encrypt(key_location):
        ui = input("What data would you like to encrypt? ").rstrip()
        if len(ui) == 0 or len(ui) > KEY_LEN:
                return -1

        start = key_location
        stop = key_location + len(ui)

        kf = open(KEY_FILE, "rb").read()

        if stop >= KEY_LEN:
                stop = stop % KEY_LEN
                key = kf[start:] + kf[:stop]
        else:
                key = kf[start:stop]
        key_location = stop

        result = list(map(lambda p, k: "{:02x}".format(ord(p) ^ k), ui, key))

        print("Here ya go!\n{}\n".format("".join(result)))

        return key_location


print("******************Welcome to our OTP implementation!******************")
c = startup(0)
while c >= 0:
        c = encrypt(c)

```
## Solution
when we connect to the service we get this.
```text
┌──(kali㉿kali)-[~/Downloads]
└─$ nc mercury.picoctf.net 36981
******************Welcome to our OTP implementation!******************
This is the encrypted flag!
5541103a246e415e036c4c5f0e3d415a513e4a560050644859536b4f57003d4c

What data would you like to encrypt? abcdef
Here ya go!
50035d381d04

What data would you like to encrypt? 


What data would you like to encrypt? 
```

> From the service implementation, we see that it uses a XOR pad of length 50000 to encrypt the input. This should be unbreakable if it's used as a one-time-pad, but in our case the service performs a wrap-around and reuses the same pad for every 50000 characters.
> Here we are taking the encrypted text checking its length it is 32 character long
```text
┌──(kali㉿kali)-[~/Downloads]
└─$ python                                 
Python 3.11.2 (main, Mar 13 2023, 12:18:29) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> len('5541103a246e415e036c4c5f0e3d415a513e4a560050644859536b4f57003d4c')/2
32.0
>>> 
```
> now we will generate the padding and pipe with nc mercury.picoctf.net 36981
```text
┌──(kali㉿kali)-[~/Downloads]
└─$ python -c "print('a'*49968);print('a'*32)" |nc mercury.picoctf.net 36981  
******************Welcome to our OTP implementation!******************
This is the encrypted flag!
5541103a246e415e036c4c5f0e3d415a513e4a560050644859536b4f57003d4c

What data would you like to encrypt? Here ya go!
50005f3d1903553d1958560a543436333d1902513d1903503d1905553d1950553d1904553d1904573d1905523d1907523b1652563d190407333a3d195004453d190758533d1907574e3d1902043d1956073d1900503d1902073d1905533c333d1950501c543d190504343d1904573d1958073d1904021c3d1907553d19025255013d1905583d1905553d190733d19505.......
What data would you like to encrypt? Here ya go!
0346483f243d1959563d1907563d1903543d190551023d1959073d1902573d19

What data would you like to encrypt? ^Z
```
> Now we can xor encrypted flag,encrypted a and Plaintext of a.

```text
┌──(kali㉿kali)-[~/Downloads]
└─$ python
Python 3.11.2 (main, Mar 13 2023, 12:18:29) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> pa=0x6161616161616161616161616161616161616161616161616161616161616161
>>> ef=0x5541103a246e415e036c4c5f0e3d415a513e4a560050644859536b4f57003d4c
>>> ea=0x0346483f243d1959563d1907563d1903543d190551023d1959073d1902573d19
>>> pa^ef^ea
25057821178459433675179303676199845791448110008844411929611525408183085850932
>>> '{:x}'.format(pa^ef^ea)
'3766396461323966343034393961393864623232303338306135373734366134'
```
> now visit https://www.rapidtables.com/convert/number/hex-to-ascii.html paste 3766396461323966343034393961393864623232303338306135373734366134 contert it to ascii

> Flag is: picoCTF{7f9da29f40499a98db220380a57746a4}
