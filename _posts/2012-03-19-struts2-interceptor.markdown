---
layout: post
title: "Struts2 Interceptor(拦截器)备忘"
categories: JavaEE
date: 2012-03-19 11:17
comments: true
---


> 这个不是给初学struts2的同学看的，至少你需要掌握Action概念及Struts2的基本配置，以及了解什么是拦截器  



###Struts2中拦截器的分类
* 普通拦截器，需要实现**com.opensymphony.xwork2.interceptor.Interceptor**接口  

{% highlight java %}
import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.Interceptor;

public class LoginInterceptor implements Interceptor{

	private static final long serialVersionUID = 1L;

	public void destroy() {
		
	}

	public void init() {
		
	}

	public String intercept(ActionInvocation invocation) throws Exception {
		return null;
	}
}
{% endhighlight %}
<!-- more -->
其中**init**方法用于拦截器进行初始化操作时调用，**destroy**方法用于拦截器销毁时调用，**intercept**方法则是拦截器的核心方法。  
在大部分的情况下，我们不需要实现**init**方法以及**destroy**方法，那么此时只需要让拦截器类继承自**com.opensymphony.xwork2.interceptor.AbstractInterceptor**类，该类使用适配器模式对**init**方法和**destroy**方法进行了空实现，具体可以查看该类的源代码。  

{% highlight java %}
import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.AbstractInterceptor;

public class LoginInterceptor extends AbstractInterceptor{

	private static final long serialVersionUID = 1L;

	@Override
	public String intercept(ActionInvocation invocation) throws Exception {
		return null;
	}
}
{% endhighlight %}

* 方法拦截器  
普通的拦截器对一个*Action*中所有的业务方法都会进行拦截，如果我们需要仅对*Action*中某些方法进行拦截，或者说某些方法排除在拦截范围之外，此时就需要让自己的拦截器继承自**com.opensymphony.xwork2.interceptor.MethodFilterInterceptor**这个抽象类  
{% highlight java %}  
import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.MethodFilterInterceptor;

public class LoginInterceptor extends MethodFilterInterceptor{

	private static final long serialVersionUID = 1L;

	@Override
	protected String doIntercept(ActionInvocation invocation) throws Exception {
		return null;
	}

}
{% endhighlight %}
方法拦截器中核心方法就是**doIntercept**方法，在该方法中去实现拦截器的业务逻辑代码。方法拦截器需要在使用时通过配置文件来实现哪些方法拦截哪些方法不拦截。  

{% highlight xml %}
<interceptor-ref name="loginInterceptor">
	<param name="excludeMethods">execute</param>
	<param name="includeMethods">login</param>
</interceptor-ref>
{% endhighlight %}
**excludeMethods**属性用来声明哪些方法不被拦截，**includeMethod**属性用来声明哪些方法被拦截，多个方法之间使用英文的逗号进行分割。  
###拦截器的配置
{% highlight xml %}
<package name="mystruts" extends="struts-default">
	<interceptors>
		<interceptor name="loginInterceptor" class="com.kaishengit.web.interceptor.LoginInteceptor"/>
	</interceptors>
		...
</package>
{% endhighlight %}
###拦截器使用
* 在指定的Action中使用  
{% highlight xml %}
<action name="login" class="com.kaishengit.web.LoginAction" method="login">
	<result>suc.jsp</result>
	<interceptor-ref name="loginInterceptor"/>
</action>
{% endhighlight %}
当Action中配置拦截器后，**package中**默认的**defaultStack**拦截器栈将无法使用，所以必须在引入自定义拦截器的同时也将默认**defaultStack**引入到Action中，正确做法如下：  
{% highlight xml %}
<action name="login" class="com.kaishengit.web.LoginAction" method="login">
	<result>suc.jsp</result>
	<interceptor-ref name="loginInterceptor"/>
	<interceptor-ref name="defaultStack"/>
</action>
{% endhighlight %}
* 在package中所有的Action中去使用
在每个Action中引入拦截器是个很啰嗦的事，所以可以通过更改**package**默认拦截器的方式来让每个Action都应用到。  
{% highlight xml %}
<package name="mystruts" extends="struts-default">
	<interceptors>
		<interceptor name="loginInterceptor" class="com.kaishengit.web.interceptor.LoginInteceptor"/>
		<interceptor-stack name="mystrack">
			<interceptor-ref name="loginInterceptor"/>
			<interceptor-ref name="defaultStrack"/>
		</interceptor-stack>
	</interceptors>
	<default-interceptor-ref name="mystrack"/>
	…
</package>
{% endhighlight %}
###拦截器实例
#####自定义Timer拦截器
{% highlight java %}
public class MyTimerInterceptor extends AbstractInterceptor{

	private static final long serialVersionUID = 1L;

	@Override
	public String intercept(ActionInvocation invocation) throws Exception {
		//获取开始时间
		long startTime = System.currentTimeMillis();
		String result = invocation.invoke();
		//计算耗时
		long time = System.currentTimeMillis() - startTime;
		
		//获取当前拦截的Action在struts.xml中配置的name名称
		String actionName = invocation.getProxy().getActionName();
		//获取当前拦截的Action所执行的方法
		String method = invocation.getProxy().getMethod();
		
		StringBuilder msg = new StringBuilder();
		msg.append(actionName).append("!")
			.append(method).append("执行耗时：")
			.append(time).append("ms");
		System.out.println(msg);
		return result;
	}

}
{% endhighlight %} 
######登录验证拦截器  

{% highlight java %}
public class LoginInterceptor extends AbstractInterceptor{

	private static final long serialVersionUID = 1L;

	private Set<String> excludeActionSet;
	
	@Override
	public String intercept(ActionInvocation invocation) throws Exception {
		//获取当前执行的Action的name
		String name = invocation.getProxy().getActionName();

		if(excludeActionSet.contains(name)) {
			//如果不属于拦截的Action，直接放行
			return invocation.invoke();
		} else {
			//属于拦截的Action，检查Session中是否有User对象
			Map<String,Object> session = invocation.getInvocationContext().getContext().getSession();
			User user = (User) session.get("session_in_user");
			if(user == null) {
				return Action.LOGIN;
			} else {
				return invocation.invoke();
			}
		}
	}
	/**
	 * 该方法由struts2自动调用，用来配置哪些Action不被该拦截器拦截
	 * @param excludeActions
	 */
	public void setExcludeActions(String excludeActions) {
		//将一个字符串根据逗号分割，返回set集合
		excludeActionSet = TextParseUtil.commaDelimitedStringToSet(excludeActions);
	}

}
{% endhighlight %} 
使用方式
{% highlight xml %}
<interceptors>
	<interceptor name="loginInterceptor" class="com.kaishengit.web.interceptor.LoginInteceptor"/>
	
	<interceptor-stack name="mystrack">
		<interceptor-ref name="loginInterceptor">
			<!-- 配置名称为index和login的Action不被拦截 -->
			<param name="excludeActions">index,login</param>
		</interceptor-ref>
		<interceptor-ref name="defaultStrack"/>
	</interceptor-stack>
</interceptors>
{% endhighlight %}
##其他
* 获取拦截的Action的完全限定名  
{% highlight java %}
invocation.getAction().getClass().getName()；
{% endhighlight %}
