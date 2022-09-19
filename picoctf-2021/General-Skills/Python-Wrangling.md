下載三個檔案之後按照說明解碼即可得到flag

```shell
$ wget -q https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.key
$ wget -q https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/pw.txt
$ wget -q https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/flag.txt.en
$ python ./ende.py -d flag.txt.en $(cat pw.txt)
picoCTF{4p0110_1n_7h3_h0us3_aa821c16}
```
