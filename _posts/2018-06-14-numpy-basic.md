---
layout: post
title: hello numpy
date: 2018-06-14
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
  login: 
  email: dongliang5250@gmail.com
  display_name: slayer
---
* TOC
{:toc}

### Numpy简介
* 导入Numpy模块
	Numpy是Python的一个很重要的第三方库，很多其他科学计算的第三方库都是以Numpy为基础建立的。
	Numpy的一个重要特性是它的数组计算。
	在使用Numpy之前，我们需要导入numpy包：
	```
	import numpy as np
	```
* 数组上的操作
	```
	a = [1, 2, 3, 4]
	b = [2, 3, 4, 5]
	a = array(a)
	a + 1
	a + b
	a * b
	a ** b
	```
	输出：
	> array([2, 3, 4, 5])
	> array([3, 5, 7, 9])
	> array([2, 6, 12, 20])
	> array([1, 8, 81, 1024])
* 提取数组中的元素
    ```
    # 提取第一个元素、提取前两个元素、提取最后两个元素
    a[0]
    a[:2]
    a[-2:]
    ```
* 修改数组形状
    ```
    a.shape  # (4L,)
    a.shape = 2,2 # array([[1,2],[3,4]])
    ```
* 多维数组
    加法对应相加，乘法对应元素相乘。
* 画图
    linspace用来生成一组等间隔的数据：
    ```
    a = linspace(0, 2*pi, 21)
    print(a)
    b = sin(a)
    print(b)

    plt.figure()
    mask = b >= 0
    plt.plot(a, b)
    plt.plot(a[mask], b[mask], "ro")
    plt.show()
    ```

### Matplotlib基础
* 导入matplotlib模块
    ```
    import matplotlib.pyplot as plt
    ```
* plot二维图
    ```
    x = linspace(0, 2 * pi, 50)
    plot(sin(x))
    # 给定x和y值
    plot(x, sin(x))
    # 多条数据线
    plot(x, sin(x), x, sin(2 * x))
    # 使用字符串给定线条参数
    plot(x, sin(x), 'r-^')
    ```
* scatter 散点图
    ```
    plot(x, sin(x), 'bo')
    # 可以使用 scatter 达到同样的效果
    scatter(x, sin(x))
    # 事实上，scatter函数与Matlab的用法相同，还可以指定它的大小，颜色等参数
    x = rand(200)
    y = rand(200)
    size = rand(200) * 30
    color = rand(200)
    scatter(x, y, size, color)
    # 显示颜色条
    colorbar()
    ```
* 多图
    ```
    t = linspace(0, 2*pi, 50)
    x = sin(t)
    y = cos(t)
    figure()
    plot(x)
    figure()
    plot(y)
    show()
    # 或者
    subplot(1, 2, 1)
    plot(x)
    subplot(1, 2, 2)
    plot(y)
    ```
* 标签
    ```
    plot(x, label='sin')
    plot(y, label='cos')
    legend()
    # 或者
    plot(x)
    plot(y)
    legend(['sin', 'cos'])
    ```
* 坐标轴、标题、网络
    ```
    plot(x, sin(x))
    xlabel('radians')
    # 可以设置字体大小
    ylabel('amplitude', fontsize='large')
    title('Sin(x)')
    # 显示网格
    grid()
    ```



