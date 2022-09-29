# Disk, disk, sleuth!

[題目連結](https://play.picoctf.org/practice/challenge/113)

先下載檔案

```console
$ wget -q https://mercury.picoctf.net/static/f63e4eba644c99e92324b65cbd875db6/dds1-alpine.flag.img.gz
$ file dds1-alpine.flag.img.gz
dds1-alpine.flag.img.gz: gzip compressed data, was "dds1-alpine.flag.img", last modified: Tue Mar 16 00:19:51 2021, from Unix, original size modulo 2^32 134217728
$ gunzip dds1-alpine.flag.img.gz
$ file dds1-alpine.flag.img
dds1-alpine.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0x10,81,1), startsector 2048, 260096 sectors
```

這個應該是一個映像檔，用 `fdisk` 看一下

```console
$ fdisk -l dds1-alpine.flag.img
Disk dds1-alpine.flag.img: 128 MiB, 134217728 bytes, 262144 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x5d8b75fc

Device                Boot Start    End Sectors  Size Id Type
dds1-alpine.flag.img1 *     2048 262143  260096  127M 83 Linux
```

依照題目的提示，下載`sleuthkit`並使用`srch_strings`指令，沒想到就直接有flag了

```console
$ srch_strings dds1-alpine.flag.img | grep picoCTF
  SAY picoCTF{f0r3ns1c4t0r_n30phyt3_ad5c96c0}
```

不過其實也不用下載`sleuthkit`，直接用`strings`、`grep`，或是那些有尋找字串功能的編輯器也可以
