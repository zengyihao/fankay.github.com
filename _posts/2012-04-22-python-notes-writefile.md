---
layout: post
title: Python学习笔记(四):写文件
date: 2012-04-22 11:03
comments: true
categories: Python
---
!["Python"](http://www.python.org/images/python-logo.gif)
####以写模式打开文件
{% highlight python %}
my_file = open("data.txt","w")
{% endhighlight %}
*open()*函数的第二个参数是打开文件的模式，常见模式如下：  
+ w:写模式，该模式下，如果文件中有内容，则会把原有内容清空再写入新的数据
+ a:追加模式，该模式下，新的数据会加入到原有文件的内容中，而不会清空原有的数据
+ w+:读写模式，该模式下，文件可以进行读操作和写操作，而且不会清空原有数据  

注意**如果写的文件在磁盘中不存在，则会先创建该文件再进行写操作**
默认地，*print()*函数会将内容输出到屏幕中，如果希望将内容写到文件中则需要通过*file*参数来指定所使用的文件对象：
{% highlight python %}
my_file = open("data.txt","w")
print("Hi,this is python",file = my_file)
my_file.close()
{% endhighlight %}
完成操作时，务必要调用*close()*函数来关闭文件对象。

####异常异常！
如果在写文件时，磁盘已满或者路径不存在，则会引发**IOError**异常，所以在进行写操作时也要进行**try…except…**方式的来捕获异常：
{% highlight python %}
try :
	my_file = open("data.txt","w")
	my_file2 = open("file.txt","w")
	
	print("Hi,this is python",file = my_file)
	print("xixi",file = myfile2)
	
	my_file.close()
	my_file2.close()
except IOError :
	print("IOError")
{% endhighlight %}
在上面的示例代码中如果my_file2在打开时出现了异常那么就会导致程序直接跳转到except代码块中去执行，这样引起的后果就是文件的*close()*方法没有办法执行了。我们希望的结果是，无论是否出现异常，文件都要安全的关闭，所以Python中也有类似Java中的**finally**代码块：
{% highlight python %}
try :
	my_file = open("data.txt","w")
	my_file2 = open("file.txt","w")
	
	print("Hi,this is python",file = my_file)
	print("xixi",file = myfile2)
except IOError :
	print("IOError")
finally :
	my_file.close()
	my_file2.close()
{% endhighlight %}
在仔细想想，如果my_file2打开时异常，那么就根本无法创建my_file2对象，那么在**finally**代码块中调用myfile2的*close()*函数时会引发**NameError**，所以在关闭时需要判断文件对象是否存在，如果存在，则调用*close()*函数进行关闭。Python内置的函数*locals()*会返回当前作用域中定义的所有变量的集合，所以我们需要判断变量是否存在*locals()*函数返回的集合中即可：
{% highlight python %}
try :
	my_file = open("data.txt","w")
	my_file2 = open("file.txt","w")
	
	print("Hi,this is python",file = my_file)
	print("xixi",file = myfile2)
except IOError :
	print("IOError")
finally :
	if my_file in locals() :
		my_file.close()
	if my_file2 in locals() :
		my_file2.close()
{% endhighlight %}
#####获取异常信息
当出现异常时，代码会跳转到**except**代码块中，但是无法获取错误的详细信息。例如在上面的写文件的代码中，如果*open()*函数调用出现异常，那么是因为磁盘已满？还是没有文件的写入权限(只读文件)? 所以，我们不仅仅需要知道错误的类型，还需要知道错误的详细内容：
{% highlight python %}
try :
	my_file = open("data.txt","w")
	my_file2 = open("file.txt","w")
	
	print("Hi,this is python",file = my_file)
	print("xixi",file = myfile2)
except IOError as err :
	print("IOError:" + str(err))
finally :
	if my_file in locals() :
		my_file.close()
	if my_file2 in locals() :
		my_file2.close()
{% endhighlight %}
注意上面示例代码中的**except**部分，将异常的对象给了变量err，然后在显示错误信息时，将err的内容进行输出。*str()*函数用于将其他类型的数据转换为字符串类型。
####更简单的文件处理方式
对于文件操作时，**try…except…finally**的方式非常的常用，但是不够简洁，Python中提供了*with… as…*语法来简化这样的操作:
{% highlight python %}
try : 
	with open("data.txt","w") as my_file :
		print("Hi,this is python",file = my_file)
	with open("file.txt","w") as my_file2 :
		print("xixi",file = myfile2)
except IOError as err :
	print("IOError:" + str(err))
{% endhighlight %}
使用**with**语法进行操作时，不需要关心文件的关闭，因为Python会自动为我们做这些。上面的代码还可以简化如下：
{% highlight python %}
try : 
	with open("data.txt","w") as my_file,open("file.txt","w") as my_file2 :
		print("Hi,this is python",file = my_file)
		print("xixi",file = myfile2)
except IOError as err :
	print("IOError:" + str(err))
{% endhighlight %}
####以二进制方式来读写数据
Python中提供了*pickle*模块来对文件进行二进制的操作，例如**dump()**函数用于将文本以二进制的方式存储在文件中。**load()**函数用于将二进制文件读取到Python中。
{% highlight python %}
import pickle

try:
	with open("myfile.dat","wb") as myfile :
		pickle.dump("Hi,this is a Python",myfile)
except PickleError as err:
	print("error:" + str(err))
{% endhighlight%}
上面的代码中用到了*pickle*模块，所以我们需要使用关键字*import*将其导入到我们的代码中。*wb*模式代表以二进制的形式进行写操作。读取二进制文件示例如下：
{% highlight python %}
import pickle

try:
	with open("myfile.dat","rb") as myfile :
		content = pickle.load(myfile);
	print(content)
except PickleError as err :
	print("error:" + str(err))
{% endhighlight%}
*rb*代表以二进制模式来读取文件。


