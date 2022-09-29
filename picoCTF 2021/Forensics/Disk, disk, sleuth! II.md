# Disk, disk, sleuth! II

[題目連結](https://play.picoctf.org/practice/challenge/137)

先下載檔案

```console
$ wget -q https://mercury.picoctf.net/static/544be9762e9f9c0adcbeb7bcf27f49a2/dds2-alpine.flag.img.gz
$ file dds2-alpine.flag.img.gz
dds2-alpine.flag.img.gz: gzip compressed data, was "dds2-alpine.flag.img", last modified: Tue Mar 16 00:28:15 2021, from Unix, original size modulo 2^32 134217728
$ gunzip dds2-alpine.flag.img.gz
$ file dds2-alpine.flag.img
dds2-alpine.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0x10,81,1), startsector 2048, 260096 sectors
```

這個應該是一個映像檔，用 `fdisk` 看一下，和[Disk, disk, sleuth!](Disk,%20disk,%20sleuth!.md)一樣

```console
$ fdisk -l dds2-alpine.flag.img
Disk dds2-alpine.flag.img: 128 MiB, 134217728 bytes, 262144 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x5d8b75fc

Device                Boot Start    End Sectors  Size Id Type
dds2-alpine.flag.img1 *     2048 262143  260096  127M 83 Linux
```

依照題目的提示，下載`sleuthkit`並使用`tsk_gettimes`指令尋找`down-at-the-bottom.txt`

```console
$ tsk_gettimes dds2-alpine.flag.img | grep down-at-the-bottom.txt
0|vol2/root/down-at-the-bottom.txt|18291|r/rrw-r--r--|0|0|900|1613499680|1613499680|1613499680|0
```

找到它的位置後，使用`tsk_recover`指令將它復原出來，就可以得到flag了，注意2048的offset

```console
$ tsk_recover -o 2048 -a dds2-alpine.flag.img recover
Files Recovered: 1566
$ cat recover/root/down-at-the-bottom.txt
   _     _     _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( p ) ( i ) ( c ) ( o ) ( C ) ( T ) ( F ) ( { ) ( f ) ( 0 ) ( r ) ( 3 ) ( n )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
   _     _     _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( s ) ( 1 ) ( c ) ( 4 ) ( t ) ( 0 ) ( r ) ( _ ) ( n ) ( 0 ) ( v ) ( 1 ) ( c )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
   _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( 3 ) ( _ ) ( 6 ) ( 9 ) ( a ) ( b ) ( 1 ) ( d ) ( c ) ( 8 ) ( } )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
```

不過其實也不用下載`sleuthkit`，直接用`7z`也可以

```console
$ 7z l dds2-alpine.flag.img | grep down-at-the-bottom.txt
2021-02-17 02:21:20 .....          900         1024  root/down-at-the-bottom.txt
$ 7z e dds2-alpine.flag.img -o. root/down-at-the-bottom.txt >/dev/null
$ cat down-at-the-bottom.txt
   _     _     _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( p ) ( i ) ( c ) ( o ) ( C ) ( T ) ( F ) ( { ) ( f ) ( 0 ) ( r ) ( 3 ) ( n )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
   _     _     _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( s ) ( 1 ) ( c ) ( 4 ) ( t ) ( 0 ) ( r ) ( _ ) ( n ) ( 0 ) ( v ) ( 1 ) ( c )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
   _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( 3 ) ( _ ) ( 6 ) ( 9 ) ( a ) ( b ) ( 1 ) ( d ) ( c ) ( 8 ) ( } )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
```
