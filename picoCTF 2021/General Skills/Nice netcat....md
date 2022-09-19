```bash
nc mercury.picoctf.net 43239 > flag.txt
```

載下來的flag是ascii code，用以下的python script解碼

```python
# decode.py
with open("flag.txt", "r", encoding="utf-8") as f:
    flag = f.read()
    flag = flag.split("\n")
    flag = [chr(int(i)) for i in flag if i != ""]
    print("".join(flag))
```

可以得到flag

```console
$ python3 decode.py
picoCTF{g00d_k1tty!_n1c3_k1tty!_7c0821f5}
```
