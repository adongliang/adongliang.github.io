---
layout: post
title: hello flask
date: 2018-05-08
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
  login: slayer
  email: dongyado@gmail.com
  display_name: slayer
---
* TOC
{:toc}

### 第一个flask程序：
1. 第一次创建项目的时候，要添加flask的虚拟环境。添加虚拟环境的时候，一定要选择到python这个执行文件。
比如你的flask的虚拟环境的目录在/User/Virtualenv/flask-env/bin/python。
2. flask程序代码的详细解释：
    ```
    # 从flask这个框架中导入Flask这个类
    from flask import Flask

    # 初始化一个Flask对象
    # Flaks()
    # 需要传递一个参数__name__
    # 1. 方便flask框架去寻找资源
    # 2. 方便flask插件比如Flask-Sqlalchemy出现错误的时候，好去寻找问题所在的位置
    app = Flask(__name__)


    # @app.route是一个装饰器
    # @开头，并且在函数的上面，说明是装饰器
    # 这个装饰器的作用，是做一个url与视图函数的映射
    # 127.0.0.1:5000/   ->  去请求hello_world这个函数，然后将结果返回给浏览器
    @app.route('/')
    def hello_world():
        return '我是第一个flask程序'


    # 如果当前这个文件是作为入口程序运行，那么就执行app.run()
    if __name__ == '__main__':
        # app.run()
        # 启动一个应用服务器，来接受用户的请求
        # while True:
        #   listen()
        app.run()
    ```

### 设置debug模式：
1. 在app.run()中传入一个关键字参数debug,app.run(debug=True)，就设置当前项目为debug模式。
2. debug模式的两大功能：
    * 当程序出现问题的时候，可以在页面中看到错误信息和出错的位置。
    * 只要修改了项目中的`python`文件，程序会自动加载，不需要手动重新启动服务器。

### 使用配置文件：
1. 新建一个`config.py`文件
2. 在主app文件中导入这个文件，并且配置到`app`中，示例代码如下：
    ```
    import config
    app.config.from_object(config)
    ```
3. 还有许多的其他参数，都是放在这个配置文件中，比如`SECRET_KEY`和`SQLALCHEMY`这些配置，都是在这个文件中。

### URL传参数：
1. 参数的作用：可以在相同的URL，但是指定不同的参数，来加载不同的数据。
2. 在flask中如何使用参数：
    ```
    @app.route('/article/<id>')
    def article(id):
        return u'您请求的参数是：%s' % id
    ``` 
    * 参数需要放在两个尖括号中。
    * 视图函数中需要放和url中的参数同名的参数。

### 反转URL：
1. 什么叫做反转URL：从视图函数到url的转换叫做反转url
2. 反转url的用处：
    * 在页面重定向的时候，会使用url反转。
    * 在模板中，也会使用url反转。

### 页面跳转和重定向：
1. 用处：在用户访问一些需要登录的页面的时候，如果用户没有登录，那么可以让她重定向到登录页面。
2. 代码实现：
    ```
    from flask import redirect,url
    redirect(url_for('login'))
    ```  

### cookie：
1. `cookie`出现的原因：在网站中，http请求是无状态的。也就是说即使第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。cookie的出现就是为了解决这个问题，第一次登录后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动的把上次请求存储的cookie数据自动的携带给服务器，服务器通过浏览器携带的数据就能判断当前用户是哪个了。
2. 如果服务器返回了`cookie`给浏览器，那么浏览器下次再请求相同的服务器的时候，就会自动的把`cookie`发送给浏览器，这个过程，用户根本不需要管。
3. `cookie`是保存在浏览器中的，相对的是浏览器。

### session：
1. `session`介绍：session和cookie的作用有点类似，都是为了存储用户相关的信息。不同的是，cookie是存储在本地浏览器，而session存储在服务器。存储在服务器的数据会更加的安全，不容易被窃取。但存储在服务器也有一定的弊端，就是会占用服务器的资源，但现在服务器已经发展至今，一些session信息还是绰绰有余的。
2. 使用`session`的好处：
    * 敏感数据不是直接发送回给浏览器，而是发送回一个`session_id`，服务器将`session_id`和敏感数据做一个映射存储在`session`(在服务器上面)中，更加安全。
    * `session`可以设置过期时间，也从另外一方面，保证了用户的账号安全。

### flask中的session工作机制：
1. flask中的session机制是：把敏感数据经过加密后放入`session`中，然后再把`session`存放到`cookie`中，下次请求的时候，再从浏览器发送过来的`cookie`中读取`session`，然后再从`session`中读取敏感数据，并进行解密，获取最终的用户数据。
2. flask的这种`session`机制，可以节省服务器的开销，因为把所有的信息都存储到了客户端（浏览器）。
3. 安全是相对的，把`session`放到`cookie`中，经过加密，也是比较安全的，这点大家放心使用就可以了。

### 操作session：
1. session的操作方式：
    * 使用`session`需要从`flask`中导入`session`，以后所有和`sessoin`相关的操作都是通过这个变量来的。
    * 使用`session`需要设置`SECRET_KEY`，用来作为加密用的。并且这个`SECRET_KEY`如果每次服务器启动后都变化的话，那么之前的`session`就不能再通过当前这个`SECRET_KEY`进行解密了。
    * 操作`session`的时候，跟操作字典是一样的。
    * 添加`session`：`session['username']`。
    * 删除：`session.pop('username')`或者`del session['username']`。
    * 清除所有`session`：`session.clear()`
    * 获取`session`：`session.get('username')`
2. 设置session的过期时间：
    * 如果没有指定session的过期时间，那么默认是浏览器关闭后就自动结束
    * 如果设置了session的permanent属性为True，那么过期时间是31天。
    * 可以通过给`app.config`设置`PERMANENT_SESSION_LIFETIME`来更改过期时间，这个值的数据类型是`datetime.timedelay`类型。


