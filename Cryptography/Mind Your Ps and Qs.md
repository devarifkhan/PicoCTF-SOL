# Cryptography
## Mind your Ps and Qs - 20 Points
## Question
> In RSA, a small e value can be problematic, but what about N? Can you decrypt this?
> After download the values file. cat values shows it.

```text
Decrypt my super sick RSA:
c: 861270243527190895777142537838333832920579264010533029282104230006461420086153423
n: 1311097532562595991877980619849724606784164430105441327897358800116889057763413423
e: 65537  
```

## Solution
> C is the ciphertext we wish to decode. N is the result of multiplying two prime numbers p and q, ie. n = p * q. E is the multiplicative inverse of a private exponent d modulo phi. Phi is equal to (p-1)*(q-1).Now we need p and q.

> visit http://factordb.com/index.php?query=1422450808944701344261903748621562998784243662042303391362692043823716783771691667 and your will get p and q.So we can write.
n = 2159947535959146091116171018558446546179 * 658558036833541874645521278345168572231473 
```python
from Crypto.Util.number import inverse,long_to_bytes

c = 843044897663847841476319711639772861390329326681532977209935413827620909782846667
n = 1422450808944701344261903748621562998784243662042303391362692043823716783771691667
e = 65537
p = 2159947535959146091116171018558446546179
q = 658558036833541874645521278345168572231473

phi= (p-1)* (q-1)

d=inverse(e, phi)
m=pow(c,d,n)
print(long_to_bytes(m))
```
> picoCTF{sma11_N_n0_g0od_00264570}
