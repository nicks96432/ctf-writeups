# information

[題目連結](https://play.picoctf.org/practice/challenge/186)

首先下載圖片

```bash
wget -q https://mercury.picoctf.net/static/b4d62f6e431dc8e563309ea8c33a06b3/cat.jpg
```

直接看圖片，是一隻貓在macbook上，除此之外並沒有什麼特別的地方，所以改朝metadata調查

使用`exiftool`查看圖片的metadata，archlinux需要安裝`perl-image-exiftool`

```console
$ exiftool cat.jpg
ExifTool Version Number         : 12.42
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2021:03:16 02:24:46+08:00
File Access Date/Time           : 2022:09:19 19:37:13+08:00
File Inode Change Date/Time     : 2022:09:19 19:37:11+08:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

可以找到License看起來像是base64編碼，解碼後得到flag

```console
$ echo cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9 | base64 -d -
picoCTF{the_m3tadata_1s_modified}
```
