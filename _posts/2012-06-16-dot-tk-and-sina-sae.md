---
layout: post
title: 使用dot.tk和Sina SAE服务搭建自己的网站
date: 2012-06-16 11:39
comments: true
categories: SAE,云,互联网
---
学习完了前端开发课程(HTML,CSS,JavaScript)或者自己开发了一个互联网项目，总是想把自己的作品放到互联网上让自己的亲朋好友去膜拜下。那么这个时候就需要将作品发布到互联网中，大概需要这样几个步骤： 

1. 申请域名 
2. 购买服务器 
3. 配置域名的DNS

首先从购买域名开始，在国内靠谱的就是[万网](http://www.net.cn),但是不得不说价格有些贵。所以推荐国外的[GoDaddy](www.godaddy.com),这个网站服务不错，价格便宜，还支持支付宝付款。但是因为种种原因，大家不想掏银子购买域名，那么就可以申请免费的域名，这些服务互联网上也有很多，但是大部分都是[二级域名](http://baike.baidu.com/view/263945.htm),但是国外的[dot.tk](http://www.dot.tk)提供了免费的顶级域名的注册，但是后缀必须是.tk。在dot.tk中注册一个账户，然后登陆，选择Domain Panel中的Add a Domain就可以看到这样的界面：  
![add a domain](http://ww2.sinaimg.cn/mw690/62772de6tw1dtzuza8wklj.jpg)  
在文本框中输入自己喜欢的域名，如果没有被注册，那就点击add new domain，选择free Domain，Next,然后看到下面的界面：  
![domain setting](http://ww3.sinaimg.cn/mw690/62772de6tw1dtzv49q8k1j.jpg)
其中途中的第一步，请选择**Use DNS for this domain**中的**Use my own DNS Service**,那么这个四个框中填什么值呢？IP Address下面的两个框中不用填写。在Nameserver中填入自己的DNS服务器地址，那怎么拥有免费的DNS服务器呢？请到[DnsPod](http://www.dnspod.cn)注册一个账号，并在DNSPod中添加域名，那么就可以使用DNSPod提供的两个免费的域名服务器**f1g1ns1.dnspod.net**和**f1g1ns2.dnspod.net**，将这两个值分别填入Nameserver的两个框中。第二步是选择注册该域名的时间长度，最多是12个月。第三步不用说了吧，输入正确的验证码，OK，域名的注册部分就结束了。

接下来就是服务器的购买了，可供大家选择的就是虚拟空间或VPS。但是我们今天使用的是Sina的云服务[SAE](http://sae.sina.com.cn)，每个刚注册SAE的用户都可以免费获得500云豆，这些云豆用于支付SAE服务器消耗的费用，如果用完，那就要向Sina购买了。注册登录SAE，点击创建新应用，看到如下界面：
![SAE](http://ww2.sinaimg.cn/mw690/62772de6tw1dtzvgixshvj.jpg)
输入应用的二级域名和应用名称，开发语言选择PHP或Python或Java(Python和Java需要有SAE的邀请码才可以使用)，点击创建应用。创建完成后，进入应用，点击左侧菜单中的**代码管理**，上传你的代码或[使用SVN部署你的应用](http://sae.sina.com.cn/?m=devcenter&catId=212)，代码部署完成后，就可以通过你刚刚在SAE中申请的二级域名(例如xxx.sinaapp.com)来访问你的应用了。

最后，要将申请到的免费域名绑定到SAE中，这样就不再需要使用Sina的二级域名访问了。在SAE中选择**应用设置**菜单项，点击**独立域名设置**中的**新增**，把你刚刚在dot.tk中申请到的域名进行添加。
![SAE domain](http://ww3.sinaimg.cn/mw690/62772de6tw1dtzvqt18lxj.jpg)

此时会看到类似下面的提示框，让在域名中添加CNAME配置和A记录配置

![SAE domain](http://ww1.sinaimg.cn/mw690/62772de6tw1dtzvua9h0cj.jpg)

现在回到DNSPod的域名设置面板中，添加对应的记录
![dospod setting](http://ww2.sinaimg.cn/mw690/62772de6tw1dtzvxt59tqj.jpg)

OK，现在你就可以通过在dot.tk网站中申请的免费域名来访问你在SAE中部署的应用了！最后还是要说下，因为国内的特殊环境，域名需要备案才能使用。但是SAE使用在日本的反向代理，才可以让未备案的域名在国内可以正常使用，不过偶尔会出现502错误，这是因为GFW在作怪，正常情况，稍后片刻就好了。

同样感谢dot.tk和DNSPod,以及SAE为我们提供的服务！























