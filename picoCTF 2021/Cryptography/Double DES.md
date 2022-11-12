# Double DES

[題目連結](https://play.picoctf.org/practice/challenge/140)

這題可以先傳一個明文，然後把回傳的密文記下來，再用meet in the middle的方式破解。

```python
import binascii

from Crypto.Cipher import DES
from pwn import context, remote

context.log_level = "error"
payload = "A" * 7


def pad(msg: str):
    block_len = 8
    over = len(msg) % block_len
    pad_count = block_len - over
    return (msg + " " * pad_count).encode("utf-8")


with remote("mercury.picoctf.net", 33425) as r:
    r.recvline()
    flag = bytes.fromhex(r.recvline().decode("utf-8"))
    r.sendline(binascii.hexlify(payload.encode("utf-8")))
    payload_cipher = bytes.fromhex(r.recvline().decode("utf-8").split()[-1])

mitm_map: dict[bytes, bytes] = {}

for i in range(1000000):
    key2 = pad(f"{i:06d}")
    des = DES.new(key2, DES.MODE_ECB)
    mitm_map[des.decrypt(payload_cipher)] = key2

for i in range(1000000):
    key1 = pad(f"{i:06d}")
    des = DES.new(key1, DES.MODE_ECB)
    encrypted = des.encrypt(pad(payload))
    key2 = mitm_map.get(encrypted)
    if key2 is not None:
        break

des1 = DES.new(key1, DES.MODE_ECB)
des2 = DES.new(key2, DES.MODE_ECB)
# af5fa5d565081bac320f42feaf69b405
print(des1.decrypt(des2.decrypt(flag)).decode("utf-8").rstrip())
```
