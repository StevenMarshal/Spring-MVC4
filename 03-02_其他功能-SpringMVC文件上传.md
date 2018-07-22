## 3.2 SpringMVC文件上传

### 3.2.1 导入文件上传的支持jar 包

导入文件上传的jar包为：  

    commons-fileupload-1.2.2.jar
    commons-io-2.4.jar

### 3.2.2 创建上传文件表单页面upload.jsp

示例：

	<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
	
	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
	<html>
	  <head>
	    <title>文件上传</title>
	    
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
	  <form action="${pageContext.request.contextPath }/upload/test1.action" method="post" enctype="multipart/form-data">
	  	用户名：<input type="text" name="userName"/><br/>
	  	密码：<input type="password" name="userPass"/><br/>
	  	头像:<input type="file" name="headIcon"/><br/>
	  	<input type="submit" value="保存"/>
	  </form>
	  </body>
	</html>

其中form的属性必须加上enctype="multipart/form-data"

### 3.2.3 创建User类实体bean

示例：

	package com.marshal.domain;
	
	import java.io.Serializable;
	
	public class User implements Serializable {
		
		private String userName;
		private String userPass;
		
		public String getUserName() {
			return userName;
		}
		public void setUserName(String userName) {
			this.userName = userName;
		}
		public String getUserPass() {
			return userPass;
		}
		public void setUserPass(String userPass) {
			this.userPass = userPass;
		}
		
	}

### 3.2.4 创建文件上传Controller类

示例：

	package com.marshal.controller;
	
	import java.io.File;
	import java.io.IOException;
	
	import javax.servlet.http.HttpServletRequest;
	
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.multipart.MultipartFile;
	
	import com.marshal.domain.User;
	
	
	@Controller
	@RequestMapping("/upload")
	public class UploadController {
	
		@RequestMapping("test1")
		public String test1(User user,MultipartFile headIcon,HttpServletRequest request) throws Exception{
			System.out.println(user.getUserName());
			System.out.println(user.getUserPass());
			
			//保存文件
			String uploadPath = request.getServletContext().getRealPath("/upload");
			headIcon.transferTo(new File(uploadPath+"/"+headIcon.getOriginalFilename()));
			
			return "success";
		}
	}

其中参数MultipartFile的参数名称必须要与表单中的参数名称相同，都是headIcon，如果想要不相同可以通过@RequestParam来实现。同时需要在项目下建立一个upload文件夹。

### 3.2.5 配置spring-mvc.xml

需要配置一个专门用于文件上传的bean。

示例：

    <!-- 处理文件上传 -->
	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 文件大小限制 -->
		<property name="maxUploadSize" value="20480"></property>
    </bean>

注意：该bean的id一定要有，而且名字一定要叫作“multipartResolver”。
