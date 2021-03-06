---
title: ASCII 编码和 GB2312 编码
---

计算机的数据类型分为数值型和非数值型，非数值型的实现其实就是给每一个字符进行相应地标号，我们称之为编码，不同的语言有不同的编码方式，这里介绍最常用的 ASCII 编码和 GB2312 编码。

## ASCII 编码

ASCII 字符集包括了所有英文字母和一些常用符号，还有 33 个功能码。一个字符用一个字节的二进制表示，首位为 0，其余 7 位参照 ASCII 码表，行表头表示高三位 $b_6b_5b_4$，列表头表示第四位 $b_3b_2b_1b_0$。

|          | 000  | 001  | 010  | 011   | 100   | 101  | 110   | 111  |
| -------- | ---- | ---- | ---- | ----- | ----- | ---- | ----- | ---- |
| **0000** | NUL  | DLE  | SP   | **0** | @     | P    | 、    | p    |
| **0001** | SOH  | DC1  | !    | 1     | **A** | Q    | **a** | q    |
| **0010** | STX  | DC2  | “    | 2     | B     | R    | b     | r    |
| **0011** | ETX  | DC3  | \#   | 3     | C     | S    | c     | s    |
| **0100** | EOT  | DC4  | $    | 4     | D     | T    | d     | t    |
| **0101** | ENQ  | NAK  | %    | 5     | E     | U    | e     | u    |
| **0110** | ACK  | SYN  | &    | 6     | F     | V    | f     | v    |
| **0111** | BEL  | ETB  | ‘    | 7     | G     | W    | g     | w    |
| **1000** | BS   | CAN  | (    | 8     | H     | X    | h     | x    |
| **1001** | HT   | EM   | )    | 9     | I     | Y    | i     | y    |
| **1010** | LF   | SUB  | *    | :     | J     | Z    | j     | z    |
| **1011** | VT   | ESC  | +    | ;     | K     | [    | k     | {    |
| **1100** | FF   | FS   | ,    | <     | L     | \    | l     | \|   |
| **1101** | CR   | GS   | -    | =     | M     | ]    | m     | }    |
| **1110** | SO   | RS   | .    | \>    | N     | ^    | n     | ~    |
| **1111** | SI   | US   | /    | ?     | O     | _    | o     | DEL  |

一般情况下，我们只需要记住 0、A、a 的 ASCII 码即可，其他可以推出来。

- 功能码

  观察 0~32 的编码和 127 的编码，这些字符不会出现在屏幕上，但是它们都有特殊的功能，因此被称为功能码，不论何种字符集，都应该包含这些功能码。例如 127 的编码便是删除的功能。其他功能码的作用可以自行百度。

## GB2312 编码

GB 是国标的缩写，这是我国汉字的标准编码，于1980年开始制定。基本集共收入汉字 6763 个和非汉字图形字符 682 个。

1. 区位码

   汉字这么多，一个字节是不够放了，所以我们的汉字是用两个字节进行编码，可以收录 94\*94 个不同的字符，因为汉字也是需要功能码的，每个字节后七位可以表示 128 个位置，减去 34个功能码，只剩下 94 个位置。我们将第一个字节的编码成为区，第二个字节的编号成为位，故称区位码。(“啊” 的区位码是1601，十六进制为1001H)。

2. 国标码（汉字交换码）

   区位码是从 0101H 开始的，这和功能码产生了冲突，为了兼容功能码，因此给每个字节加上 32（十六进制是 20），完美解决。（“啊” 的区位码是 1001H，加上 2020H，得到国标码为 3021H）。如何你的计算机使用该国标码，也称之为汉字交换码。

3. 机内码

   如何运用在计算机上，我们发现国标码和 ASCII 码发生了冲突，为了和 ASCII 码区分开来，我们给每个字节的首位置一，也就是每个字节加上 80H。（“啊” 的国标码是3021H，加上 8080H 得到其机内码为 B0A1H）。因此，区分中英文可以逐个判断字节首位是否为 1。

   | **汉字** | **区位码** | **汉字国标码** | **汉字机内码** |
   | -------- | ---------- | -------------- | -------------- |
   | **中**   | 5448       | 8680=5650H     | D6D0H          |
   | **国**   | 2590       | 57122=397AH    | B9FAH          |

4. 汉字字形码

   字形码是一种用点阵表示汉字字形的编码，它主要用于汉字输出(打印、显示等)时产生的汉字字形。每个汉字都有对应的一个字形码，所有字形码的集合称为汉字库。其中点阵大小类型有 `16*16`、`24*24`、`32*32` 以上。

   例如：把一个方块横向和纵向都分为16 格。若用 1 表示黑点，用 0 表示白点，则 16×16 的点阵汉字可用 256 位二进制数来表示，占用 32B。汉字 “宝” 的 16×16 点阵数字化信息。

   ![汉字"宝"](https://raw.githubusercontent.com/wiki/professordeng/blog/computer/bao.png)

   按行翻译得到其十六进制流为： 02H 00H、01H 04H、7FH FEH、40H 04H、80H 08H、00H 00H、3FH F8H、01H 00H、01H 00H、1FH F0H、01H 00H、01H 40H、01H 20H、01H 20H、7FH FCH、00H 00H。

5. 汉字输入码

   汉字输入码就是让用户直接使用标准键盘输入汉字。五笔就是一种汉字输入码，现在大多数人都是利用拼音输入法，我是用微软自带的。例如“啊”的汉字输入码就是按键 A 。

## 总结

1. 计算机利用 ASCII 编码来存储英文信息。
2. 每一种字符集都包含功能码。
3. 要想让一个汉字在屏幕上显示，我们得敲入汉字输入码，汉字输入码会先转化为汉字交换码（类似拼音打字之后的选择），然后得到汉字机内码，机内码会从汉字库中得到汉字字形码，最后输出到屏幕或者打印出来。   



{{ site.math }}