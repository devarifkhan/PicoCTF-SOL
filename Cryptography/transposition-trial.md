# Cryptography
## transposition-trial - 100 Points
## Question
> Our data got corrupted on the way here. Luckily, nothing got replaced, but every block of 3 got scrambled around! The first word seems to be three letters long, maybe you can use that to recover the rest of the message. Download the corrupted message here.

## Solution
> The message shows : heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_V091B0AE}2
> To be honest, this challenge needs a bit of an inspiration, but if you replace spaces with underscores (_) and look carefully, you can see the encrypting rules.
```text
"heT" : "The"
"fl_" : "_fl"
"g_a" : "ag_"
"s_i" : "is_"
"icp" : "pic"
"CTo" : "oCT"
```

## Solution
```python
def main():
    txt = 'heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_V091B0AE}2'
    n=3
    txt3gram = [txt[i:i+n] for i in range(0, len(txt), n)]
    decode_lst = []
    for i in range(len(txt3gram)):
        decode_lst.append(txt3gram[i][2]+txt3gram[i][0]+txt3gram[i][1])
    print(''.join(decode_lst))
if __name__ == '__main__':
    main()
```
## Output
```python
[Running] python -u "/home/kali/Downloads/PicoCTF-SOL/Cryptography/sol.py"
The flag is picoCTF{7R4N5P051N6_15_3XP3N51V3_109AB02E}

```

> Flag is: picoCTF{7R4N5P051N6_15_3XP3N51V3_109AB02E}
