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

### MySQL-python中间件的介绍与安装：
1. 如果是在类unix系统上，直接进入虚拟环境，输入`sudo pip install mysql-python`。
2. 如果是在windows系统上，那么在这里下载`http://www.lfd.uci.edu/~gohlke/pythonlibs/#mysql-python`下载`MySQL_python‑1.2.5‑cp27‑none‑win_amd64.whl`，然后在命令行中，进入到`MySQL_python‑1.2.5‑cp27‑none‑win_amd64.whl`所在的目录，输入以下命令进行安装：
    ```
    pip install MySQL_python‑1.2.5‑cp27‑none‑win_amd64.whl
    ```

### Flask-SQLAlchemy的介绍与安装：
1. ORM：Object Relationship Mapping（模型关系映射）。
2. flask-sqlalchemy是一套ORM框架。
3. ORM的好处：可以让我们操作数据库跟操作对象是一样的，非常方便。因为一个表就抽象成一个类，一条数据就抽象成该类的一个对象。
4. 安装`flask-sqlalchemy`：`sudo pip install flask-sqlalchemy`。

### Flask-SQLAlchemy的使用：
1. 初始化和设置数据库配置信息：
    * 使用flask_sqlalchemy中的SQLAlchemy进行初始化：
        ```
        from flask_sqlalchemy import SQLAlchemy
        app = Flask(__name__)
        db = SQLAlchemy(app)
        ```
2. 设置配置信息：在`config.py`文件中添加以下配置信息：
    ```
    # dialect+driver://username:password@host:port/database
    DIALECT = 'mysql'
    DRIVER = 'mysqldb'
    USERNAME = 'root'
    PASSWORD = 'root'
    HOST = '127.0.0.1'
    PORT = '3306'
    DATABASE = 'db_demo1'

    SQLALCHEMY_DATABASE_URI = "{}+{}://{}:{}@{}:{}/{}?charset=utf8".format(DIALECT,DRIVER,USERNAME,PASSWORD,HOST
                                                 ,PORT,DATABASE)

    SQLALCHEMY_TRACK_MODIFICATIONS = False
    ```

3. 在主`app`文件中，添加配置文件：
    ```
    app = Flask(__name__)
    app.config.from_object(config)
    db = SQLAlchemy(app)
    ```
4. 做测试，看有没有问题：
    ```
    db.create_all()
    ```
    如果没有报错，说明配置没有问题，如果有错误，可以根据错误进行修改。

### 使用Flask-SQLAlchemy创建模型与表的映射：
1. 模型需要继承自`db.Model`，然后需要映射到表中的属性，必须写成`db.Column`的数据类型。
2. 数据类型：
    * `db.Integer`代表的是整形.
    * `db.String`代表的是`varchar`，需要指定最长的长度。
    * `db.Text`代表的是`text`。
3. 其他参数：
    * `primary_key`：代表的是将这个字段设置为主键。
    * `autoincrement`：代表的是这个主键为自增长的。
    * `nullable`：代表的是这个字段是否可以为空，默认可以为空，可以将这个值设置为`False`，在数据库中，这个值就不能为空了。
4. 最后需要调用`db.create_all`来将模型真正的创建到数据库中。

### Flask-SQLAlchemy数据的增、删、改、查：
1. 增：
    ```
    # 增加：
    article1 = Article(title='aaa',content='bbb')
    db.session.add(article1)
    # 事务
    db.session.commit()
    ```
2. 查：
    ```
    # 查
    # select * from article where article.title='aaa';
    article1 = Article.query.filter(Article.title == 'aaa').first()
    print 'title:%s' % article1.title
    print 'content:%s' % article1.content
    ```
3. 改：
    ```
    # 改：
    # 1. 先把你要更改的数据查找出来
    article1 = Article.query.filter(Article.title == 'aaa').first()
    # 2. 把这条数据，你需要修改的地方进行修改
    article1.title = 'new title'
    # 3. 做事务的提交
    db.session.commit()
    ```
4. 删：
    ```
    # 删
    # 1. 把需要删除的数据查找出来
    article1 = Article.query.filter(Article.content == 'bbb').first()
    # 2. 把这条数据删除掉
    db.session.delete(article1)
    # 3. 做事务提交
    db.session.commit()
    ```

### Flask-SQLAlchemy外键及其关系：
1. 外键：
    ```
    class User(db.Model):
        __tablename__ = 'user'
        id = db.Column(db.Integer,primary_key=True,autoincrement=True)
        username = db.Column(db.String(100),nullable=False)

    class Article(db.Model):
        __tablename__ = 'article'
        id = db.Column(db.Integer, primary_key=True, autoincrement=True)
        title = db.Column(db.String(100),nullable=False)
        content = db.Column(db.Text,nullable=False)
        author_id = db.Column(db.Integer,db.ForeignKey('user.id'))

        author = db.relationship('User',backref=db.backref('articles'))
    ```
2. `author = db.relationship('User',backref=db.backref('articles'))`解释：
    * 给`Article`这个模型添加一个`author`属性，可以访问这篇文章的作者的数据，像访问普通模型一样。
    * `backref`是定义反向引用，可以通过`User.articles`访问这个模型所写的所有文章。

3. 多对多：
    * 多对多的关系，要通过一个中间表进行关联。
    * 中间表，不能通过`class`的方式实现，只能通过`db.Table`的方式实现。
    * 设置关联：`tags = db.relationship('Tag',secondary=article_tag,backref=db.backref('articles'))`需要使用一个关键字参数`secondary=中间表`来进行关联。
    * 访问和数据添加可以通过以下方式进行操作：
        - 添加数据：
            ```
            article1 = Article(title='aaa')
            article2 = Article(title='bbb')

            tag1 = Tag(name='111')
            tag2 = Tag(name='222')

            article1.tags.append(tag1)
            article1.tags.append(tag2)

            article2.tags.append(tag1)
            article2.tags.append(tag2)

            db.session.add(article1)
            db.session.add(article2)

            db.session.add(tag1)
            db.session.add(tag2)

            db.session.commit()
            ``` 
        - 访问数据：
            ```
            article1 = Article.query.filter(Article.title == 'aaa').first()
            tags = article1.tags
            for tag in tags:
                print tag.name
            ```

### Flask-Script的介绍与安装：
1. Flask-Script：Flask-Script的作用是可以通过命令行的形式来操作Flask。例如通过命令跑一个开发版本的服务器、设置数据库，定时任务等。
2. 安装：首先进入到虚拟环境中，然后`pip install flask-script`来进行安装。
3. 如果直接在主`manage.py`中写命令，那么在终端就只需要`python manage.py command_name`就可以了。
4. 如果把一些命令集中在一个文件中，那么在终端就需要输入一个父命令，比如`python manage.py db init`。
5. 例子：
    ```
    from flask_script import Manager
    from flask_script_demo import app
    from db_scripts import DBManager

    manager = Manager(app)


    # 和数据库相关的操作，我都放在一起

    @manager.command
    def runserver():
        print '服务器跑起来了!!!!!'
    manager.add_command('db',DBManager)

    if __name__ == '__main__':
        manager.run()
    ```
6. 有子命令的例子：
    ```
    #encoding: utf-8

    from flask_script import Manager

    DBManager = Manager()

    @DBManager.command
    def init():
        print '数据库初始化完成'

    @DBManager.command
    def migrate():
        print '数据表迁移成功'
    ```

### 分开`models`以及解决循环引用：
1. 分开models的目的：为了让代码更加方便的管理。
2. 如何解决循环引用：把`db`放在一个单独的文件中，切断循环引用的线条就可以了。


### Flask-Migrate的介绍与安装：
1. 介绍：因为采用`db.create_all`在后期修改字段的时候，不会自动的映射到数据库中，必须删除表，然后重新运行`db.craete_all`才会重新映射，这样不符合我们的需求。因此flask-migrate就是为了解决这个问题，她可以在每次修改模型后，可以将修改的东西映射到数据库中。
2. 首先进入到你的虚拟环境中，然后使用`pip install flask-migrate`进行安装就可以了。
3. 使用`flask_migrate`必须借助`flask_scripts`，这个包的`MigrateCommand`中包含了所有和数据库相关的命令。
4. `flask_migrate`相关的命令：
    * `python manage.py db init`：初始化一个迁移脚本的环境，只需要执行一次。
    * `python manage.py db migrate`：将模型生成迁移文件，只要模型更改了，就需要执行一遍这个命令。
    * `python manage.py db upgrade`：将迁移文件真正的映射到数据库中。每次运行了`migrate`命令后，就记得要运行这个命令。
5. 注意点：需要将你想要映射到数据库中的模型，都要导入到`manage.py`文件中，如果没有导入进去，就不会映射到数据库中。
6. `manage.py`的相关代码：
    ```
    from flask_script import Manager
    from migrate_demo import app
    from flask_migrate import Migrate,MigrateCommand
    from exts import db
    from models import Article

    # init
    # migrate
    # upgrade
    # 模型  ->  迁移文件  ->  表

    manager = Manager(app)

    # 1. 要使用flask_migrate，必须绑定app和db
    migrate = Migrate(app,db)

    # 2. 把MigrateCommand命令添加到manager中
    manager.add_command('db',MigrateCommand)

    if __name__ == '__main__':
        manager.run()
    ```

