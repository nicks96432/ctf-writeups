# Trivial Flag Transfer Protocol

[題目連結](https://play.picoctf.org/practice/challenge/103)

先下載檔案

```console
wget -q https://mercury.picoctf.net/static/4fe0f4357f7458c6892af394426eab55/tftp.pcapng
$ file tftp.pcapng
tftp.pcapng: pcapng capture file - version 1.0
```

用wireshark打開來看，都是TFTP在傳檔案，先用`File > Export Objects > TFTP...`把所有檔案載下來，如圖所示

![screenshot](Trivial%20Flag%20Transfer%20Protocol.png)

其中`instructions.txt`和`plan`都是看起來沒意義的大寫字母字串，先放著。
三張風景圖片也都看不出有flag的地方，不過最後的program.deb裡面放的是一個叫steghide的程式，
拿去餵狗發現是一種可以處裡圖片和音檔隱寫的程式。試試看從三張圖片中取得隱寫的資料，但沒有密碼所以失敗。

回來看這兩個文字檔案，base64解不出啥，試試rot13沒想到就有了

```console
$ py rot13.py < instructions.txt
TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN
$ py rot13.py < plan
IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
```

稍微加個空格，換個小寫

```text
TFTP doesn't encrypt our traffic so we must disguise our flag transfer. Figure out way to hide the flag and I will check back for the plan
I used the program and hid it with-DUEDILIGENCE. Check out the photos
```

試試看DUEDILIGENCE當作密碼，結果在第三個檔案成功得到flag，一開始還懷疑在第二個檔案，因為它超大一個，有25MB

```console
$ steghide extract -sf picture3.bmp -p DUEDILIGENCE
wrote extracted data to "flag.txt".
$ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```
