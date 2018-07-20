## 1.6 @RequestMapping注解的使用

@RequestMapping：声明SpringMVC控制器的类或方法的访问路径。  

### 1.6.1 value属性（*）

访问路径，用在类或方法上面。  

例如：  

	@Controller
	@RequestMapping(value="/requestMapping")
	public class RequestMappingController {
	
		/**
		 * value属性
		 * @return
		 */
		@RequestMapping(value="/test1")
		public ModelAndView test1(){
			System.out.println("RequestMappingController的test1");
			ModelAndView mv = new ModelAndView();
			mv.setViewName("success");
			return mv;
		}
	}

### 1.6.2 method属性（*）

该控制器的方法支持哪些HTTP 的请求方式。（GET，POST，PUT，DELETE）。   

例如：

    /**
	 * method属性
	 * @return
	 */
	@RequestMapping(value="/test2",method=RequestMethod.POST)
	public ModelAndView test2(){
		System.out.println("RequestMappingController的test2");
		ModelAndView mv = new ModelAndView();
		mv.setViewName("success");
		return mv;
	}

### 1.6.3 param属性（*）

声明控制器的方法可以接受什么参数。

例如：

    /**
	 * params属性
	 * @return
	 */
	@RequestMapping(value="/test3",method=RequestMethod.GET,params={"name"})
	public ModelAndView test3(){
		System.out.println("RequestMappingController的test3");
		ModelAndView mv = new ModelAndView();
		mv.setViewName("success");
		return mv;
    }

### 1.6.4 其他属性

* producers属性

该控制器的方法返回值的数据类型（xml/json）。

* consumer属性

该控制器的方法接受参数的数据类型（xml/json）。

* headers属性

该控制器的方法接受包含什么请求头的请求。
