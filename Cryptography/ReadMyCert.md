# Cryptography
## ReadMyCert - 100 Points
## Question
> How about we take you on an adventure on exploring certificate signing requests Take a look at this CSR file here.

## Solution
> Download the file it is a CSR file.
> use this command to read
```python
openssl req -in readmycert.csr -text -noout  
```
> Now you can see the flag in subject attribute

> Flag is: picoCTF{read_mycert_57f58832}
