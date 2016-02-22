---
title: Unicode和UTF基础
---


# Unicode

Unicode是一个字符集, 为每一个字符提供一个唯一的编号, 由[Unicode Consortium][2]维护.


详细信息参考[wikipedia: Unicode][3], [http://www.unicode.org/][4]

# code point

Unicode为字符提供的唯一编号叫做`code point`或者`code position`.

# Unicode Plane

在Unicode标准中, 一个[plane][5]由连续的**65536(2的十六次方)** 个code point组成.

一共有17个plane, 位置编码的前两位表示plane编码, 即**hh hhhh**的前两位.

plane 0 叫做[Basic Multilingual Plane][6], 其余的plane叫做Supplementary plane

# Basic Multilingual Plane

BMP几乎包括现代语言的所有字符和大量图标. 其中的大部分code point用于编码[CJK字符][7]


# UTF

Unicode Transformation Format(UTF)是用于将每一个Unicode code point映射为一个唯一的字节序列的编码算法. 常见的编码算法有UTF8, UTF-16, UTF-32.

# code unit

编码算法用于表示字符的最小bit长度, 如UTF8的code unit是8, UTF16的code unit是16

# UTF8

[UTF8][8]的code unit为8 bit

[9]: http://illegalargumentexception.blogspot.jp/2010/12/javascript-validating-utf-8-string.html
[8]: https://en.wikipedia.org/wiki/UTF-8
[7]: https://en.wikipedia.org/wiki/CJK_characters
[6]: https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane
[5]: https://en.wikipedia.org/wiki/Plane_(Unicode)
[4]: http://www.unicode.org/
[3]: https://en.wikipedia.org/wiki/Unicode
[2]: http://www.unicode.org/consortium/consort.html
[1]: http://www.unicode.org/standard/WhatIsUnicode.html
