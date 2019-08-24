---
layout: post
title:  "Swift String"
date:   2018-11-29 15:29:31 +0800
categories: Swift
---

## 编码
### ASCII
> `ASCII` ((`American Standard Code for Information Interchang`): 美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统，主要用于显示现代英语和其他西欧语言。

> `ASCII` 码使用指定的`7位`或`8位`二进制数组合来表示`128`或`256`种可能的字符。标准ASCII码也叫基础ASCII码，使用`7位二进制数`（剩下的1位二进制为0）来表示所有的`大写`和`小写字母`，`数字0到9`、`标点符号`，以及在美式英语中使用的`特殊控制字符`。

* 表示
    * `0～31`及`127`(共33个)是控制字符或通信专用字符（其余为可显示字符）
        * 如控制符：`LF`（换行）、`CR`（回车）、`FF`（换页）、`DEL`（删除）、`BS`（退格)、`BEL`（响铃）等；
        * 通信专用字符：`SOH`（文头）、`EOT`（文尾）、`ACK`（确认）等；`ASCII`值为8、9、10 和13 分别转换为退格、制表、换行和回车字符。
        * 它们并没有特定的图形显示，但会依不同的应用程序，而对文本显示有不同的影响。
    * `32～126`(共95个)是字符(32是空格），其中`48～57`为0到9十个阿拉伯数字。
    * `65～90`为26个大写英文字母
    * `97～122`号为26个小写英文字母
    * 其余为一些标点符号、运算符号等(详见文末附录)。

* 常见ASCII码的大小规则：`0~9<A~Z<a~z`。
    * 数字比字母要小。如 “7”<“F”；
    * 数字0比数字9要小，并按0到9顺序递增。如 “3”<“8” ；
    * 字母A比字母Z要小，并按A到Z顺序递增。如“A”<“Z” ；
    * 同个字母的大写字母比小写字母要小32。如“A”<“a” 。
    * 几个常见字母的ASCII码大小：`“A”为65`；`“a”为97`；`“0”为 48`

### ISO/IEC 8859
> 对于非英语的文字，或者受众不是美国人的时候，ASCII 编码就不够了。其他国家和语言需要不一样的字符 (就连同样说英语的英国人都需要一个表示英镑的 `£` 符号)，其中绝大多数需要的字符用七个比特是放不下的。`ISO/IEC 8859` 使用了额外的第八个比特，并且在 ASCII 范围外又定义了 16 种不同的编码。比如第 1 部分(ISO/IEC 8859-1，又叫 Latin-1)，涵盖多种西欧语言；以及第 5 部分，涵盖那些使用西里尔 (俄语) 字母的语言。

* `ISO-8859-1`编码是单字节编码，向下兼容`ASCII`，其编码范围是`0x00-0xFF`
* `0x00-0x7F`之间完全和`ASCII`一致
* `0x80-0x9F`之间是控制字符
* `0xA0-0xFF`之间是文字符号。

> 此字符集支持部分于欧洲使用的语言，包括阿尔巴尼亚语、巴斯克语、布列塔尼语、加泰罗尼亚语、丹麦语、荷兰语、法罗语、弗里西语、加利西亚语、德语、格陵兰语、冰岛语、爱尔兰盖尔语、意大利语、拉丁语、卢森堡语、挪威语、葡萄牙语、里托罗曼斯语、苏格兰盖尔语、西班牙语及瑞典语。

### Unicode
1. 起因
    > 如果你想按照 ISO/IEC 8859 来用土耳其语书写关于古希腊的内容，那你就不怎么走运了。因为你只能在第 7 部分 (Latin/Greek) 或者第 9 部分 (Turkish) 中选一种。另外，八个比特对于许多语言的编码来说依然是不够的。比如第 6 部分 (Latin/Arabic) 没有包括书写乌尔都语或者波斯语这样的阿拉伯字母语言所需要的字符。同时，在从 ASCII 的下半区替换了少量字符后，我们才能用八比特去编码基于拉丁字母但同时又有大量变音符组合的越南语。而其他东亚语言则完全不能被放入八个比特中。

2. 选择
    当固定宽度的编码空间被用完后，有两种选择：
    * 选择增加宽度
    * 切换到可变长的编码。

    最初的时候，`Unicode` 被定义成两个字节固定宽度的格式，这种格式现在被称为 `UCS-2`。不过这已经是现实问题出现之前的决定了，而且大家也都承认其实两个字节也还是不够用，四个字节的话在大多数情况下又太低效。

3. 组成
    所以今天的 Unicode 是一个可变长格式。它的可变长特性有两种不同的意义：
    * 由编码单元 (`code unit`) 组成 Unicode 标量 (`Unicode scalar`)；
    * 由 `Unicode` 标量组成字符。

4. 表示
    在表示一个 `Unicode` 的字符时，通常会用`U+`然后紧接着一组十六进制的数字来表示这一个字符。
    
5. 层次
    `Unicode` 编码系统，可分为
    * 编码方式
    * 实现方式

6. 编码方式
    统一码的编码方式与 `ISO 10646` 的通用字符集概念相对应。当前实际应用的统一码版本对应于 `UCS-2`，使用 16 位的编码空间。也就是每个字符占用 `2 个字节`。这样理论上一共最多可以表示 216（即 65536）个字符。基本满足各种语言的使用。实际上当前版本的统一码并未完全使用这 16 位编码，而是保留了大量空间以作为特殊使用或将来扩展。

7. 实现方式
    `Unicode` 的实现方式不同于编码方式。一个字符的 `Unicode` 编码是确定的。但是在实际传输过程中，由于不同系统平台的设计不一定一致，以及出于节省空间的目的，对 `Unicode` 编码的实现方式有所不同。`Unicode` 的实现方式称为 `Unicode`转换格式（`Unicode Transformation Format`，简称为 `UTF`）。
    
### UTF-8
UTF-8（`8-bit Unicode Transformation Format`）是一种针对`Unicode`的可变长度字符编码，也是一种前缀码。它可以用来表示`Unicode`标准中的任何字符，且其编码中的第一个字节仍与`ASCII`兼容，这使得原来处理`ASCII`字符的软件无须或只须做少部分修改，即可继续使用。

编码字节含义：

* 对于UTF-8编码中的任意字节B，如果B的第一位为0，则B独立的表示一个字符(ASCII码)；
* 如果B的第一位为1，第二位为0，则B为一个多字节字符中的一个字节(非ASCII字符)；
* 如果B的前两位为1，第三位为0，则B为两个字节表示的字符中的第一个字节；
* 如果B的前三位为1，第四位为0，则B为三个字节表示的字符中的第一个字节；
* 如果B的前四位为1，第五位为0，则B为四个字节表示的字符中的第一个字节；

因此，对UTF-8编码中的任意字节，根据第一位，可判断是否为ASCII字符；根据前二位，可判断该字节是否为一个字符编码的第一个字节；根据前四位（如果前两位均为1），可确定该字节为字符编码的第一个字节，并且可判断对应的字符由几个字节表示；根据前五位（如果前四位为1），可判断编码是否有错误或数据传输过程中是否有错误。

`Unicode` 和 `UTF-8` 之间的转换关系表 ( `x` 字符表示码点占据的位 )

| 码点的位数 | 码点起值 | 码点终值| 字节序列 | Byte 1| Byte 2|Byte 3| Byte 4| Byte 5|Byte 6|
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|   7  | U+0000    | U+007F| 1   | 0xxxxxxx |
| 11   | U+0080    | U+07FF     | 2   | 110xxxxx  | 10xxxxxx |
| 16   | U+0800    | U+FFFF     | 3   | 1110xxxx  | 10xxxxxx  | 10xxxxxx |
| 21   | U+10000   | U+1FFFFF   | 4   | 11110xxx  | 10xxxxxx  | 10xxxxxx  |10xxxxxx |
| 26   | U+200000  | U+3FFFFFF  | 5   | 111110xx  | 10xxxxxx  | 10xxxxxx  | 10xxxxxx  | 10xxxxxx |
| 31   | U+4000000 | U+7FFFFFFF | 6   | 1111110x  | 10xxxxxx  | 10xxxxxx  | 10xxxxxx  | 10xxxxxx  | 10xxxxxx |

转换示例：
下面由 `D`,`o`,`g`,`‼`(`DOUBLE EXCLAMATION MARK`, Unicode 标量 `U+203C`)和 🐶(`DOG FACE，Unicode` 标量为 `U+1F436`)组成的字符串中的每一个字符代表着一种不同的表示：

```
let dogString = "Dog‼🐶"
```

可以通过遍历 `String` 的 `utf8` 属性来访问它的 `UTF-8` 表示。其为 `String.UTF8View` 类型的属性，`UTF8View` 是无符号 8 位（`UInt8`）值的集合，每一个 `UInt8` 值都是一个字符的 `UTF-8 `表示：

```
let dogString = "Dog‼🐶"
dogString.utf8.forEach { print($0) }
```

| Character | D<br>U+0044 | o<br>U+006F | g<br>U+0067 | ‼<br>U+203C | 🐶<br>U+1F436 |  |  |  |  |  |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| UTF-8 Code Unit | 68 | 111 | 103 | 226 | 128 | 188 | 240 | 159 | 144 | 182 |
| hexadecimal | 0x44 | 0x6F | 0x67 | 0xE2 | 0x80 | 0xBC | 0xF0 | 0X9F | 0x90 | 0XB6 |
| Position | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |

前三个 10 进制 `codeUnit` 值（`68`、`111`、`103`）代表了字符 `D`、`o` 和 `g`，它们的 `UTF-8` 表示与 `ASCII` 表示相同。接下来的三个 10 进制 `codeUnit` 值（`226`、`128`、`188`）是 `DOUBLE EXCLAMATION MARK` 的3字节 `UTF-8` 表示。最后的四个 `codeUnit` 值（`240`、`159`、`144`、`182`）是 `DOG FACE` 的4字节 `UTF-8` 表示。

以下已 🐶 为例做转换：
由转换关系表可知 `U+1F436` 转换后的格式为, 其中码点需要 21 位（`x`的个数）

```
1111 0xxx 10xx xxxx 10xx xxxx 10xx xxxx
```

`1F436` 的二进制数表示为, 一共 20 位，不足码点的占位数（21位），因此需要在前面补0，凑齐码点的占位数（21位）

```
0001 1111 0100 0011 0110
```

补齐后的二进制数为

```
0 0001 1111 0100 0011 0110
```

把补齐后的二进制数依次替换掉占位码点（`x`）

```
1111 0xxx 10xx xxxx 10xx xxxx 10xx xxxx
	  000   01 1111   01 0000   11 0110

	  ||
	  ||
	  ||
	  \/

1111 0000 1001 1111 1001 0000 1011 0110
```

转换为 16 进制为

| 二进制 |1111 0000 | 1001 1111 | 1001 0000 | 1011 0110 |
| :-:| :-: | :-: | :-: | :-: |
| 十六进制 | 0xF0 | 0x9F | 0x90 | 0xB6 |
| 十进制 | 240 | 159 | 144 | 182 |

## 附录
1. ASCII码表
<table log-set-param="table_view" width="99%" class="table-view log-set-param" data-sort="sortDisabled"><tbody><tr><td height="49" align="center" valign="middle"><div class="para" label-module="para">Bin</div>
<div class="para" label-module="para">(二进制)</div>
</td><td height="49" align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">Oct</div>
<div class="para" label-module="para">(八进制)</div>
</td><td height="49" align="center" valign="middle"><div class="para" label-module="para">Dec</div>
<div class="para" label-module="para">(十进制)</div>
</td><td height="49" align="center" valign="middle"><div class="para" label-module="para">Hex</div>
<div class="para" label-module="para">(十六进制)</div>
</td><td height="49" align="center" valign="middle"><div class="para" label-module="para">缩写/字符</div>
</td><td height="49" align="center" valign="middle" colspan="1"><div class="para" label-module="para">解释</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">00</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x00</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">NUL(null)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">空字符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">01</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">1</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x01</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">SOH(start of headline)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">标题开始</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">02</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">2</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x02</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">STX (start of text)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">正文开始</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">03</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">3</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x03</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">ETX (end of text)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">正文结束</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">04</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">4</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x04</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">EOT (end of transmission)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">传输结束</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">05</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">5</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x05</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">ENQ (enquiry)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">请求</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">06</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">6</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x06</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">ACK (acknowledge)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">收到通知</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">07</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">7</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x07</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">BEL (bell)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">响铃</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">010</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">8</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x08</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">BS (backspace)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">退格</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">011</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">9</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x09</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">HT (horizontal tab)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">水平制表符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">012</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">10</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x0A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">LF (NL line feed, new line)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">换行键</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">013</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">11</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x0B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">VT (vertical tab)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">垂直制表符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">014</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">12</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x0C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">FF (NP form feed, new page)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">换页键</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">015</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">13</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x0D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">CR (carriage return)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">回车键</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">016</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">14</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x0E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">SO (shift out)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">不用切换</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0000 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">017</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">15</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x0F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">SI (shift in)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">启用切换</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">020</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">16</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x10</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">DLE (data link escape)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">数据链路转义</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">021</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">17</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x11</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">DC1 (device control 1)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">设备控制1</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">022</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">18</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x12</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">DC2 (device control 2)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">设备控制2</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">023</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">19</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x13</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">DC3 (device control 3)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">设备控制3</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">024</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">20</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x14</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">DC4 (device control 4)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">设备控制4</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">025</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">21</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x15</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">NAK (negative acknowledge)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">拒绝接收</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">026</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">22</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x16</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">SYN (synchronous idle)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">同步空闲</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">027</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">23</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x17</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">ETB (end of trans. block)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">结束传输块</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">030</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">24</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x18</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">CAN (cancel)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">取消</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">031</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">25</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x19</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">EM (end of medium)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">媒介结束</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">032</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">26</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x1A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">SUB (substitute)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">代替</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">033</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">27</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x1B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">ESC (escape)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">换码(溢出)</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">034</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">28</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x1C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">FS (file separator)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">文件分隔符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">035</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">29</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x1D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">GS (group separator)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">分组符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">036</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">30</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x1E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">RS (record separator)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">记录分隔符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0001 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">037</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">31</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x1F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">US (unit separator)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">单元分隔符</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">040</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">32</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x20</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">(space)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">空格</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">041</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">33</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x21</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">!</div>
</td><td height="0" align="center" valign="middle" colspan="1">叹号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">042</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">34</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x22</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">"</div>
</td><td height="0" align="center" valign="middle" colspan="1">双引号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">043</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">35</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x23</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">#</div>
</td><td height="0" align="center" valign="middle" colspan="1">井号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">044</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">36</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x24</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">$</div>
</td><td height="0" align="center" valign="middle" colspan="1">美元符</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">045</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">37</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x25</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">%</div>
</td><td height="0" align="center" valign="middle" colspan="1">百分号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">046</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">38</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x26</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">&amp;</div>
</td><td height="0" align="center" valign="middle" colspan="1">和号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">047</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">39</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x27</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">'</div>
</td><td height="0" align="center" valign="middle" colspan="1">闭单引号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">050</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">40</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x28</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">(</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">开括号</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">051</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">41</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x29</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">)</div>
</td><td height="0" align="center" valign="middle" colspan="1"><div class="para" label-module="para">闭括号</div>
</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">052</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">42</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x2A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">*</div>
</td><td height="0" align="center" valign="middle" colspan="1">星号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">053</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">43</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x2B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">+</div>
</td><td height="0" align="center" valign="middle" colspan="1">加号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">054</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">44</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x2C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">,</div>
</td><td height="0" align="center" valign="middle" colspan="1">逗号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">055</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">45</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x2D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">-</div>
</td><td height="0" align="center" valign="middle" colspan="1">减号/破折号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">056</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">46</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x2E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">.</div>
</td><td height="0" align="center" valign="middle" colspan="1">句号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0010 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">057</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">47</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x2F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">/</div>
</td><td height="0" align="center" valign="middle" colspan="1">斜杠</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">060</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">48</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x30</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0</div>
</td><td height="0" align="center" valign="middle" colspan="1">字符0</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">061</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">49</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x31</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">1</div>
</td><td align="center" valign="middle">字符1</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">062</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">50</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x32</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">2</div>
</td><td align="center" valign="middle">字符2</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">063</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">51</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x33</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">3</div>
</td><td align="center" valign="middle">字符3</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">064</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">52</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x34</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">4</div>
</td><td align="center" valign="middle">字符4</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">065</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">53</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x35</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">5</div>
</td><td align="center" valign="middle">字符5</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">066</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">54</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x36</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">6</div>
</td><td align="center" valign="middle">字符6</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">067</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">55</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x37</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">7</div>
</td><td align="center" valign="middle">字符7</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">070</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">56</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x38</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">8</div>
</td><td align="center" valign="middle">字符8</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">071</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">57</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x39</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">9</div>
</td><td align="center" valign="middle">字符9</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">072</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">58</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x3A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">:</div>
</td><td align="center" valign="middle">冒号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">073</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">59</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x3B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">;</div>
</td><td align="center" valign="middle">分号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">074</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">60</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x3C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">&lt;</div>
</td><td align="center" valign="middle">小于</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">075</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">61</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x3D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">=</div>
</td><td align="center" valign="middle">等号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">076</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">62</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x3E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">&gt;</div>
</td><td align="center" valign="middle">大于</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0011 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">077</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">63</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x3F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">?</div>
</td><td align="center" valign="middle">问号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0100</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">64</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x40</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">@</div>
</td><td align="center" valign="middle">电子邮件符号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0101</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">65</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x41</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">A</div>
</td><td align="center" valign="middle">大写字母A</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0102</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">66</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x42</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">B</div>
</td><td align="center" valign="middle">大写字母B</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0103</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">67</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x43</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">C</div>
</td><td align="center" valign="middle">大写字母C</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0104</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">68</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x44</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">D</div>
</td><td align="center" valign="middle">大写字母D</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0105</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">69</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x45</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">E</div>
</td><td align="center" valign="middle">大写字母E</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0106</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">70</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x46</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">F</div>
</td><td align="center" valign="middle">大写字母F</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0107</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">71</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x47</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">G</div>
</td><td align="center" valign="middle">大写字母G</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0110</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">72</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x48</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">H</div>
</td><td align="center" valign="middle">大写字母H</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0111</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">73</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x49</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">I</div>
</td><td align="center" valign="middle">大写字母I</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">01001010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0112</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">74</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x4A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">J</div>
</td><td align="center" valign="middle">大写字母J</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0113</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">75</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x4B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">K</div>
</td><td align="center" valign="middle">大写字母K</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0114</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">76</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x4C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">L</div>
</td><td align="center" valign="middle">大写字母L</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0115</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">77</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x4D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">M</div>
</td><td align="center" valign="middle">大写字母M</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0116</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">78</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x4E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">N</div>
</td><td align="center" valign="middle">大写字母N</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0100 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0117</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">79</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x4F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">O</div>
</td><td align="center" valign="middle">大写字母O</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0120</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">80</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x50</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">P</div>
</td><td align="center" valign="middle">大写字母P</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0121</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">81</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x51</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">Q</div>
</td><td align="center" valign="middle">大写字母Q</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0122</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">82</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x52</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">R</div>
</td><td align="center" valign="middle">大写字母R</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0123</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">83</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x53</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">S</div>
</td><td align="center" valign="middle">大写字母S</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0124</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">84</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x54</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">T</div>
</td><td align="center" valign="middle">大写字母T</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0125</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">85</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x55</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">U</div>
</td><td align="center" valign="middle">大写字母U</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0126</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">86</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x56</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">V</div>
</td><td align="center" valign="middle">大写字母V</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0127</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">87</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x57</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">W</div>
</td><td align="center" valign="middle">大写字母W</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0130</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">88</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x58</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">X</div>
</td><td align="center" valign="middle">大写字母X</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0131</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">89</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x59</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">Y</div>
</td><td align="center" valign="middle">大写字母Y</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0132</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">90</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x5A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">Z</div>
</td><td align="center" valign="middle">大写字母Z</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0133</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">91</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x5B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">[</div>
</td><td align="center" valign="middle">开方括号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0134</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">92</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x5C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">\</div>
</td><td align="center" valign="middle">反斜杠</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0135</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">93</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x5D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">]</div>
</td><td align="center" valign="middle">闭方括号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0136</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">94</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x5E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">^</div>
</td><td align="center" valign="middle">脱字符</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0101 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0137</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">95</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x5F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">_</div>
</td><td align="center" valign="middle">下划线</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0140</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">96</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x60</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">`</div>
</td><td align="center" valign="middle">开单引号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0141</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">97</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x61</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">a</div>
</td><td align="center" valign="middle">小写字母a</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0142</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">98</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x62</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">b</div>
</td><td align="center" valign="middle">小写字母b</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0143</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">99</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x63</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">c</div>
</td><td align="center" valign="middle">小写字母c</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0144</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">100</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x64</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">d</div>
</td><td align="center" valign="middle">小写字母d</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0145</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">101</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x65</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">e</div>
</td><td align="center" valign="middle">小写字母e</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0146</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">102</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x66</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">f</div>
</td><td align="center" valign="middle">小写字母f</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0147</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">103</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x67</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">g</div>
</td><td align="center" valign="middle">小写字母g</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0150</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">104</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x68</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">h</div>
</td><td align="center" valign="middle">小写字母h</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0151</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">105</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x69</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">i</div>
</td><td align="center" valign="middle">小写字母i</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0152</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">106</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x6A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">j</div>
</td><td align="center" valign="middle">小写字母j</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0153</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">107</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x6B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">k</div>
</td><td align="center" valign="middle">小写字母k</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0154</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">108</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x6C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">l</div>
</td><td align="center" valign="middle">小写字母l</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0155</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">109</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x6D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">m</div>
</td><td align="center" valign="middle">小写字母m</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0156</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">110</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x6E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">n</div>
</td><td align="center" valign="middle">小写字母n</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0110 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0157</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">111</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x6F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">o</div>
</td><td align="center" valign="middle">小写字母o</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0160</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">112</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x70</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">p</div>
</td><td align="center" valign="middle">小写字母p</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0161</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">113</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x71</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">q</div>
</td><td align="center" valign="middle">小写字母q</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0162</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">114</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x72</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">r</div>
</td><td align="center" valign="middle">小写字母r</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0163</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">115</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x73</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">s</div>
</td><td align="center" valign="middle">小写字母s</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0164</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">116</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x74</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">t</div>
</td><td align="center" valign="middle">小写字母t</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0165</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">117</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x75</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">u</div>
</td><td align="center" valign="middle">小写字母u</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0166</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">118</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x76</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">v</div>
</td><td align="center" valign="middle">小写字母v</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 0111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0167</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">119</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x77</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">w</div>
</td><td align="center" valign="middle">小写字母w</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1000</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0170</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">120</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x78</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">x</div>
</td><td align="center" valign="middle">小写字母x</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1001</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0171</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">121</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x79</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">y</div>
</td><td align="center" valign="middle">小写字母y</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1010</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0172</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">122</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x7A</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">z</div>
</td><td align="center" valign="middle">小写字母z</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1011</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0173</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">123</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x7B</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">{</div>
</td><td align="center" valign="middle">开花括号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1100</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0174</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">124</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x7C</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">|</div>
</td><td align="center" valign="middle">垂线</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1101</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0175</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">125</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x7D</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">}</div>
</td><td align="center" valign="middle">闭花括号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1110</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0176</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">126</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x7E</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">~</div>
</td><td align="center" valign="middle">波浪号</td></tr><tr><td align="center" valign="middle"><div class="para" label-module="para">0111 1111</div>
</td><td align="center" valign="middle" colspan="1" rowspan="1"><div class="para" label-module="para">0177</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">127</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">0x7F</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">DEL (delete)</div>
</td><td align="center" valign="middle"><div class="para" label-module="para">删除</div>
</td></tr></tbody></table>

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

