---
title: xml grammar
date: 2021-01-01 12:44:42
tags: [xml]
description: Notes for xml when writing the parser
cover: /img/xmllogo.png
---
###  xml 属性值必须加引号
与 HTML 类似，XML 元素也可拥有属性（名称/值的对）。
在 XML 中，XML 的属性值必须加引号。
请研究下面的两个 XML 文档。 
- 第一个是错误的，
	```xml
	<note date=12/11/2007>
	<to>Tove</to>
	<from>Jani</from>
	</note>	
    ```
- 第二个是正确的：
	```xml
	<note date="12/11/2007">
	<to>Tove</to>
	<from>Jani</from>
	</note>
	```
### xml 实体引用(Entity)
在 XML 中，一些字符拥有特殊的意义。
如果您把字符 "<" 放在 XML 元素中，会发生错误，这是因为解析器会把它当作新元素的开始。
这样会产生 XML 错误：
```xml
<message>if salary < 1000 then</message>
```
为了避免这个错误，请用实体引用来代替 "<" 字符：
```xml
<message>if salary &lt; 1000 then</message>
```
在 XML 中，有 5 个预定义的实体引用：
| Entity References | Character | meaning | 
|------------|------|---------|
| `&lt;`     | <    | less than      |
| `&gt;`     | >    | greater than   |
| `&amp;`    | &    | ampersand      |
| `&apos;`   | '    | apostrophe     |
| `&quot;`   | "    | quotation mark |

注释：在 XML 中，只有字符 "<" 和 "&" 确实是非法的。大于号是合法的，但是用实体引用来代替它是一个好习惯。
### xml comments
和html是一样的
```xml
<!-- This is a comment -->
```

### xml 以 LF 存储换行
- 在 Windows 应用程序中，换行通常以一对字符来存储：回车符（CR）和换行符（LF）
- 在 Unix 和 Mac OSX 中，使用 LF 来存储新行
- 在旧的 Mac 系统中，使用 CR 来存储新行。
XML 以 LF 存储换行
### xml一个元素多个属性
一个元素可以有多个属性，它的基本格式为：
```xml
<元素名 属性名1="属性值1" 属性名2="属性值2">
```
### xml属性值分隔
属性值用双引号 " 或单引号 ' 分隔，
- 如果属性值中有单引号，则用双引号分隔
- 如果有双引号，则用单引号分隔
- 属性值不能包括 <,>,&，如果一定要包含，也要使用实体
- 那么如果属性值中既有单引号还有双引号怎么办？这种要使用实体（转义字符，类似于html中的空格符），XML 有 5 个预定义的实体字符，如下：
  
| xml grammar| mark | meaning | 
|------------|------|---------|
| `&lt;`     | <    | less than      |
| `&gt;`     | >    | greater than   |
| `&amp;`    | &    | ampersand      |
| `&apos;`   | '    | apostrophe     |
| `&quot;`   | "    | quotation mark |

### xml `attribute` vs `element`
In the first example, sex is an attribute. 
Attributes provide extra information about elements.
```xml
<person sex="female">
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
</person>
```
In the last, sex is a child element. 
```xml
<person>
  <sex>female</sex>
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
</person>
```
Both examples provide the same information. There are no rules about when to use attributes, and when to use child elements. My experience is that attributes are handy in HTML, but in XML you should try to avoid them. Use child elements if the information feels like data.

### xml `PCDATA` vs `CDATA`
#### `PCDATA`
- PCDATA means parsed character data.
- PCDATA is text that WILL be parsed by a parser. The text will be examined by the parser for entities and markup.
  
#### `CDATA`
- CDATA means character data.
- CDATA is text that will NOT be parsed by a parser. 