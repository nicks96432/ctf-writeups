# Milkslap

[題目連結](https://play.picoctf.org/practice/challenge/139)

這題的題目是一個網頁，裡面的圖片會隨著游標左右移動播放，是一個人被牛奶打巴掌的連續圖片。
稍微研究一下，發現是根據游標位置改變`background-position-y`，所以原來的圖片是一長串圖片接在一起。

那就先把圖片載下來研究

```console
$ wget -q http://mercury.picoctf.net:7585/concat_v.png
$ file concat_v.png
concat_v.png: PNG image data, 1280 x 47520, 8-bit/color RGB, non-interlaced
$ binwalk -e concat_v.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
41            0x29            Zlib compressed data, default compression
3210141       0x30FB9D        MySQL ISAM compressed data file Version 2
```

在網路上找到`zsteg`可以對png圖片進行隱寫資訊的檢查，所以先用`zsteg`看看，不過我直接安裝不知道為啥他都跑不出來，所以我用docker環境安裝。

```dockerfile
FROM ruby:3
RUN gem install zsteg
ENTRYPOINT [ "zsteg" ]
```

然後就可以找到flag了

```console
$ docker build -t zsteg .
$ docker run -it --rm -v $(pwd):/data zsteg /data/concat_v.png
imagedata           .. text: "\n\n\n\n\n\n\t\t"
b1,b,lsb,xy         .. text: "picoCTF{imag3_m4n1pul4t10n_sl4p5}\n"
b1,bgr,lsb,xy       .. <wbStego size=9706075, data="\xB6\xAD\xB6}\xDB\xB2lR\x7F\xDF\x86\xB7c\xFC\xFF\xBF\x02Zr\x8E\xE2Z\x12\xD8q\xE5&MJ-X:\xB5\xBF\xF7\x7F\xDB\xDFI\bm\xDB\xDB\x80m\x00\x00\x00\xB6m\xDB\xDB\xB6\x00\x00\x00\xB6\xB6\x00m\xDB\x12\x12m\xDB\xDB\x00\x00\x00\x00\x00\xB6m\xDB\x00\xB6\x00\x00\x00\xDB\xB6mm\xDB\xB6\xB6\x00\x00\x00\x00\x00m\xDB", even=true, mix=true, controlbyte="[">
b2,r,lsb,xy         .. file: SoftQuad DESC or font file binary
b2,r,msb,xy         .. file: VISX image file
b2,g,lsb,xy         .. file: VISX image file
b2,g,msb,xy         .. file: SoftQuad DESC or font file binary - version 15722
b2,b,msb,xy         .. text: "UfUUUU@UUU"
b4,r,lsb,xy         .. text: "\"\"\"\"\"#4D"
b4,r,msb,xy         .. text: "wwww3333"
b4,g,lsb,xy         .. text: "wewwwwvUS"
b4,g,msb,xy         .. text: "\"\"\"\"DDDD"
b4,b,lsb,xy         .. text: "vdUeVwweDFw"
b4,b,msb,xy         .. text: "UUYYUUUUUUUU"
```
