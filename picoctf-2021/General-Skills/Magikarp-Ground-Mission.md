首先連線到指定的container

```bash
ssh ctf-player@venus.picoctf.net -p 56962
```

稍微勘查一下地形，會發現兩個檔案

```console
$ ls
1of3.flag.txt  instructions-to-2of3.txt
$ cat 1of3.flag.txt
picoCTF{xxsh_
$ cat instructions-to-2of3.txt
Next, go to the root of all things, more succinctly `/`
```

照著指示去`/`，會發現又有兩個檔案

```console
$ cd /
$ ls
2of3.flag.txt  boot  etc   instructions-to-3of3.txt  lib64  mnt  proc  run   srv  tmp  var
bin            dev   home  lib                       media  opt  root  sbin  sys  usr
cat 2of3.flag.txt
0ut_0f_\/\/4t3r_
cat instructions-to-3of3.txt
Lastly, ctf-player, go home... more succinctly `~`
```

再次照者指示去`~`，會發現flag最後一部分，那個drop-in是最一開始在的資料夾

```console
$ cd
$ ls
3of3.flag.txt  drop-in
cat 3of3.flag.txt
5190b070}
```

三個部分組合在一起，可得flag就是`picoCTF{xxsh_0ut_0f_\/\/4t3r_5190b070}`
