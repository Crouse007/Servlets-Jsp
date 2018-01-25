# Servlet  可以在任何服务器端运行的小程序
> - 必须实现**Servlet**接口
> - `void service(ServletRequest req, ServletResponse res)` *最重要的方法，被容器直接调用*
> - 给web用的继承类**HttpServlet**
> - 一个Servlet被调用的时候默认掉用service方法
> - *构建路径-> 引入外部归档 导入 servlet-api.jar*

### HttpServlet
> HttpServlet() 被容器调用
> ###### 常用方法
>> - protected void doGet(HttpServletRequest req, HttpServletResponse resp)
>> - protected void doPost(HttpServletRequest req, HttpServletResponse resp)       
>             
> - protected void doHead(HttpServletRequest req, HttpServletResponse resp) *只拿头信息,一般写浏览器的人用*      
> - protected void service(HttpServletRequest req, HttpServletResponse resp) *一般不重写，service方法默认帮我们调用doPost或doGet*


### 部署Servlet
> #### xml
>> *servlet-name* 随意设置            
>> *servlet-class* servlet（最好复制）                 
>> *servlet-pattern* 在浏览器里输入什么地址才调用这个servlet,用/开头，相对于webapplication的跟路径(http://127.0.0.1:8888/test/abc)
      <servlet>
        <servlet-name>HW</servlet-name>
        <servlet-class>HelloWorldServlet</servlet-class>
      </servlet>

      <servlet-mapping>
        <servlet-name>HW</servlet-name>
        <url-pattern>/abc</url-pattern>
      </servlet-mapping>
****
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doGet"); //后台打印，会打印到tomcat的后台界面
            response.getWriter().write("<a href='http://www.baidu.com'>go</a>");
	}
      
 ### Servlet的生命周期
 > - 加载	ClassLoader
 > - 实例化 new
 > - 初始化 init(ServletConfig)
 > - 处理请求 service()
 > - 退出服务 destory()
 ##### API中的过程
 > void init(ServletConfig config) //init()方法只执行一次，第一次初始化的时候                 
 > void service(ServletRequest req,ServletResponse res)throws ServletException,java.io.IOException                  
 > void destroy() //webapplication关闭的时候                    
 > **在非分布的情况下，通常Servlet在服务器中只有一个实例**             
 public class TestLifeCycleServlet extends HttpServlet{

	public TestLifeCycleServlet() {
		System.out.println("constructor!");
	}

	@Override
	protected void doGet(HttpServletRequest requestq, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doGet");
	}

	@Override
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init");
	}

	@Override
	public void destroy() {
		System.out.println("destory");
	}

}

### 用doGet和doPost方法处理请求
> - `protected void doGet(HttpServletRequest request, HttpServletResponse response)`
> - `protected void doPost(HttpServletRequest request, HttpServletResponse response)`

> HttpServletRequest
>> java.lang.String getParameter(java.lang.String name)
>> Enumeration<String> getParameterNames()
>> String[] getParameterValues(String name)
>> Map<String,String[]> getParameterMap()
> ****
> HttpServletResponse
>> response.setContentType("text/html;charset=gb2312");  *设置MIME*
>> PrintWriter pw = response.getWriter();
