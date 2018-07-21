## 2.2 @PathVariable通过路径接收参数

### 2.2.1 页面

示例：

    <h3>2.@PathVariable路径参数接收</h3>
    <form action="${pageContext.request.contextPath}/param/test2/zhangsan/123456.action" method="post">
        <input type="submit" value="提交"/>
    </form>

### 2.2.2 Controller

    /**
	 * 2.@PathVariable路径参数接收
	 */
	@RequestMapping(value="/test2/{userName}/{userPass}",method=RequestMethod.POST)
	public String test2(@PathVariable(value="userName")String userName,@PathVariable(value="userPass")String userPass){
		System.out.println(userName);
		System.out.println(userPass);
		return "success";
	}
