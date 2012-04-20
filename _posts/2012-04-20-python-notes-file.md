---
layout: post
title: Python学习笔记(三):读文件与异常处理
date: 2012-04-20 9:24
comments: true
categories: Python
---

!["Python"](http://www.python.org/images/python-logo.gif) 

####逐行读取文件
Python中提供了*open(filename)*函数用于读取磁盘上的文件,示例代码：
{% highlight python %}
data = open("data.txt")
{% endhighlight %}
将文件读取后赋给变量data，然后通过Python内置的函数*readline()*来读取文件中的一行数据。
{% highlight python %}
#读取第一行文本
line_txt = data.readline()
#读取第二行文本
next_txt = data.readline()
{% endhighlight %}
需要要注意的是，文件读取完毕后，一定要调用*close()*方法来关闭文件。
{% highlight python %}
data.close()
{% endhighlight %}

####利用循环来读取文件
{% highlight python %}
data = open("data.txt")
for line in data :
	print(line)
data.close()
{% endhighlight %}

####读取不存在的文件
如果使用Python来读取一个不存在的文件，则会引发**IOError**异常，Python中采用了和Java中一样的异常处理机制来处理异常。
{% highlight python %}
try :
	data = open("data.txt")
	for line in data :
		print(line)
	data.close()
except :
	pass
{% endhighlight %}
上面的**pass**关键字表示什么都不做，相当与Java中的空或null，如果需要在命令行中输出提示，可以这样：
{% highlight python %}
try :
	data = open("data.txt")
	for line in data :
		print(line)
	data.close()
except :
	print("can't find this file")
{% endhighlight %}
使用**try:…except:…**语法可以处理所有的异常，如果需要指定处理某个异常，例如上面例子中找不到文件的异常IOError，则可以在**except**语句中指出：
{% highlight python %}
try :
	data = open("data.txt")
	for line in data :
		print(line)
	data.close()
except IOError:
	print("can't find this file")
{% endhighlight %}

####其他知识点
1. 通过*a,b = 12,34*可以同时给变量a和变量b赋值，a的值为12，b的值为34
2. 在文件的读的过程中调用*seek()*函数可以让文件回到第一行的位置重新读取
3. 字符串的*split(sep)*函数可以将一个字符串根据sep分割为列表
4. 字符串的*find(str)*可以在字符串中查找str的下标，如果未找到，则返回-1