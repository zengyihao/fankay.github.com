---
layout: post
title: "使用Dropbox分享文件"
date: 2012-04-10 16:51
comments: true
categories: 互联网
---
[DropBox](https://www.dropbox.com/referrals/NTUyNDE1Mzc3OQ?src=referrals_twitter9)中默认提供了一个名字叫做Public的文件夹，可以利用这个文件来分享这个文件，例如在个人博客中插入图片。  

现在在博客中插入图片要么利用博客的文件上传功能，要么利用第三方的图片网站，但是不少的图片网站对外链的政策是要么收费，要么禁止外链。当前在国内用的比较多的就是新浪微博提供的相册服务，至少当前新浪并没有禁止外链。例如你现在看到下面的这张图片就来自新浪微博的相册：  

!['新浪微博相册中的图片'](http://ww4.sinaimg.cn/mw600/62772de6jw1drunc37pr3j.jpg)

<!-- more -->

除了新浪微博的相册服务，还可以选择Dropbox的Public文件夹来做这样的服务，将一张图片放到Dropbox的Public文件夹中，然后右键该文件，可以获取到该文件的外链地址，格式为：
[http://dl.dropbox.com/u/52415377/it.png](http://dl.dropbox.com/u/52415377/it.png),不出意外的话你点击此超链接不会看到任何图片，因为GFW已经将Dropbox进行认证。那么如何搞定呢，方法有如下两种：  
####使用https协议即可
将上面的图片连接由http改为https，[https://dl.dropbox.com/u/52415377/it.png](https://dl.dropbox.com/u/52415377/it.png)。下面的图片就来自我的Dropbox：  
!['Dropbox相片'](https://dl.dropbox.com/u/52415377/it.png)

#####使用自己域名的CNAME功能
在域名的控制面板中，添加一个CNAME，值为dl,将其指向dl.dropbox.com，例如配置我的域名为dl.fankai.me,这样就可以突破GFW，在不适用https协议的情况下来访问Dropbox中的图片。格式为[http://dl.fankai.me/u/52415377/it.png](http://dl.fankai.me/u/52415377/it.png),其中52415377为你个人账号的ID，每人是不一样的，下面的图片来自CNAME后的显示：
!['CNAME后的图片'](http://dl.fankai.me/u/52415377/it.png)  

使用CNAME后如果在配上[https://www.cloudflare.com](https://www.cloudflare.com)的免费CDN服务，显示图片的速度将加快很多！