## 2.4 JavaBean对象封装参数

### 2.4.1 页面

示例：

    <h3>4.JavaBean类型</h3>
    <form action="${pageContext.request.contextPath}/param/test4.action" method="post">
        用户名：<input type="text" name="userName"/><br/>
        密码：<input type="password" name="userPass"/><br/>
        手机：<input type="text" name="userTelephone"/><br/>
        <input type="submit" value="提交"/>
    </form>

### 2.4.2 User类

示例：

    import java.io.Serializable;

    public class User implements Serializable{
	
	    private String userName;
	    private String userPass;
	    private String userTelephone;
	
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
	    public String getUserTelephone() {
		    return userTelephone;
	    }
	    public void setUserTelephone(String userTelephone) {
		    this.userTelephone = userTelephone;
	    }
    }

### 2.4.3 Controller

示例：

    /**
	 * 4.JavaBean类型
	 */
	@RequestMapping(value="/test4",method=RequestMethod.POST)
	public String test4(User user){

		System.out.println(user.getUserName());
		System.out.println(user.getUserPass());
		System.out.println(user.getUserTelephone());
		return "success";
    }
