## 3.4 SpringMVC拦截器

拦截器的作用：  
拦截器可以实现在Controller的方法执行前和之后加入业务逻辑。  
例如：权限判断逻辑、日志记录等。  

### 3.4.1 SpringMVC拦截器的开发步骤

#### 3.4.1.1 创建目标Controller方法

创建InterceptorController控制器  

示例：  

	package com.marshal.controller;
	
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	
	@Controller
	@RequestMapping("/interceptor")
	public class InterceptorController {
	
		@RequestMapping("/test1")
		public String test1(){
			System.out.println("InterceptorController的test1方法");
			return "success";
		}
	}

#### 3.4.1.2 创建第一个SpringMVC拦截器

新建interceptor包并创建拦截器类。  

示例：  

	package com.marshal.interceptor;
	
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	
	import org.springframework.web.servlet.HandlerInterceptor;
	import org.springframework.web.servlet.ModelAndView;
	
	public class MyInterceptor1 implements HandlerInterceptor{
	
		//在Controller方法完成之后被执行
		@Override
		public void afterCompletion(HttpServletRequest arg0,
				HttpServletResponse arg1, Object arg2, Exception arg3)
				throws Exception {
			System.out.println("执行MyInterceptor1的afterCompletion");
		}
	
		//在Controller方法执行之后被执行
		@Override
		public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1,
				Object arg2, ModelAndView arg3) throws Exception {
			System.out.println("执行MyInterceptor1的postHandle");
		}
	
		//在Controller方法执行之前被执行
		/**
		 * 该方法的返回值，代表是否继续执行Controller方法
		 *     true：继续执行
		 *     false:不执行
		 */
		@Override
		public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1,
				Object arg2) throws Exception {
			System.out.println("执行MyInterceptor1的preHandle");
			return true;
		}
	
	}

该类实现了HandlerInterceptor接口，并需要同时实现三个方法，这三个方法分别需要在Controller被拦截方法执行之前、之后、完成这三个时间段执行。

#### 3.4.1.3 配置spring-mvc.xml

示例：  

    <!-- 配置拦截器 -->
	<mvc:interceptors>
		<mvc:interceptor>
			<!-- 拦截的路径
				注意: path路径必须是完整的路径（包括后缀名称）
			 -->
			<mvc:mapping path="/interceptor/test1.action"/>
			<bean class="cn.sm1234.interceptor.MyInterceptor1"></bean>
		</mvc:interceptor>
    </mvc:interceptors>

#### 3.4.1.4 执行效果

    执行MyInterceptor1 的preHandle
    InterceptorController 的test1 方法
    执行MyInterceptor1 的postHandle
    执行MyInterceptor1 的afterCompletion

### 3.4.2 多个拦截器的配置与执行顺序

#### 3.4.2.1 再创建一个拦截器类

示例：  

	package com.marshal.interceptor;
	
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	
	import org.springframework.web.servlet.HandlerInterceptor;
	import org.springframework.web.servlet.ModelAndView;
	
	public class MyInterceptor2 implements HandlerInterceptor{
	
		//在Controller方法完成之后被执行
		@Override
		public void afterCompletion(HttpServletRequest arg0,
				HttpServletResponse arg1, Object arg2, Exception arg3)
				throws Exception {
			System.out.println("执行MyInterceptor2的afterCompletion");
		}
	
		//在Controller方法执行之后被执行
		@Override
		public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1,
				Object arg2, ModelAndView arg3) throws Exception {
			System.out.println("执行MyInterceptor2的postHandle");
		}
	
		//在Controller方法执行之前被执行
		/**
		 * 该方法的返回值，代表是否继续执行Controller方法
		 *     true：继续执行
		 *     false:不执行
		 */
		@Override
		public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1,
				Object arg2) throws Exception {
			System.out.println("执行MyInterceptor2的preHandle");
			return true;
		}
	}

#### 3.4.2.2 配置spring-mvc.xml

示例：  

	<!-- 配置拦截器 -->
	<mvc:interceptors>
		<mvc:interceptor>
			<!-- 拦截的路径
				注意: path路径必须是完整的路径（包括后缀名称）
			 -->
			<mvc:mapping path="/interceptor/test1.action"/>
			<bean class="cn.sm1234.interceptor.MyInterceptor1"></bean>
		</mvc:interceptor>
		<mvc:interceptor>
			<mvc:mapping path="/interceptor/test1.action"/>
			<bean class="cn.sm1234.interceptor.MyInterceptor2"></bean>
		</mvc:interceptor>
	</mvc:interceptors>

#### 3.4.2.3 执行效果

示例：  

	执行MyInterceptor1 的preHandle
	执行MyInterceptor2 的preHandle
	InterceptorController 的test1 方法
	执行MyInterceptor2 的postHandle
	执行MyInterceptor1 的postHandle
	执行MyInterceptor2 的afterCompletion
	执行MyInterceptor1 的afterCompletion

拦截器的执行顺序是根据spring-mvc.xml文件中的拦截器的配置顺序有关。
