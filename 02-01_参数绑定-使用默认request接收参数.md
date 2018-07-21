## 2.1 使用默认的request接收参数

### 2.1.1 页面

示例：

    <h3>1.默认参数类型</h3>
    <form action="${pageContext.request.contextPath}/param/test1.action" method="post">
        用户名：<input type="text" name="userName"/><br/>
        密码：<input type="password" name="userPass"/><br/>
        <input type="submit" value="提交"/>
    </form>

### 2.1.2 Controller

示例：

    /**
	 * 1.默认参数类型
	 */
	@RequestMapping(value="/test1",method=RequestMethod.POST)
	public String test1(HttpServletRequest request){
		String userName = request.getParameter("userName");
		String userPass = request.getParameter("userPass");
		System.out.println(userName);
		System.out.println(userPass);
		return "success";
	}

### 2.1.3 解决POST请求中文乱码问题

在web.xml加上Spring的编码过滤器：

    <!-- 配置Spring的POST请求编码过滤器 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
