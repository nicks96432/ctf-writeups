# No Padding, No Problem

[題目連結](https://play.picoctf.org/practice/challenge/154)

這題的oracle可以解密除了flag以外的所有密文，所以很明顯的可以用chosen cipertext attack來解。

```python
from pwn import context, remote
from sage.all import mod

context.log_level = "error"

with remote("mercury.picoctf.net", 42248) as r:
    r.recvlines(4)
    N = int(r.recvline().split()[1].decode("utf-8"))
    e = int(r.recvline().split()[1].decode("utf-8"))

    c = mod(int(r.recvline().split()[1].decode("utf-8")), N)
    r.sendline(str(c * mod(2**e, N)).encode("utf-8"))
    r.recvlines(2)

    line = r.recvline().decode("utf-8")
    m = int(line.split()[-1]) // 2
    # picoCTF{m4yb3_Th0se_m3s54g3s_4r3_difurrent_7416022}
    print(bytes.fromhex(hex(int(m))[2:]).decode("utf-8"))
```
