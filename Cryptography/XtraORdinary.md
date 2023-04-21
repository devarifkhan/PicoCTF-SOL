# Cryptography
## XtraORdinary - 150 Points
## Question
> Check out my new, never-before-seen method of encryption! I totally invented it myself. I added so many for loops that I don't even know what it does. It's extraordinarily secure!
CHALLENGE ENDPOINTS
Download output.txt	output.txt
Download encrypt.py	encrypt.py

## Solution
> The output file shows: 57657535570c1e1c612b3468106a18492140662d2f5967442a2960684d28017931617b1f3637

> encrypt.py shows:

```python
#!/usr/bin/env python3

from random import randint

with open('flag.txt', 'rb') as f:
    flag = f.read()

with open('secret-key.txt', 'rb') as f:
    key = f.read()

def encrypt(ptxt, key):
    ctxt = b''
    for i in range(len(ptxt)):
        a = ptxt[i]
        b = key[i % len(key)]
        ctxt += bytes([a ^ b])
    return ctxt

ctxt = encrypt(flag, key)

random_strs = [
    b'my encryption method',
    b'is absolutely impenetrable',
    b'and you will never',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'break it'
]

for random_str in random_strs:
    for i in range(randint(0, pow(2, 8))):
        for j in range(randint(0, pow(2, 6))):
            for k in range(randint(0, pow(2, 4))):
                for l in range(randint(0, pow(2, 2))):
                    for m in range(randint(0, pow(2, 0))):
                        ctxt = encrypt(ctxt, random_str)

with open('output.txt', 'w') as f:
    f.write(ctxt.hex())
```
> At first we will edit the code and see how it works.

```python
#!/usr/bin/env python3

from random import randint

with open('flag.txt', 'rb') as f:
    flag = f.read()

with open('secret-key.txt', 'rb') as f:
    key = f.read()

def encrypt(ptxt, key):
    ctxt = b''
    for i in range(len(ptxt)):
        a = ptxt[i]
        b = key[i % len(key)]
        ctxt += bytes([a ^ b])
    return ctxt

ctxt = encrypt(flag, key)

random_strs = [
    b'my encryption method',
    b'is absolutely impenetrable',
    b'and you will never',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'break it'
]

for random_str in random_strs:
    for i in range(randint(0, pow(2, 8))):
        for j in range(randint(0, pow(2, 6))):
            for k in range(randint(0, pow(2, 4))):
                for l in range(randint(0, pow(2, 2))):
                    for m in range(randint(0, pow(2, 0))):
                        ctxt = encrypt(ctxt, random_str)

with open('output.txt', 'w') as f:
    f.write(ctxt.hex())
```
> The code output shows: ctxt.hex = 132a7f2b3d
Here it is found that the key encrypt() function is simply to XOR encrypt 2 sets of strings

> The characteristics  of  xor is if it's 11,00. The same string does XOR "even times", it is equivalent to not doing it.

> Now we need to find out the key.There are basically 10 random strings.So if i remove dulplicate there will be 5 to reduce the computational cost.

    A = 'my encryption method'
    B = 'is absolutely impenetrable'
    C = 'and you will never'
    D = 'ever'
    E = 'break it'

> So there can be 2^5 =32 combinations of XOR.
> The hardest part here is that we don't know the key, but we know that the prefix of the flag should be , so we can exhaust the 32 keys under this premise.picoCTF{

```python
from random import randint
import itertools
def encrypt(ptxt, key):
    ctxt = b''
    for i in range(len(ptxt)):
        a = ptxt[i]
        b = key[i % len(key)]
        ctxt += bytes([a ^ b])
    return ctxt

key = b''
random_strs = [
    b'my encryption method',
    b'is absolutely impenetrable',
    b'and you will never',
    b'ever',
    b'break it'
]

flag_prefix = b'picoCTF{'
ctxt = '57657535570c1e1c612b3468106a18492140662d2f5967442a2960684d28017931617b1f3637'

for i in range(0,6):
    c = bytes.fromhex(ctxt)
    new_string = list(itertools.combinations(random_strs, i))
    for sub_new_string in new_string:
        for word in sub_new_string:
            c = encrypt(c, word)

        key = encrypt(c, flag_prefix)
        key = key[:len(flag_prefix)].decode()
        if key.isprintable():
            print(key)
```
> Here I used the key.isprintable() function to reduce the range of keys to 7

    Bhr~a'0R
    Elrmoq<T
    Bhr~a'0R
    Elrmoq<T
    Bhr~a'0R
    Africa!A
    Elrmoq<T

>In fact, this question is a bit confusing, because this key must meet 2 prerequisites, the first is that the flag must have this prefix, and the second is that the length of the key must be less than the length of this prefix.picoCTF{picoCTF{
Then look at the above 7 possible keys, most likely, in addition to being a meaningful word, there is also its final A repeated, indicating that the key is likely to be a loop.Africa!AAfrica!

> So the final code will be
```python
from random import randint
import itertools
def encrypt(ptxt, key):
    ctxt = b''
    for i in range(len(ptxt)):
        a = ptxt[i]
        b = key[i % len(key)]
        ctxt += bytes([a ^ b])
    return ctxt

key = b''
random_strs = [
    b'my encryption method',
    b'is absolutely impenetrable',
    b'and you will never',
    b'ever',
    b'break it'
]

key = b'Africa!'
ctxt = '57657535570c1e1c612b3468106a18492140662d2f5967442a2960684d28017931617b1f3637'

for i in range(0,6):
    c = bytes.fromhex(ctxt)
    new_string = list(itertools.combinations(random_strs, i))
    for sub_new_string in new_string:
        for word in sub_new_string:
            c = encrypt(c, word)

        flag = encrypt(c, key)
        flag = flag.decode()
        if flag.isprintable():
            print(flag)
```
> We got tcckOD[nr4=wHs4W5Re0}e5X3hm9>aTd1>'..y
tcckOD[nr4=wHs4W5Re0}e5X3hm9>aTd1>'..y
picoCTF{w41t_s0_1_d1dnt_1nv3nt_x0r???}
tcckOD[nr4=wHs4W5Re0}e5X3hm9>aTd1>'..y

> Flag is: picoCTF{w41t_s0_1_d1dnt_1nv3nt_x0r???}