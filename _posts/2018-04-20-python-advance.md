---
layout: post
title: python-装饰器-生成器-迭代器
categories:
- 正经文章
tags: [python]
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  views: '2'
author:
  login: dongliang
  email: dongliang5250@outlook.com
  display_name: dongliang
description: 先这样
---
* TOC
{:toc}
# 生成器
### 1.什么是生成器
通过列表生成式，我们可以直接创建一个列表。但是受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

### 2.创建生成器的方法(1)
把一个列表生成式的[]改为()
即把`L = [ x*2 for x in range(5)]`
改为`G = ( x*2 for x in range(5))`
创建 L 和 G 的区别仅在于最外层的 [ ] 和 ( ) ， L 是一个列表，而 G 是一个生成器。我们可以直接打印出L的每一个元素，通过 next() 函数获得G的下一个返回值

### 3.创建生成器的方法(2)
使用yield关键字
~~~python
def fib(times):
    n = 0
    a,b = 0,1
    while n<times:
        yield b
         a,b = b,a+b
         n+=1
    return 'done'
~~~
用for循环调用generator时，发现拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中。


### 4.yield的使用示例
~~~python
# 多任务执行（可以理解为同时进行）
def test1():
	while True:
		print('--1--')
		yield None
def test2():
	while True:
		print('--2--')
		yield None

t1 = test1()
t2 = test2()
while True:
	t1.__next__()
	t2.__next__()
~~~

### 5.生成器总结
生成器是这样一个函数，它记住上一次返回时在函数体中的位置。对生成器函数的第二次（或第 n 次）调用跳转至该函数中间，而上次调用的所有局部变量都保持不变。
生成器不仅“记住”了它数据状态；生成器还“记住”了它在流控制构造（在命令式编程中，这种构造不只是数据值）中的位置。
生成器的特点：
1、节约内存
2、迭代到下一次的调用时，所使用的参数都是第一次所保留下的，即是说，在整个所有函数调用的参数都是第一次所调用时保留的，而不是新创建的

#  迭代器
### 1.可迭代对象
迭代是访问集合元素的一种方式。迭代器是一个可以记住遍历的位置的对象。迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。
以直接作用于 for 循环的数据类型有以下几种：
一类是集合数据类型，如 list 、 tuple 、 dict 、 set 、 str 等；
一类是 generator ，包括生成器和带 yield 的generator function。
这些可以直接作用于 for 循环的对象统称为可迭代对象： Iterable 。  

### 2.判断是否可以迭代  
使用 isinstance() 判断一个对象是否是 Iterable 对象：  
`isinstance('abc', Iterable)`  

### 3.迭代器
生成器都是 Iterator 对象，但 list 、 dict 、 str 虽然是 Iterable ，却不是 Iterator 。  
把 list 、 dict 、 str 等 Iterable 变成 Iterator 可以使用 iter() 函数：  
`isinstance(iter('abc'), Iterator)`  

### 4.迭代器总结  
1、凡是可作用于 for 循环的对象都是 Iterable 类型。  
2、凡是可作用于 next() 函数的对象都是 Iterator 类型。  
3、集合数据类型如 list 、 dict 、 str 等是 Iterable 但不是 Iterator ，不过可以通过 iter() 函数获得一个 Iterator 对象。  

# 装饰器
~~~python
#定义函数：完成包裹数据
def makeBold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

#定义函数：完成包裹数据
def makeItalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makeBold
def test1():
    return "hello world-1"

@makeItalic
def test2():
    return "hello world-2"

@makeBold
@makeItalic
def test3():
    return "hello world-3"

print(test1()))
print(test2()))
print(test3()))
~~~
运行结果：
~~~html
<b>hello world-1</b>
<i>hello world-2</i>
<b><i>hello world-3</i></b>
~~~

**通用类型的装饰器（有无参数、不定长参数、含return）**
~~~python
def func(functionName):
	def func_in(*args, **kwargs):
		ret = functionName(*args, **kwargs)
		return ret
	return func_in

@func
def test():
	print('--test--')
	return 'haha'

@func
def test2():
	print('---test2-')

@func
def test3(a):
	print('--test3--a=%d--'%a)

ret = test()
print('test return value is %s '%ret)

a = test2()
print('test2 return value is '%a)

test3(11)
~~~
