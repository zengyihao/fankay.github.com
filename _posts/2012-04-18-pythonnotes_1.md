---
layout: post
title: "Python学习笔记一"
date: 2012-04-18 12:24
comments: true
categories: Python
---

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
{% highlight %}
for stu in student:
	print(str)
//or
index = 0
while index < len(student)
	print(student[index])
	index = index + 1
{% endhighlight %}
#####提示
列表中可以混合存放多种数据类型的数据，例如：
{% highlight %}
student = ["Tom",23,"Jerry",22,"Rose",18]
{% endhighlight %}

(未完)




