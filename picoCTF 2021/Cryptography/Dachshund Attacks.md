# Dachshund Attacks

[題目連結](https://play.picoctf.org/practice/challenge/159)

因為d很小，所以可以用wiener's attack

```python
from pwn import context, remote
from sage.all import Rational, power_mod, solve, var

context(log_level="error")

with remote("mercury.picoctf.net", 36463) as r:
    r.recvline()
    e = Rational(int(r.recvline().split()[1].decode("utf-8")))
    N = Rational(int(r.recvline().split()[1].decode("utf-8")))
    c = Rational(int(r.recvline().split()[1].decode("utf-8")))

convergents = (e / N).continued_fraction().convergents()

for convergent in convergents:
    if convergent == 0:
        continue

    k, d = convergent.numerator(), convergent.denominator()

    if (e * d - 1) % k == 0:
        phi = (e * d - 1) / k
        b = N - phi + 1
        D = b**2 - 4 * N

        if D.is_square():
            d = power_mod(e, -1, phi)
            m = power_mod(c, d, N)
            # picoCTF{proving_wiener_2635457}
            print(bytes.fromhex(hex(m)[2:]).decode("utf-8"))
            break
```
