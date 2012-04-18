---
layout: post
title: "在Java中使用Gravatar头像服务"
date: 2012-04-15 11:07
comments: true
categories: JavaEE
---
[Gravatar](http://en.gravatar.com/)是一个从事全球头像服务的网站，其实也就是根据你注册的电子邮件地址来绑定一个头像(当然需要你自己上传)。当你在一个支持Gravatar服务的网站上使用你的电子邮件地址作为注册的邮箱是，该网站就可以自动读取到你在Gravatar网站是设置的头像并显示。这样就保证了你一个头像，全网通用了。对于开发者来说也不需要开发头像上传服务了。只需要让用户在[Gravator](http://en.gravatar.com)上进行设置即可。

在Java的Web项目中使用Gravator也是非常简单的，方式有两种：
####方法1：自己动手  
+ 创建[MD5Util](http://en.gravatar.com/site/implement/images/java/)类
{% highlight java%}
public class MD5Util {
  public static String hex(byte[] array) {
      StringBuffer sb = new StringBuffer();
      for (int i = 0; i < array.length; ++i) {
	  sb.append(Integer.toHexString((array[i]
	      & 0xFF) | 0x100).substring(1,3));        
      }
      return sb.toString();
  }
  public static String md5Hex (String message) {
      try {
	  MessageDigest md = 
	      MessageDigest.getInstance("MD5");
	  return hex (md.digest(message.getBytes("CP1252")));
      } catch (NoSuchAlgorithmException e) {
      } catch (UnsupportedEncodingException e) {
      }
      return null;
  }
}
{% endhighlight %}
+ 使用MD5Util类获取电子邮件的hex码
{% highlight java%}
String result = MD5Util.md5Hex("fankai@kaishengit.com");
{% endhighlight %}
这样就获取了该电子邮件对应的hex编码，将该编码传入的到http://www.gravatar.com/avatar/链接后就可以获取该邮件对应的图片了。例如上述邮件的hex码为8266241e1c2c5460924cf1a512190d2c，那么最终的图片地址就是http://www.gravatar.com/avatar/8266241e1c2c5460924cf1a512190d2c，显示如下：  
![我的头像](http://www.gravatar.com/avatar/8266241e1c2c5460924cf1a512190d2c)

如果该电子邮件未在Gravatar网站上进行注册，则会返回一个默认图片，如下：  
![默认头像](http://www.gravatar.com/avatar/8266241e1c2c5460924cf1a51xxx) 

####方法2：使用JGavatar项目
[jgavatar](https://github.com/ralfebert/jgravatar)提供了一系列的API让我们更方便的使用Gravatar的服务，可以到[Github](https://github.com/ralfebert/jgravatar)上下载。

