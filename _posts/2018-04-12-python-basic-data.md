---
layout: post
title: python基础-数据类型 
date: 2018-04-12
categories:
- python
tags: [python, 数据]
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  views: '2'
author:
  login: dongliang
  email: dongliang5250@gmail.com
  display_name: dongliang
---
# 字符串  
## 字符串输出  
~~~python
name = 'xiaoming'  
print('--------------------------------------------------')  
print("姓名：%s"%name)  
~~~
## 字符串输入  
~~~python
userName = input('请输入用户名:')  
print("用户名为：%s"%userName)  
~~~
## 字符串常见操作  
~~~python
name = 'abcdef'  
print(name[0:3]) 	# 取 下标0~2 的字符  
print(name[2:]) 	# 取 下标为2开始到最后的字符  
print(name[1:-1]) 	# 取 下标为1开始 到 最后第2个  之间的字符  
print(name[1:5:2]) 	# 取 下标为1开始到5  间隔两个取一次  
~~~

`a = 'hello world ha ni ge da tou gui'`

**检测 str 是否包含在 a中，如果是返回开始的索引值，否则返回-1**  
`a.find(str, start=0, end=len(a))`

**跟find()方法一样，只不过如果str不在 a中会报一个异常**  
`a.index(str, start=0, end=len(a))`

**返回 str在start和end之间 在 a里面出现的次数**  
`a.count(str, start=0, end=len(a))`

**把 a 中的 str1 替换成 str2,如果 count 指定，则替换不超过 count 次**  
`a.replace(str1, str2,  a.count(str1))`

**以 str 为分隔符切片 a，如果 maxsplit有指定值，则仅分隔 maxsplit 个子字符串**  
`a.split(str=" ", 2) `

**把字符串的第一个字符大写**  
`a.capitalize()`

**把字符串的每个单词首字母大写**  
`a.title()`

**检查字符串是否是以 obj 开头\结束, 是则返回 True，否则返回 False**  
`a.startswith(obj)`  `a.endswith(obj)`

**转换 a 中所有大写字符为小/大写**  
`a.lower()`  `a.upper()`  

**返回一个原字符串左\右\居中对齐,并使用空格填充至长度 width 的新字符串**  
`a.ljust(width)`  `a.rjust(width)`  `a.center(width) ` 

**删除 a 字符串末尾的空白字符**   
`a.rstrip()`  
 
**删除a字符串两端的空白字符**    
`a.strip()`  

**类似于 find()函数，不过是从右边开始查找.**    
`a.rfind(str, start=0,end=len(mystr) )`

**把mystr以str分割成三部分,str前，str和str后**    
`mystr.partition(str)`

**类似于 partition()函数,不过是从右边开始.**     
`mystr.rpartition(str)`

**按照行分隔，返回一个包含各行作为元素的列表**      
`mystr.splitlines()`

**如果 mystr 所有字符都是字母\数字\空格 则返回 True,否则返回 False**      
`mystr.isalpha()`  `mystr.isdigit()`  `mystr.isspace()`

**mystr 中每个字符后面插入str,构造出一个新的字符串**      
`mystr.join(str)`  

# 列表  
**列表循环**    
~~~python
namesList = ['xiaoWang','xiaoZhang','xiaoHua']
for name in namesList:
print(name)
~~~
~~~python
i = 0
while i<len(nameList):
    print(namesList[i])
    i+=1
~~~
**添加元素("增"append, extend, insert)**   
通过append可以向列表添加元素 
`A.append(temp)`   
通过extend可以将另一个集合中的元素逐一添加到列表中  
`a = [1, 2] b = [3, 4] a.append(b)`  
insert(index, object) 在指定位置index前插入元素object  
`a.insert(1, 3)`    

**修改元素("改")**      
~~~python
A = ['xiaoWang','xiaoZhang','xiaoHua']

print("-----修改之前，列表A的数据-----")
for tempName in A:
    print(tempName)

A[1] = 'xiaoLu'
print("-----修改之后，列表A的数据-----")
for tempName in A:
    print(tempName)
~~~

**查找元素("查"in, not in, index, count)**    
python中查找的常用方法为：
	in（存在）,如果存在那么结果为true，否则为false
	not in（不存在），如果不存在那么结果为true，否则false
index和count与字符串中的用法相同

**删除元素("删"del, pop, remove)**    
列表元素的常用删除方法有：
	del：根据下标进行删除
	pop：删除最后一个元素
	remove：根据元素的值进行删除

**排序(sort, reverse)**    
sort方法是将list按特定顺序重新排列，默认为由小到大，参数reverse=True可改为倒序，由大到小。
reverse方法是将list逆置。
`a.sort(reverse=True)`

# 元组
Python的元组与列表类似，不同之处在于元组的元素不能修改。元组使用小括号，列表使用方括号。
**访问元组**    
`t=('hello',100,3.12)`
`t(0)`  

**修改元组**      
`t(0)='hhhhh'`

**元组的内置函数count, index**      
index和count与字符串和列表中的用法相同

# 字典
**修改元素**    
`info = {'name':'班长', 'id':100, 'sex':'f', 'address':'地球亚洲中国北京'}`  
`info['id'] = 13`  

**添加元素**    
`info['phone'] = 13200001234`

**删除元素**    
`del info['name']`
`del info`
`info.clear()`

**测量字典中，键值对的个数**    
`len(dic)`

**返回一个包含字典所有KEY的列表**    
`dic.keys()`

**返回一个包含字典所有value的列表**    
`dic.values()`

**返回一个包含所有（键，值）元祖的列表**    
`dic.items()`
