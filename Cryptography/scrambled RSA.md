# Cryptography
## Scrambled: RSA - 140 Points
## Question
> Hmmm I wonder if you have learned your lesson... Let's see if you understand RSA and how the encryption works. Connect with nc mercury.picoctf.net 4484.

## Solution
> I don't know why this question is related to RSA, so I directly find a way to find an understanding of the law of psychic observation.

> The first is to allow it to encrypt a string of length 1, and it can be found that its ciphertext is fixed. After encrypting a two-character string, if you find > that it has only two kinds of ciphertext, after careful observation, you can find that both ciphertexts have only one character ciphertext, so after removing that paragraph, the remaining ciphertext should be, but it is not only encrypted one-character ciphertext.ababb

> I speculated that it probably had something to do with the prefix, so I continued to do some testing and found that it matched my guess, but the order was completely random. In this way, my approach is to use a dictionary to save the correspondence between characters and ciphertext, and then brute-force the flag one by one.


> Flag is: picoCTF{bad_1d3a5_5700361}