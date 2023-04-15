# Cryptography
## Play Nice - 110 Points
## Question
> Not all ancient ciphers were so bad... The flag is not in standard format. nc mercury.picoctf.net 19860 playfair.py

## Solution
> The source code mention that the square size is 6
> Using [playfair decode](https://www.dcode.fr/playfair-cipher) we will decode the cipher.Make sure your resize is 6x6.
```python
Here is the alphabet: lsi28c14ot0vbf7nagh3mpjuxy5kwz6edqr9
Here is the encrypted message: 1x5hqlod8x7oa88h0de1b5r6xja5sd
```
> Paste the alphabet in your playfair grid box.
> Paste the cipher text.
> This will decrypt and your will get LHXM62I5LWOI0RLJOR647XQ9WH7WIE this.
> using python make it lower.
```python
┌──(kali㉿kali)-[~/Downloads/PicoCTF-SOL]
└─$ python
Python 3.11.2 (main, Mar 13 2023, 12:18:29) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 'LHXM62I5LWOI0RLJOR647XQ9WH7WIE'.lower()
'lhxm62i5lwoi0rljor647xq9wh7wie'
```

> Now paste this and you will get the flag.
```python
┌──(kali㉿kali)-[~/Downloads]
└─$ nc mercury.picoctf.net 19860
Here is the alphabet: lsi28c14ot0vbf7nagh3mpjuxy5kwz6edqr9
Here is the encrypted message: 1x5hqlod8x7oa88h0de1b5r6xja5sd
What is the plaintext message? lhxm62i5lwoi0rljor647xq9wh7wie
Congratulations! Here's the flag: f391b621282ef5063ab2de93ab9e4bff

```

> Flag is: 	f391b621282ef5063ab2de93ab9e4bff Don't use picoCTF{} here.
