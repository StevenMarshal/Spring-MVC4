## 3.5 SpringMVC对restful风格的支持

### 3.5.1 Restful的概念

Restful风格的API是一种软件架构风格，是设计风格而不是标准。  
只是提供了一组设计原则和约束条件。  
它主要用于客户端和服务器交互类的软件。  
基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。  

在Restful风格中，用户请求的url使用同一个url而请求方式：get、post、delete、put…等方式对请求的处理方法进行区分，这样可以在前后分离式的开发中使得前端开发人员不会对请求的资源地址产生混淆和大量的检查方法名的麻烦，形成一个统一的接口。  

在Restful风格中，现有规定如下：

* GET（SELECT）  
从服务器查询，可以在服务器通过请求的参数区分查询的方式。

* POST（CREATE）  
在服务器新建一个资源，调用insert操作。  

* PUT（UPDATE）  
在服务器更新资源，调用update操作。  

* DELETE（DELETE）  
从服务器删除资源，调用delete语句。  

例如：  

如果当前url是：  
http://localhost:8080/User  
那么用户只要请求这样同一个URL就可以实现不同的增删改查操作，例如：  

* http://localhost:8080/User?_method=get&id=1001  
这样可以通过get请求获取到数据库user表里面id=1001的用户信息  

* http://localhost:8080/User?_method=post&id=1001&name=zhangsan  
这样可以向数据库user表里面插入一条记录

* http://localhost:8080/User?_method=put&id=1001&name=lisi  
这样可以将user表里面id=1001的用户名改为lisi。  

* http://localhost:8080/User?_method=delete&id=1001  
这样用于将数据库user表里面的id=1001的信息删除。

这样定义的规范被称之为Restful风格的API接口，即可以通过同一个url来实现各种操作。

### 3.5.2 SpringMVC对Restful风格的支持

#### 3.5.2.1 配置Restful风格支持的过滤器

在web.xml文件中配置过滤器。

示例：

	<!-- restful风格支持的过滤器：把POST请求转为指定请求方式（PUT,DELETE等) -->
	<filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
		<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
	</filter>
	<filter-mapping>
	  	<filter-name>HiddenHttpMethodFilter</filter-name>
	  	<url-pattern>/*</url-pattern>
	</filter-mapping>

#### 3.5.2.2 模拟增删改查的页面

示例：

	<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
	
	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
	<html>
	  <head>
	    <title>对restful支持</title>
	    
		<meta http-equiv="pragma" content="no-cache">
		<meta http-equiv="cache-control" content="no-cache">
		<meta http-equiv="expires" content="0">    
		<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
		<meta http-equiv="description" content="This is my page">
		<!--
		<link rel="stylesheet" type="text/css" href="styles.css">
		-->
	
	  </head>
	  
	  <body>
	  <h3>查询操作</h3>
	  <form action="${pageContext.request.contextPath}/restful.action" method="get">
	  	<input type="submit" value="提交"/>
	  </form>
	  
	  <hr/>
	  
	  <h3>新增操作</h3>
	  <form action="${pageContext.request.contextPath}/restful.action" method="post">
	  	用户名：<input type="text" name="userName"/><br/>
	  	密码：<input type="password" name="userPass"/><br/>
	  	<input type="submit" value="提交"/>
	  </form>
	  
	   <h3>更新操作</h3>
	  <form action="${pageContext.request.contextPath}/restful.action" method="post">
	  	<input type="hidden" name="_method" value="put">
	  	<input type="hidden" name="userId" value="1">
	  	用户名：<input type="text" name="userName"/><br/>
	  	密码：<input type="password" name="userPass"/><br/>
	  	<input type="submit" value="提交"/>
	  </form>
	  
	  <h3>删除操作</h3>
	  <form action="${pageContext.request.contextPath}/restful/1.action" method="post">
	  	<input type="hidden" name="_method" value="delete">
	  	<input type="submit" value="提交"/>
	  </form>
	  </body>
	</html>

#### 3.5.2.3 编写Controller控制器

示例：

	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.PathVariable;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RequestMethod;
	
	import com.marshal.domain.User;
	
	@Controller
	@RequestMapping("/restful")
	public class RestfulController {
	
		/**
		 * 查询操作
		 */
		@RequestMapping(method=RequestMethod.GET)
		public String testGet(){
			System.out.println("执行RestfulController的testGet");
			return "success";
		}
	
		/**
		 * 新增操作
		 */
		@RequestMapping(method=RequestMethod.POST)
		public String testPost(User user){
			System.out.println("执行RestfulController的testPost");
			System.out.println(user);
			return "success";
		}
		
		/**
		 * 更新操作
		 */
		@RequestMapping(method=RequestMethod.PUT)
		public String testPut(User user){
			System.out.println("执行RestfulController的testPut");
			System.out.println(user);
			return "success";
		}
		
		/**
		 * 删除操作
		 */
		@RequestMapping(value="/{id}",method=RequestMethod.DELETE)
		public String testDelete(@PathVariable("id")Integer id){
			System.out.println("执行RestfulController的testDelete");
			System.out.println(id);
			return "success";
		}
	}

