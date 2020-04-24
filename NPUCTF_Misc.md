用户名：xyxmm
联系方式：QQ：907648980
          校内邮箱：2019302931@mail.nwpu.edu.cn

Misc
打不开的图片（emm,具体题目被我删了，只能靠回忆了）

首先，下载附件发现是.zip后缀，解压后发现一个文件，用winhex打开分析格式。发现文件头是233…，显然被更改过，再去看文件尾，百度以后发现是.png的，继续百度出.png的文件头，改回去。
然后发现第二行的宽度全为零，（由于本人是CTF小白，也不知道有CRC这种东西，所以当时随便改了一个宽度，导致后面一天都走了偏路），于是又去网上搜了一波关于png宽度和高度的修改问题，找到了一段python代码(更改后的)（如下）
import struct
import binascii
import os

m = open("lxy.png","rb").read()
for i in range(1024):
    c = m[12:16] + struct.pack('>i', i) + m[20:29]
    crc = binascii.crc32(c) & 0xffffffff
    if crc == 0xc02f6b4f:
        print(i)

改了一下CRC值，运行程序后找到了宽度为439，换成16进制以后，代入文件。
再将文件后缀改为.png，打开得到下图得到弗拉格：MZWGCZ33OV2HC3D5。直接提交后发现不对，于是怀疑这是通过加密以后得出的结果。我先用base64转换了一下，发现明显不是，就又开启了百度模式，找了关于各种密码加密后的特征，进行一一比对。由于最开始以为O是0，导致第一遍搜索失败，但base32加密后只有26个字母和2-7的数字的特征引起了我的警觉，我又重新看了一下原图，发现0可能是O，尝试着用解码工具解密后，最终得到了flag{utql}的结果。