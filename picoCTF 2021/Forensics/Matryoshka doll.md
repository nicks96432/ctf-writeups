# Matryoshka doll

[題目連結](https://play.picoctf.org/practice/challenge/129)

首先下載圖片

```bash
wget -q https://mercury.picoctf.net/static/5ef2e9103d55972d975437f68175b9ab/dolls.jpg
```

直接看圖片，發現是一個俄羅斯娃娃，除此之外沒有什麼特別的地方

因為題目說這個檔案裡面包很多東西，所以我們可以用`binwalk`來看看

```console
$ binwalk -e dolls.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378954, uncompressed size: 383940, name: base_images/2_c.jpg
651612        0x9F15C         End of Zip archive, footer length: 22
```

`-e`選項代表自動把已知的檔案類型拆除來

可以發現它裡面還包著另一張俄羅斯娃娃圖片，至於那個zip檔的內容就是png的內容，所以可以忽略。
一樣用`binwalk`來看看裡面有什麼

```console
$ binwalk -e 2_c.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 526 x 1106, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
187707        0x2DD3B         Zip archive data, at least v2.0 to extract, compressed size: 196045, uncompressed size: 201447, name: base_images/3_c.jpg
383807        0x5DB3F         End of Zip archive, footer length: 22
383918        0x5DBAE         End of Zip archive, footer length: 22
```

又有一張圖片，再用一次`binwalk`

```console
$ binwalk -e 3_c.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 428 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
123606        0x1E2D6         Zip archive data, at least v2.0 to extract, compressed size: 77653, uncompressed size: 79808, name: base_images/4_c.jpg
201425        0x312D1         End of Zip archive, footer length: 22
```

最後一張圖片，再用一次`binwalk`，可以得到一個`flag.txt`

```console
$ binwalk -e 4_c.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 320 x 768, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
79578         0x136DA         Zip archive data, at least v2.0 to extract, compressed size: 64, uncompressed size: 81, name: flag.txt
79786         0x137AA         End of Zip archive, footer length: 22
```

`flag.txt`裡面就是flag

```console
$ cat flag.txt
picoCTF{e3f378fe6c1ea7f6bc5ac2c3d6801c1f}
```
