---
title: encodeURI 和 encodeURIComponent
categories: 计算机基础
tags: encodeURI、encodeURIComponent
---

|   比较   | encodeURI( )                                                 | encodeURIComponent( )                                        |
| :------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 编码对象 | 不会对本身属于URI的特殊字符进行编码，例如：冒号、正斜杠、问号、井字号等 | 会对发现的任何非标准字符进行编码                             |
| 作用范围 | 可以对整个URI使用                                            | 只能对附加在现有URI后面的字符串使用                          |
|   解码   | decodeURI( )                                                 | decodeURIComponent( )                                        |
|   示例   | "http://www.jxbh.cn/illegal value.htm#start";<br />”http://www.jxbh.cn/illegal%20value .htm#start”<br />除了空格之外的其他字符都原封不动，只有空格被替换成了%20。 | "http://www.jxbh.cn/illegal value.htm#start";<br />”http% 3A%2F%2Fwww.jxbh.cn%2 Fillegal%2 0value. htm%23 start”<br />使用对应的编码替换所有非字母数字字符 |

