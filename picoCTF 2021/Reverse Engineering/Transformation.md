# Transformation

[題目連結](https://play.picoctf.org/practice/challenge/104)

```console
$ file enc
enc: Unicode text, UTF-8 text, with no line terminators
```

照著題目敘述反過來做就好了。

```python
with open("enc", "r", encoding="utf-8") as f:
    flag = f.readline()

    # picoCTF{16_bits_inst34d_of_8_04c0760d}
    for c in flag:
        print(chr(ord(c) >> 8), end="")
        print(chr(ord(c) & 0xff), end="")
    print()
```
