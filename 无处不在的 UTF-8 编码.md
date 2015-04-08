#无处不在的 UTF-8 宣言


##1 这份文档的目的

为了提升对 UTF-8 编码的使用与支持，为了让人们确信 UTF-8 编码应该是在内存或磁盘中存储文本字符串用于沟通和
所有其他用途的默认编码选择。我们相信所有其它 Unicode（或文本）的编码属于极少的优化的边界情况，应该被主流
的用户避免。

特别地，我们认为非常流行的 UTF-16 编码（在 Windows 世界被错用做宽字符（widechar）和 Unicode 的同义词）不
足以用在库 APIs 中（除非用于处理文本的特殊的库）。

本文档建议在 Windows 应用程序中使用 UTF-8 编码存储字符串，尽管由于历史原因和原生API级别 UTF-8 支持的缺乏
这个标准在这个领域并不流行。但是，我们相信即使在这个平台上，后面的一些观点胜过原生支持的缺乏。并且，我们
建议永远忘记内码表（ANSI codepages）和它们的用途。在文本字符串中混用任意种语言是属于用户的权利。

我们建议避免 C++ 应用代码依赖于 UNICODE 或者 _UNICODE 定义。这包括 Windows 上的 TCHAR/LPTSTR 类型和定义
为宏的APIs，例如 CreateWindow 和 _tcslen。我们也建议使用其他的方式到达这些 APIs 的目的。

我们还认为，如果一个应用不是专业用于文本，它的架构必须让程序无需考虑编码问题。举例来说，一个文件复制工具
为了支持非英语文件名不应该被写的不同。[Joel 关于 Unicode 的优秀文章](http://www.joelonsoftware.com/articles/Unicode.html)给初学者很好地解释了编码，但是它缺乏最
重要的部分：一个程序员应该如何继续，如果他不在意[一个字符串中有什么](http://en.wikipedia.org/wiki/Opaque_data_type)。


##2 背景

1988年，Joseph D. Becker 发布了[第一个 Unicode 草案提议](http://unicode.org/history/unicode88.pdf)。他的
设计的基础是一个天真的假设每个字符 16 位足够。在1991年，Unicode 标准的第一个版本发布，码点背限制在16个字
符。在接下来的几年中，许多系统添加了对 Unicode 的支持并且转换到了 UCS-2 编码。对于新技术，例如 Qt 框架，
（1992），Windows NT 3.1（1993）和 Java （1995），它是非常有吸引力的。

然而，很快就发现每个字符16位对于 Unicode 是不够的。在1996年，UTF-16 编码被创造出来，从而使现存的系统能够
处理非16位的字符。这使得最初选用16位字符编码，也就是固定宽度编码的理论没有了依据。现在，Unicode 包含109449
个字符，其中大约74500个属于中日韩（CJK）统一表意文字。

![image](https://github.com/Mjinrui/UTF-8Everywhere/blob/master/nagoya-museum.jpg)
从那时起，微软就错误地将 Unicode 和宽字符（widechar）作为了 UCS-2 和 UTF-16 的同义词。而且，因为 UTF-8 不能
被设置为窄字符串 WinAPI 的编码，人们必须用 UNICODE 编译代码。Windows C++ 程序员被教育 Unicode 必须和宽字符
一起使用。作为这些混乱的结果，他们现在关于正确处理文本最疑惑的人。

与此同时，在 Linux 和 Web 世界里，有一个默认的共识 UTF-8 是地球上对Unicode 最正确的编码。虽然相对于其它的文
本，这对英语，包括计算机语言（例如 C++，HTML，XML等）有一些偏爱，但在常用的字符集上，它很少比 UTF-16 编码效率
低。
