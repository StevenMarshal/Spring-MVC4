## 3.3 SpringMVC接收json数据和响应json数据

### 3.3.1 导入支持json的包文件

导入文件上传的jar包为：  

    jackson-annotations-2.4.0.jar
    jackson-core-2.4.2.jar
    jackson-databind-2.4.2.jar

### 3.3.2 创建页面json.jsp

在项目的WebRoot目录下新建js目录，并将jquery文件放入该文件夹中。  
创建json.jsp夜间文件。  

示例：  

	<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
	
	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
	<html>
	  <head>
	    <title>json数据处理</title>
	    
		<meta http-equiv="pragma" content="no-cache">
		<meta http-equiv="cache-control" content="no-cache">
		<meta http-equiv="expires" content="0">    
		<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
		<meta http-equiv="description" content="This is my page">
		<script type="text/javascript" src="js/jquery-1.11.1.min.js"></script>
	
	  </head>
	  
	  <body>
	      <script type="text/javascript">
	      	$(function(){
	      		$("#putAndGetJson").click(function(){
	      			//发出一个请求，请头中携带json格式数据
	      			$.ajax({
	      				url:"json/test1.action",
	      				data:'{"userName":"jack","userPass":"123456"}',
	      				contentType:"application/json",
	      				type:"post",
	      				success:function(data){  // data:服务端返回的数据
	      					alert(JSON.stringify(data));	
	      				},
	      				dataType:"json"
	      			});
	      		});
	      		
	      	});
	      
	      </script>
	      <input type="button" id="putAndGetJson" value="传输和获取json数据">
	  </body>
	</html>

### 3.3.3 编写JsonController处理请求

示例：  

	package com.marshal.controller;
	
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestBody;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.ResponseBody;
	
	import com.marshal.domain.User;
	
	@Controller
	@RequestMapping("/json")
	public class JsonController {
	
		//@RequestBody：代表接收页面的json数据
		//@ResponseBody: 代表Controller返回json数据
		@RequestMapping("/test1")
		@ResponseBody
		public User test1(@RequestBody User user){
			System.out.println(user.getUserName());
			System.out.println(user.getUserPass());
			return user;
		}
	}
