# RE-Perhaps You Can - BlueHens CTF 

Problem: `550d0d0a00000000b7e8566370030000e300000000000000000000000000000000080000004000000073300200006500640083015a0165026501830164016b02731c650364028301010065046501a005a100830164036b02733465036402830101006501640464058502190064066b02734c650364028301010065016407190064086b027360650364028301010065066501640919008301640a1800650665016405190083016b02738465036402830101006501640b190065076508650965066501640c19008301830164046404640785031900830183016b0273b6650364028301010065066501640b19008301640d180065066501640d190083016b0273da650364028301010065066501640e190083015a0a65066501640f190083015a0b650a650b170064106b02900173086503640283010100650c650a650b830264116b0090017320650364028301010065016412641385021900a005a100a00da10064146b029001734265036402830101006501641319006501641519006b029001735c65036402830101006501641519006501641619006b029001737665036402830101006506650164171900830165066501641819008301140064196b029001739c650364028301010065066501641a1900830165066501641b19008301140065066501641c190083011400641d6b02900173ce6503640283010100650e641ea00f6510641f64208400650164216422850219008302a10183015a116511a012a1000100641ea00f6511a10164236b029002730e650364028301010065016424190064256b0290027324650364028301010065136426830101006404530029277a07696e7075743e20e926000000e90100000069d20d00004ee9060000007a0655444354467be9fffffffffa017de905000000e902000000e907000000e903000000e909000000e908000000e90a000000e9eb000000e977000000e90b000000e9180000005a1a3333356636323333363536653566363237393734373433333665e91b000000e91f000000e919000000e91a00000069522e0000e91c000000e91d000000e91e0000006960630900da0063010000000000000000000000010000000300000043000000730c000000740074017c0083018301530029014e2902da03737472da036f72642901da0178a900721c000000fa087472796d652e7079da083c6c616d6264613e20000000f300000000721e000000e920000000e9250000005a0e3030303131313131313132353537e922000000da01347a07596f752077696e2914da05696e707574da03746d70da036c656eda0465786974da0373756dda06656e636f6465721a000000da03636872da03696e747219000000721b000000da0179da036d6178da03686578da046c697374da046a6f696eda036d6170da027979da04736f7274da057072696e74721c000000721c000000721c000000721d000000da083c6d6f64756c653e01000000734a00000008010c01080110010801100108010c0108011c0108012a0108011c0108010c010c010e010801100108011a01080112010801120108011e0108012a01080120010801100108010e010801`

Nhìn giống hex nên mình ném qua cyber chef:

![](https://i.imgur.com/lhcOkxY.png)

Sau khi xoá bớt ký tự thừa thì được: 

![](https://i.imgur.com/1QZDUK8.png)

Trông đoạn `str ord` giống như code Python nên mình liên tưởng tới đây là hex của file .PYC 
Ném đoạn hex lên http://tomeko.net/online_tools/hex_to_file.php để tái tạo lại file .PYC

Khi đã có file [.PYC](myfile.pyc) rồi ta ném lại sang https://www.decompiler.com/ để ngược về file .PY 

Code Python sau khi dịch từ .PYC
```python=
# uncompyle6 version 3.7.4
# Python bytecode 3.8 (3413)
# Decompiled from: Python 2.7.17 (default, Sep 30 2020, 13:38:04) 
# [GCC 7.5.0]
# Warning: this version of Python has problems handling the Python 3 "byte" type in constants properly.

# Embedded file name: tryme.py
# Compiled at: 2022-10-24 21:34:15
# Size of source mod 2**32: 880 bytes
tmp = input('input> ')
if not len(tmp) == 38:
    exit(1)
if not sum(tmp.encode()) == 3538:
    exit(1)
if not tmp[:6] == 'UDCTF{':
    exit(1)
if not tmp[(-1)] == '}':
    exit(1)
if not ord(tmp[5]) - 2 == ord(tmp[6]):
    exit(1)
if not tmp[7] == chr(int(str(ord(tmp[3]))[::-1])):
    exit(1)
if not ord(tmp[7]) - 9 == ord(tmp[9]):
    exit(1)
x = ord(tmp[8])
y = ord(tmp[10])
if not x + y == 235:
    exit(1)
if not max(x, y) < 119:
    exit(1)
if not tmp[11:24].encode().hex() == '335f6233656e5f62797474336e':
    exit(1)
if not tmp[24] == tmp[27]:
    exit(1)
if not tmp[27] == tmp[31]:
    exit(1)
if not ord(tmp[25]) * ord(tmp[26]) == 11858:
    exit(1)
if not ord(tmp[28]) * ord(tmp[29]) * ord(tmp[30]) == 615264:
    exit(1)
yy = list(''.join(map(lambda x: str(ord(x)), tmp[32:37])))
yy.sort()
if not ''.join(yy) == '00011111112557':
    exit(1)
if not tmp[34] == '4':
    exit(1)
print('You win')
```

Chúng ta sẽ quét lần lượt từng lệnh một trong code python

Format của flag:
```python=
tmp[:6] == 'UDCTF{'
tmp[(-1)] == '}'
len(tmp) == 38  # flag có 38 ký tự 
sum(tmp.encode()) == 3538  # tổng giá trị flag cộng lại bằng 3538
```
--> **UDCTF{}**
Dịch các ký tự từ vị trí 6 đến 10:
```python=
ord(tmp[5]) - 2 == ord(tmp[6])
# ký tự thứ 6 của flag ít hơn 2 đơn vị ký tự thứ 5 --> y
tmp[7] == chr(int(str(ord(tmp[3]))[::-1]))
# Ký tự thứ 7 là --> 0
ord(tmp[7]) - 9 == ord(tmp[9])  # ký tự thứ 9 của flag ít hơn 9 đơn vị ký tự thứ 7
x = ord(tmp[8])  # x là ascii của ký tự thứ 8 
y = ord(tmp[10])  # y là ascii của ký tự thứ 10 
x + y == 235  # x + y = 235 (tương ứng ký tự 8 + ký tự 10 bằng 235)
max(x, y) < 119  # x và y đều phải nhỏ hơn 119
# --> Ký tự thứ 8 và thứ 10 là 117 và 118 --> u và v
```
--> **y0u'v**

Dịch các ký tự từ 11 đến 27:
```python
tmp[11:24].encode().hex() == '335f6233656e5f62797474336e' 
# Ký tự thứ 11 đến 23 là: "3_b3en_bytt3n"
tmp[24] == tmp[27]: # ký tự thứ 24 bằng ký tự thứ 27 --> _
ord(tmp[25]) * ord(tmp[26]) == 11858  # Ký tự thứ 25 * Ký tự thứ 26 bằng 11858
# khả năng cao là 98 và 121 --> b và y (cho có nghĩa)
```
--> **3_b3en_bytt3n_by_**

Dịch các ký tự từ 28 đến 36:

```python=
tmp[27] == tmp[31]
# Ký tự thứ 27 bằng ký tự thứ 31 --> _
```
```python=
ord(tmp[28]) * ord(tmp[29]) * ord(tmp[30]) == 615264
# Ký tự thứ 28 * Ký tự thứ 29 *Ký tự thứ 30 =  615264 
```
Dùng tool https://calculat.io/en/number/factors-of

![](https://i.imgur.com/3Cy9JNZ.png)

Thì dựa vào việc flag có chứa số (hoặc chứ cái in hoa, thường) thì ta lọc được trường hợp của 3 ký tự này là `3, 4, D, N, W, f, h, t` --> Sau khi tư duy lọc ra được `f4t, h4t, th3, W3t` --> tuỳ thuộc vào từ cuối của flag để chọn

```python=
yy = list(''.join(map(lambda x: str(ord(x)), tmp[32:37]))) # nôm na là tách các ký tự ra thành từng số nhỏ rồi tạo mảng (ví dụ a -- > 97 --> [9, 7])
yy.sort()  # --> Sắp xếp lại mảng
''.join(yy) == '00011111112557':
```

```python
tmp[34] == '4':  # Ký tự thứ 34 là "4"
```
Dựa vào ký tự 34 là 4 --> 52 Suy ra loại trừ còn `000111111157` --> 4 số còn lại phải mỗi số đều có giá trị 3 chữ số mới đủ --> chạy từ `d` đến `z`

Sau một hồi sắp xếp thì mình nhận ra được các ký tự thoả mãn tạo nên từ là `s, n, 4, k, e` 

Kết hợp các từ đã lọc được của các ký tự 28, 29, 30 --> cụm còn thiếu là `th3`
--> **th3_sn4ke**

## flag: UDCTF{y0u'v3_b3en_bytt3n_by_th3_sn4ke}
