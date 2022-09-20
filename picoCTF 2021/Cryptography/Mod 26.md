# Mod 26

[題目連結](https://play.picoctf.org/practice/challenge/144)

利用以下的python script解碼

```python
# rot13.py
import codecs
import sys

for line in sys.stdin:
    print(codecs.decode(line, "rot_13"))
```

最後就可以得到flag

```console
$ echo "cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_MAZyqFQj}" | py rot13.py
picoCTF{next_time_I'll_try_2_rounds_of_rot13_ZNMldSDw}
```
