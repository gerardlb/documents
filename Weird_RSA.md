The Data provided is labeled as RSA, and icludes information about c (coded text), q p, (two primary numbers). The clue here is data also includes dp and dq.

If we investigate the meaning of all the data, it seems that dp and dq are used in the Chinese Remainder Algorithm (used in many crypto libraries). 

```
dP = (1/e) mod (p-1)
dQ = (1/e) mod (q-1)
qInv = (1/q) mod p

m1 = c^dP mod p
m2 = c^dQ mod q
h = qInv * (m1 - m2) mod p
m = m2 + h * q

```
The difficult part is how to calculate qInv. In the following link there's information about it:
https://www.di-mgt.com.au/crt_rsa.html

You need to calculate modular inverse, and can be calculated using Euclidean Algorithm. 
In the link https://en.wikibooks.org/wiki/Algorithm_Implementation/Mathematics/Extended_Euclidean_algorithm you can find the information:

```python
# x = mulinv(b) mod n, (x * b) % n == 1
def mulinv(b, n):
    g, x, _ = egcd(b, n)
    if g == 1:
        return x % n

# return (g, x, y) a*x + b*y = gcd(x, y)
def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, x, y = egcd(b % a, a)
        return (g, y - (b // a) * x, x)
```

so the final steps are:

```python


qinv = modinv(q, p)
m1 = pow(c, dp, p)
m2 = pow(c, dq, q)
h = (qinv * (m1 - m2)) % p
m = m2 + h * q
print(m)
print(hex(m))

```







