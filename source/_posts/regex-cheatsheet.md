---
title: regex cheatsheet
date: 2021-01-01 12:43:50
tags:
cover: /img/regex.png

---
## regex anchors
```sh
# ^ = the start of the string
# $ = the end of the string
# * = zero or more
# + = one or more
# . = match any single character: letter, number, whitespace
# \ = match metacharacters by escaping them
```
## python `re`
- `re.compile(patterns, flags)` 
	编译正则表达式，保存以便以后使用，避免重复编写
设置参数flags如下，可多选`flags=re.M|re.S`
	1. `re.I` 忽略 __大小写__
	2. `re.M` 多行模式, __换行__ 重新开始匹配
	3. `re.S` .号可匹配一切字符包括 __换行符__
	4. `re.DOTALL` 效果同`re.S` 适用于 __跨行匹配__ 的情况
---
- `re.match(pattern, string , flags)` 
	从字符串的 __开头__ 匹配，返回`一个`匹配的对象，失败返回`None`，常用于整句匹配（从头匹配）
	当匹配成功时返回一个对象：
	1. `group(int)` 返回分组 __匹配__ 字符串，`group(0)`返回匹配 __整个__ 字串
	2. `start(group)` 返回 __某个__ 分组的字符索引 __开始__ 位置
	3. `end(group)` 返回 __某个__ 分组的字符索引 __结束__ 位置
	4. `span(group)` 返回某个分组的 __(开始位置，结束位置__
	```py
	#match从头整句匹配
	line = '正则表达式语法很easy,我爱正则表达式'
	regex = re.compile('(正则表达式语法很easy),(.*)')
	match_object = re.match(regex,line)
	print(match_object.group(1),match_object.group(2))
	>>> 正则表达式语法很easy，我爱正则表达式

	#如果开头第一个字符无法匹配，则匹配失败
	line = '加入我是开头，正则表达式语法很easy,我爱正则表达式'
	re.match(regex,line)
	>>> None

	```
---

- `re.search(pattern, string, flags)` 
	扫描整个字符串，直到找到第一个匹配的对象（查找）,
	在 __string中__ 进行搜索，成功返回`Match object`, 失败返回`None`, 只匹配 __一个__	
e.g. 
	```py
	print(re.search(r"run\\", "runs\ to me"))
	>>> <_sre.SRE_MATCH object; span=(0,5), match='runs\\'>
	```
- match anything except n
	```py
	print(re.search(r"r.n", "r[ns to me]"))
	>>> <_sre.SRE_MATCH object; span=(0,3), match='r[n'>	

	#search相比match，并不需要开头每个字符扫描匹配
	regex = re.search('正则表达式',line)
	search_object = re.match(regex,line)
	print(search_object.group(1),search_object .group(2))
	>>> 正则表达式语法很easy，我爱正则表达式

	```	
---
- `re.findall(pos[string开始位置:string结束位置])` 
	扫描整个字符串，找到所有匹配的对象并返回`List`（查找所有）
	```py
	line = '我的知乎主页是:https:/zhuanlan.zhihu.com/p/119/110'
	print(re.findall(r'\d+',line))
	>>> ['119','110']
	```
---
- `re.split(pattern, string, maxsplit, flags)` 
	匹配的子串来分割字符串，返回`List`，`maxsplit`设置分隔次数（分隔）
	```py 
	line = '我的知乎主页是:https:/zhuanlan.zhihu.com/p/119/110'
	print(re.split(r'//',line))
	>>> ['我的知乎主页是:https:','zhuanlan.zhihu.com','p','119','110']

	```
---
- `re.sub(pattern, repl, string, count)` 
	将`pattern`替换`repl`，类似文本编辑的 __替换__ ，`count`设置替换 __次数__ ，返回替换后字符串，其中repl可设置为一个（替换）
	```py
	line = '我的知乎主页是:https:/zhuanlan.zhihu.com/p/119/110'
	print(re.sub(r'bihu','zhihu',line))
	>>> '我的知乎主页是:https:/zhuanlan.bihu.com/p/119/110'
	```
---
- `re.finditer()`
	在string中查找所有 匹配成功的字符串, 返回iterator，每个item是一个Match object。

## 规则类(Qualifiers)

- `^` 开头符
- `$` 结尾符
- `|` 或运算
- `+` 1+次
- `*` 0+次
- `?` 非贪心字符，0和1次
- 搭配使用 `*？`或`+？`表示非贪心，第一次匹配上就停止，0909090用09+?匹配到09，09+则匹配到- `090909
- `{m,n}` 匹配m到n次，`{m,}`匹配m+，`{,n}`匹配0~n次

## 多重匹配 (Matching Multiple)

- `[ ]` 匹配括号内字符
- `[a-z]` - 代表a到z的所有字符
- `[+*()]`匹配符号时不用加`\`
- `[^ab]`  `^`代表取反，不代表开头


## 分组获取与顺序要求 (Look Ahead, Look Behind)
- `( )` 匹配括号内的表达式 ，`？`代表特殊含义
- `(?:A)` 匹配但无法获取？
- `(?#...)` 给匹配给注释说明
- `A(?=B)` 匹配A，A在B前
- `A(?!B)` 匹配A，不在B前的A
- `(?<=A)B` 匹配B，B在A后
- `(?<!A)B` 匹配B，B不在A后
- `（...）\1` 代表第一个Group

```py
line = '前中后'
#"中"前的字
print(re.search(r'\w(?=中)',line).group(0))
>>> 前

#不在"中"前的字
print(re.search(r'\w(?!中)',line).group(0))
>>> 中

#"中"后的字
print(re.search(r'(?<=中)\w',line).group(0))
>>> 后

#不在"中"后的字
print(re.search(r'(?!=中)\w',line).group(0))
>>> 前
```