# Easy Peasy

[題目連結](https://play.picoctf.org/practice/challenge/125)

因為加密用的是xor，key長度是50000一循環，所以可以傳`50000 - len(key)`個字元，
然後再把xor過的key傳回去，就可以得到flag

```python
from pwn import context, remote

context.log_level = "error"

with remote("mercury.picoctf.net", 64260) as r:
    r.recvlines(2)
    encoded_flag = bytes.fromhex(r.recvline().decode("utf-8"))
    payload = b"A" * (50000 - len(encoded_flag))
    r.recvline()
    r.sendline(payload)
    r.recvlines(2)
    r.sendline(encoded_flag)
    r.recvlines(2)
    
    # picoCTF{3a16944dad432717ccc3945d3d96421a}
    print(f"picoCTF{{{bytes.fromhex(r.recvline().decode('utf-8')).decode('utf-8')}}}")
```
