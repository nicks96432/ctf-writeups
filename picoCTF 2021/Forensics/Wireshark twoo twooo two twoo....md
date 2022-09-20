# Wireshark twoo twooo two twoo...

[題目連結](https://play.picoctf.org/practice/challenge/110)

先下載檔案

```console
$ wget -q https://mercury.picoctf.net/static/a7b4ce62a4f4313a6e5b0b03b97be953/shark2.pcapng
$ file shark2.pcapng
shark2.pcapng: pcapng capture file - version 1.0
```

用wireshark打開，發現有很多`/flag`的HTTP request，但response都是`picoCTF{random hex string}`，
看起來就不像flag。

不過在DNS的部份看到可疑的地方，這些query的域名開頭就像是base64

![screenshot](Wireshark%20twoo%20twooo%20two%20twoo....webp)

用`File > Export Packet Dissections > As Plain Text...`輸出封包，不要勾Packet details和
Include column headings，然後再用`sed`處理，把base64字串組合在一起。

```cosnole
$ sed 's/^.*A //; s/\..*$//' packets.txt | tr -d '\n'
�:�G^��8tu
          ������8��\���U x�w�^�->y:����2�%���#�����M5���m�X���p�갔v/3�o�W��04òm�%�Ѐ��[ �1[_�Qj��u�����l]���;��fI��picoCT�m$���J�w�}g�Id|��Qx<���kOm������٭�M�'@:���(��`vE�-f2��6�p�8����K��?y m�$�SO���!*�b��H+
   n�����% �xǑԂ&�:���E�+�i�1�<�Tqebͧ,+����>�Q;�F�u2�2�$�GuDU��ח6ML��a����΂�g�ml
                                                                              ZbI��i�93�ay�{�����l)kc�T�_��|�$��X�F$|�VM��f�V�+�Ql��RYF{dns_0J�Wd�Ku"���U�"[KɱXE��T�n��/t�tks��>3������e'K    �x|��p���g��}[X�*?F("?o5I��Vd�����3��v�"v����jw�Y�PkY��7`�V�'��h�4D
                                                     !B�����݇�O'k
��}#9�
      ��㋣TlρC�M�|��3����3I22\��әn���!v۵_���G/2>Cۃ|3�[�px�<ɹxU���Jf�~�̓;IΛub�5�2+\�Q3xf1l_2NGn��%~gOy�C���M�մ8�M�����(�\���c�3l�ҟʓ�_�k�-���'��NV;X�ƌS)
                                               �ۺ�)�W����P��k�ʮ��4����ی���Ra
�ڑlBIZ�ꬊ�
_-�S�1l���&,�{��şvJNѝ?� �W]qr�k���ftw_de��1o�_2sb7�^KC���<�J`��<P�����'��:�t%��7��z�h�'���z��~�A�,�4�8�H�     ���     C�ٻ|������]�[�PZ���,�a���hp7�k�{�+W��%>|�=�"�|�;t�-
                                                                   &vj}�,.@GM&��ah�adbeefOQjꉱ̎o��,{WE�$ԑ������
         �G�Խь� �!̳-�,�-�"���y�G����cCR�|�844��ʉ��a�%B��F'ye'���e���D�Q�V�l����5a
                                                                                ��#�V���t��Yv��N����G����W�"��0�ǒ�u���>�Ԩ�6���%�4��r�n������A��V�3}�>��͉�|����k�K���b;�kSm��T�j.�������u-�h�UG����Fh
                                                                                             �y��|d\7��t���+\���P
           ���z��
�w�%��#�������
��V|��SݠV�    Y�Yn��1i錎y��ק�R��+(��/�rb��oJ�a��?U#�?潟���k�;��Ƨ�&�$ΐѲ�+��E��"$&��
       �w�O��M3�S��}%    
 ```

解碼的時候發現解不出來，改成一次單行解碼，發現有些行數會正常顯示，推測有部分是混淆用的亂碼。
在wireshark增加`ip.dst == 18.217.1.57`這個filter就可以得到flag了(不包含最後的右括號)。

```console
$　sed 's/^.*A //; s/\..*$//' packets.txt | base64 -id -
picoCTF{dns_3xf1l_ftw_deadbeef}}
```
