---
layout: post
title: Python学习笔记(二):函数
date: 2012-04-20 9:22
comments: true
categories: Python
---

!["Python"](http://www.python.org/images/python-logo.gif) 

####函数中的参数
定义一个函数，在函数中打印N(参数)次“This is Python”，示例如下：
{% highlight python %}
def print_msg(num) :
	for n in range(num) :
		print("This is Python")

#函数调用
print_msg(3)
{% endhighlight %}

其中*range()*函数会返回一个迭代器，生成一个从0到某个数(不包含该数)的数字列表

####参数中的默认值
上面的打印函数中，需要定义默认打印的次数的5次。在调用该函数时可以重新定义打印次数，如果未定义打印次数，则就采用默认的打印次数5，示例如下：
{% highlight python %}
def print_msg(num = 5) :
	for n in range(num) :
		print("This is Python")

#使用默认次数5进行打印
print_msg()
#使用自定次数2进行打印
print_msg(2)
{% endhighlight %}