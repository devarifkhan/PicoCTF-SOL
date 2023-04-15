# Cryptography
## basic-mod2 - 100 Points
## Question
> A new modular challenge! Download the message here. Take each number mod 41 and find the modular inverse for the result. Then map to the following character set: 1-26 are the alphabet, 27-36 are the decimal digits, and 37 is an underscore. Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})

## Solution
> I hope no need to explain.
```python
import string
alphabet = string.ascii_lowercase
alphabet += "0123456789_"
flag_enc = [104, 85, 69, 354, 344, 50, 149, 65, 187, 420, 77, 127, 385, 318, 133, 72, 206, 236, 206, 83, 342, 206, 370]
flag = ""
for c in flag_enc: 
    pos = pow(c, -1, 41)
    flag += alphabet[pos-1]
print(flag)
```

> Flag is: picoCTF{1nv3r53ly_h4rd_dadaacaa}
