## 2.5 包装JavaBean类型接收参数

### 2.5.1 页面

示例：

    <h3>5.包装JavaBean类型</h3>
    <form action="${pageContext.request.contextPath}/param/test5.action" method="post">
        用户名：<input type="text" name="user.userName"/><br/>
        密码：<input type="password" name="user.userPass"/><br/>
        手机：<input type="text" name="user.userTelephone"/><br/>
        性别：
        <input type="radio" name="gender" value="男"/>男
        <input type="radio" name="gender" value="女"/>女
        <input type="submit" value="提交"/>
    </form>

### 2.5.2 UserVo类

示例：

    /**
    * UserVo:包装JavaBean类型
    */
    public class UserVo {
        
        private User user;
        private String gender;

        public User getUser() {
		    return user;
	    }
        public void setUser(User user) {
            this.user = user;
        }
        public String getGender() {
            return gender;
        }
        public void setGender(String gender) {
            this.gender = gender;
        }
	}

### 2.5.3 Controller

示例：

    /**
    * 5.包装JavaBean类型
    */
    @RequestMapping(value="/test5",method=RequestMethod.POST)
    public String test5(UserVo userVo){
        System.out.println(userVo.getUser().getUserName());
        System.out.println(userVo.getUser().getUserPass());
        System.out.println(userVo.getUser().getUserTelephone());
        System.out.println(userVo.getGender());
        return "success";
    }

