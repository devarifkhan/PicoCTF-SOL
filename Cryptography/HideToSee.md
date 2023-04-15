# Cryptography
## HideToSee - 100 Points
## Question
> How about some hide and seek heh? Look at this image here.

## Solution
> This is an image and the flag is hidden inside this image.So we have to extract this flag.
> using steghide extract the hidden flie.
```python
┌──(kali㉿kali)-[~/Downloads]
└─$ steghide extract -sf atbash.jpg 
Enter passphrase: 
wrote extracted data to "encrypted.txt".
```
> The file shows krxlXGU{zgyzhs_xizxp_zx751vx6}
> This is a ciphertext. So we have to decode it now.
> We have a hint the file name is atbash and atbash is a cipher.
> we can use [decode](https://www.dcode.fr/atbash-cipher) atbash cipher to decrypt it.After decrypt your will get the flag.
> Flag is: picoCTF{atbash_crack_ac751ec6}
