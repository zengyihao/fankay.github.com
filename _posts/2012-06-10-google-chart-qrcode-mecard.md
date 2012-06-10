---
layout: post
title: 利用Google Chart服务生成二维码
date: 2012-06-10 17:05
comments: true
categories: 杂谈
---
[google chart](https://developers.google.com/chart/)提供了很多在线生成统计图的API，例如饼图、柱状图等等，今天主要关注的是二维码的生成的API和参数设置等内容。

####Google二维码文档地址
[https://developers.google.com/chart/infographics/docs/qr_codes](https://developers.google.com/chart/infographics/docs/qr_codes)

####请求的API地址及参数
**API地址:** [https://chart.googleapis.com/chart](https://chart.googleapis.com/chart)  
**请求参数:** 

* cht=qr 生成图片的格式为二维码格式
* chs=widthxheight 生成图片的大小。例如要生成300x300的图片，参数值为chs=300x300
* chl=data 二维码中包含的内容，内容必须使用UTF-8格式编码
* choe=output_encoding 非必要参数。输出的内容编码格式，默认为UTF-8
* chld=L|M|Q|H 非必要参数，生成的二维码的容错率

如果想生成一个图片大小为200x200像素，内容为**Hello，world**的二维码，那么请求的URL就是： 
[https://chart.googleapis.com/chart?cht=qr&chld=H&chs=200x200&chl=Hello,world](https://chart.googleapis.com/chart?cht=qr&chld=H&chs=200x200&chl=Hello,world)
生成的二维码效果如下：

![二维码图片](https://chart.googleapis.com/chart?cht=qr&chld=H&chs=200x200&chl=Hello,world)

利用二维码可以做很多事情，比如电子名片、促销的电子优惠券等等，下面就利用二维码来生成一个电子名片。当前电子名片流行的有两种格式，一种是vcard格式和mecard格式，当前使用的格式为mecard。该格式规范如下：  
{% highlight python %}
MECARD:  
N:  Contact name (if comma separated, uses LASTNAME,FIRSTNAME)  
SOUND:  Contact name in Japanese (Kana). Same comma rules as N:  
TEL: Telephone number  
TEL-AV: Videophone Tel number  
EMAIL:  Email address  
NOTE: Memo text field  
BDAY: Birthday (YYYMMDD format)  
ADR: Address.   
URL:  Website  
NICKNAME: Display name  
{% endhighlight %}

根据上面的格式写一个名片：  
{% highlight python %}  
MECARD:
N:汤姆  
TEL:18800000000  
TEL:0391000000  
EMAIL:tom@kaishengit.com  
ADR:河南焦作  
URL:http://www.fankai.me  
NOTE:很好的一个家伙
{% endhighlight %}

将上面的信息发送到google的二维码API中，注意**mecard的多个参数间使用分号隔开**，那么产生的URL就是[https://chart.googleapis.com/chart?cht=qr&chld=H&chs=300x300&chl=MECARD:N:汤姆;TEL:18800000000;TEL:0391000000;EMAIL:tom@kaishengit.com;ADR:河南焦作;URL:http://www.fankai.me;NOTE:很好的一个家伙;](https://chart.googleapis.com/chart?cht=qr&chld=H&chs=300x300&chl=MECARD:N:汤姆;TEL:18800000000;TEL:0391000000;EMAIL:tom@kaishengit.com;ADR:河南焦作;URL:http://www.fankai.me;NOTE:很好的一个家伙;),图片如下:  
![二维码图片](https://chart.googleapis.com/chart?cht=qr&chld=H&chs=300x300&chl=MECARD:N:%E6%B1%A4%E5%A7%86;TEL:18800000000;TEL:0391000000;EMAIL:tom@kaishengit.com;ADR:%E6%B2%B3%E5%8D%97%E7%84%A6%E4%BD%9C;URL:http://www.fankai.me;NOTE:%E5%BE%88%E5%A5%BD%E7%9A%84%E4%B8%80%E4%B8%AA%E5%AE%B6%E4%BC%99;)

那么拿起你的手机，扫描一下吧！