# keygenme-py

[題目連結](https://play.picoctf.org/practice/challenge/121)

第一次看到題目用python寫的，有點意思。(如果前面那題不算的話)

在`menu_trial() -> enter_license() -> check_key()`是判斷flag的邏輯，
可以看到前面的部分是固定的，最後的8碼是`PRITCHARD`經過sha256後的第
5, 6, 4, 7, 3, 8, 2, 9碼，也就是`54ef6292`。

```console
printf PRITCHARD | sha256sum 
496e54f222f256b023f33cdda0270853f39d7bf24fa1ca6b72d4b4fd1a9cae56  -
```

全部組合在一起就可以得到flag是`picoCTF{1n_7h3_|<3y_of_54ef6292}`。
