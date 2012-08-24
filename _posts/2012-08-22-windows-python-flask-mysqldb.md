---
layout: post
title: 在Windows下搭建Flask+MySQLDb开发环境
date: 2012-08-22 12:01
comments: true
categories: Python,Flask
---
##安装Python2.7
到[Python官网](http://www.python.org/getit/)下载2.7的Windows版本，并进行安装，我的Python安装在D:盘，路径为D:/Python27

##安装setuptools

为了支持easy_install等脚本，安装完Python后要安装[setuptools-0.6c11.win32-py2.7.exe](http://pypi.python.org/pypi/setuptools#files)。

然后在Windows的环境变量中添加**PYTHON_HOME**环境变量
{% highlight python %}
PHTHON_HOME= D:/Python27
{% endhighlight %}
在Windows的**PATH**变量中添加
{% highlight python %}
D:\Python27\Scripts
{% endhighlight %}

##安装virtualenv

在控制台中输入命令
{% highlight python %}
C:\pythontest>easy_install virtualenv
Searching for virtualenv
Best match: virtualenv 1.7.2
Processing virtualenv-1.7.2-py2.7.egg
virtualenv 1.7.2 is already the active version in easy-install.pth
Installing virtualenv-script.py script to D:\Python27\Scripts
Installing virtualenv.exe script to D:\Python27\Scripts
Installing virtualenv.exe.manifest script to D:\Python27\Scripts
Installing virtualenv-2.7-script.py script to D:\Python27\Scripts
Installing virtualenv-2.7.exe script to D:\Python27\Scripts
Installing virtualenv-2.7.exe.manifest script to D:\Python27\Scripts

Using d:\python27\lib\site-packages\virtualenv-1.7.2-py2.7.egg
Processing dependencies for virtualenv
Finished processing dependencies for virtualenv
{% endhighlight %}

##安装Flask
* 创建虚拟环境
{% highlight python %}
C:\pythontest>virtualenv env
New python executable in env\Scripts\python.exe
Installing setuptools................done.
Installing pip...................done.
#使用虚拟环境
C:\pythontest\env\Scripts>activate
(env) C:\pythontest\env\Scripts>
{% endhighlight %}
* 安装Flask
{% highlight python %}
(env) C:\pythontest\env\Scripts>easy_install Flask
{% endhighlight %}

##写一个测试应用
创建app.py文件并编写以下代码：
{% highlight python %}
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello_world():
	return "Hello,Flask and kaishengit!"

if __name__ == '__main__':
	app.run()
{% endhighlight %}
运行：
{% highlight python %}
C:\pythontest>python app.py
{% endhighlight %}
在浏览器中输入[http://127.0.0.1:5000](http://127.0.0.1)进行测试

##安装MySQLdb
下载[MySQL-python-1.2.3.win32-py2.7.exe](https://soemin.googlecode.com/files/MySQL-python-1.2.3.win32-py2.7.exe)并安装。

安装后会自动D:/Python27/Lib/site-packages中加入以下文件夹和文件：

* MySQL_python-1.2.3-py2.7.egg-info
* MySQLdb
* _mysql.pyd
* _mysql_exceptions.py
* _mysql_exceptions.pyc
* _mysql_exceptions.pyo

将上述文件夹和文件放到C:\pythontest\env\Lib\site-packages文件夹中即可

##连接数据库测试
{% highlight python %}
import MySQLdb as db

con = None

try:
	con = db.connect("localhost","root","root","mydb")

	cur = con.cursor()
	cur.execute("select now()")

	data = cur.fetchone()

	print "Now is %s" % data
except db.Error,e:
	print "Error %d: %s" % (e.args[0],e.args[1])
finally:
	if con:
		con.close()	
{% endhighlight %}

