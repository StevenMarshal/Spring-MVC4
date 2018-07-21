## 2.3 @RequestParam简单参数接收

### 2.3.1 页面

示例：

    <h3>3.简单数据类型</h3>
    <form action="${pageContext.request.contextPath}/param/test3.action" method="post">
        用户名：<input type="text" name="userName"/><br/>
        密码：<input type="password" name="userPass"/><br/>
        <input type="submit" value="提交"/>
    </form>

### 2.3.2 Controller

示例：

    /**
	 * 3.简单数据类型
	 * @RequestParam:指定参数名称
	 *    value: 指定参数名
	 *    required：该参数是否为必须
	 *    defaultValue: 默认值
	 */
	@RequestMapping(value="/test3",method=RequestMethod.POST)
	public String test3(@RequestParam(value="name",required=false,defaultValue="zhangsan")String userName,String userPass){
		System.out.println(userName);
		System.out.println(userPass);
		return "success";
    }
