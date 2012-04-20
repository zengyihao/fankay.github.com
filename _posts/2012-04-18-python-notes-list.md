---
layout: post
title: Python学习笔记(一):列表和函数初步
date: 2012-04-18 17:11
comments: true
categories: Python
---

!["Python"](http://www.python.org/images/python-logo.gif)

####Python的安装和基本使用: 

+ MacOS和Liunx上自带Python
+ Windows中需要自己去安装Python，在[Python](http://www.python.org)的官方网站上可以去下载适合自己操作系统的版本
+ 检测自己的系统中是否有Python环境
{% highlight python %}
python -V
{% endhighlight %}
+ Python安装后自带一个名称为IDLE的编辑器，在命令行中输入*python*即可进入IDLE环境，退出此环境时输入*quit()*

####Python列表
python中的列表的概念有些类似与Java中的数组，写法为：
{% highlight python %}
students = ["Jack","Tom","Jerry","Rose"]
{% endhighlight %}
注意： 

1. Python中的变量不用声明，可以直接使用
2. 列表中各项之间使用逗号分割
3. 通过下标(索引)来获取列表中的值，例如*student\[1\]*
4. *print()*函数用于向控制台中打印内容，例如*print(student\[0\])*

#####列表实例代码
{% highlight python %}
student = ["Jack","Tom","Jerry","Rose"]
print(len(student))
print(student\[1\])
student.append("Alex")
studnet.pop()
print(student)
student.extend(["zhangsan","lisi"])
student.remove("Jack")
student.insert(1,"Hanks")
{% endhighlight %}
代码解释：

1. len()函数用于输出列表的长度
2. append()函数用于在列表的末尾增加一个数据项
3. pop()函数用于删除列表的最后一项
4. extend()函数用于在列表的末尾添加一个数据项集合
5. remove()函数用于删除列表中一个特定项，如果列表中有重复项，则只会删除第一个
6. insert()函数用于在特定的位置前添加一个数据项

#####利用循环迭代列表
{% highlight python %}
for stu in student:
	print(str)
#or
index = 0
while index < len(student):
	print(student[index])
	index = index + 1
{% endhighlight %}

注意：
1. for和while后面的冒号不能省略
1. Python的for、while、if、函数定义等都没有类似Java或C中大括号来定义这些代码块的边界，而是采用缩进的方式来定义代码的边界
2. Python没有index++这种语法

#####列表中可以混合存放多种数据类型的数据
例如：
{% highlight python %}
student = ["Tom",23,"Jerry",22,"Rose",18]
{% endhighlight %}

#####Python的列表中还可以包含其他的列表
{% highlight python %}
student = ["aa","bb","cc",["xxx","yyy","zzz",["heh","xixi"]]]
{% endhighlight %}

可以利用Python中的内置函数*isinstance(name,type)*来判断变量的类型，例如调用*isinstance(student,list)*返回值将为true，list代表就是列表类型。所以在迭代这个数组时，可以使用如下代码：
{% highlight python %}
for item in student :
	if isinstance(item,list) :
		for item_sub in item :
			if isinstance(item_sub,list) :
				for item_sub_sub in item_sub :
					print(item_sub_sub)
			else :
				print(item_sub)
	else :
		print(item)
{% endhighlight %}

看到上面代码基本处于崩溃状态，因为用**递归**可以很好的来解决这个问题，但是递归在使用时需要用到函数，那么就来看下Python中函数的定义：
{% highlight python %}
def show_list(mylist) :
	for item in mylist :
		if isinstance(item,list) :
			show_list(item)
		else :
			print(item)
{% endhighlight %}
其中*def*关键字用于定义函数，类似JavaScript中的*funciton*，show_list为函数名，小括号内是参数列表，该函数调用方法：
{% highlight python %}
show_list(student)
{% endhighlight %}

#####注释
1. Python中多行注释可以以三个双引号(或三个单引号)开始，以三个双引号(单引号)结束  

2. 单行注释使用#开始，以行尾的结束而结束

#####将Python代码写入到文件中
+ Python源代码文件后缀名为.py
+ 在命令行中使用命令*python xxx.py*来运行一个Python文件中的代码
+ Python文件中如果需要使用中文，则需要在文件的第一行写：
{% highlight python %}
\#coding=UTF-8
{% endhighlight %}

Python文件示例：
{% highlight python %}
\#coding=UTF-8
student = ["aa","bb","cc",["xxx","yyy","zzz",["heh","xixi"]]]

'''
这是一个注释
'''
def show_list(mylist) :
	for item in mylist :
		if isinstance(item,list) :
			show_list(item)
		else :
			print(item)

show_list(student)
{% endhighlight %}

