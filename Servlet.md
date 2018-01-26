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
> 
> *HttpServletRequest*
>> java.lang.String getParameter(java.lang.String name)              
>> Enumeration<String> getParameterNames()            
>> String[] getParameterValues(String name)            
>> Map<String,String[]> getParameterMap()                  
> ****
> *HttpServletResponse*                
>> response.setContentType("text/html;charset=gb2312");  *设置MIME*                    
>> PrintWriter pw = response.getWriter();           

### 处理Cookie
> Http协议的无连接性要求出现一种保存C/S状态的机制                
> 服务器可以向客户端写内容，只能写入文本内容            
> 客户端可以阻止服务器写入            
> 只能拿自己webapp写入的东西            
> Cookie:保存到客户端的一个文本文件，与特定客户相关                           
> Cookie以“名-值”对的形式保存数据         
> Cookie分为两种           
>> 1. 属于窗口/子窗口          
>> 2. 属于文本              
>
> 创建Cookie: new Cookie(name,value)                      
> 可以使用Cookie的setXXX方法来设定一下相应的值                           	              
>> void	setValue(String newValue)/String getName()            
>> void	setName(String name)/String getName()            
>> void	setMaxAge(int expiry)/int getMaxAge()  //不设置生存周期或者设置为-1，写入内存，属于该窗口或子窗口，浏览器关闭就消失；设置生存周期则写入到文件               
>> 利用HttpServletResponse的void addCookie(Cookie cookie)方法将它设置到客户端                             
>> 利用HttpServletRequest的Cookie\[\] getCookies()方法来读取客户端的所有Cookie    


	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doGet");
		for (int i = 0; i < 3; i++) {
			Cookie cookie = new Cookie("Session-Cookie-"+i, "Cookie-value-S"+i);
			response.addCookie(cookie);
			
			cookie = new Cookie("Persistent-Cookie-"+i, "Cookie-value-P"+i);
			cookie.setMaxAge(3600);
			response.addCookie(cookie);
		}
		
		response.setContentType("text/html;charset=gb2312");
		PrintWriter pw = response.getWriter();
		
		Cookie[] cookies = request.getCookies();
		for (int i = 0; i < cookies.length; i++) {
			Cookie cookie = cookies[i];
			pw.print(cookie.getName());
			pw.println(cookie.getValue());
			pw.println("</br>");
		}
	}
> **一个servlet/jsp设置的Cookie能被同一路径下或者子路径(url)下的servlet或jsp读到**
>> 能读到：

	<servlet-mapping>
		<servlet-name>SetCookies</servlet-name>
		<url-pattern>/servlet/SetCookies</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>ShowCookie</servlet-name>
		<url-pattern>/ShowCookie</url-pattern>
	</servlet-mapping>
> *****	
>> 不能读到：

	<servlet-mapping>
		<servlet-name>SetCookies</servlet-name>
		<url-pattern>/SetCookies</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>ShowCookie</servlet-name>
		<url-pattern>/servlet/ShowCookie</url-pattern>
	</servlet-mapping>
	
### 会话跟踪 --- Session
> 记录一系列的状态                             
> **与Cookie的区别**：Cookie是记录在客户端；Session是记录在服务器端      
> 与浏览器窗口或子窗口关联（给浏览器赋予独一无二的号码）
> **Session的实现方法：** 
>> 1. 通过Cookie实现，把SessionID放入临时Cookie                  
>> 2. 通过url重写实现,response.encodeURL() //1、转码 2、URL后加入SessionID，**最好加**            
>             
> Session 有过期时间，在xml里配置         
> 在Session里写东西，在其他页面里面检查就可以知道是否登录       
> HttpSession 常用方法              
>> = Object getAttribute(String name)             
>> - void setAttribute(String name, Object value)             
>> - String getId()             
>> - boolean isNew()          
>> - long getCreationTime()          
>> - long getLastAccessedTime()       
>              
> 其他常用方法          
>> - request.getSession(true);	//true:无建有取  false:有取不创建          
>> - response.setContentType("text/html");     
>> - new Date(mySession.getCreationTime());             
>> - <a href="+response.encodeURL("SessionInfoServlet")+">refresh</a>         
>> - request.isRequestedSessionIdValid() //判断Session是否有效          
>> - request.isRequestedSessionIdFromURL()           
>> - request.isRequestedSessionIdFromCookie()         
>> - request.getRequestedSessionId()           
    
	public class SessionInfoServlet extends HttpServlet{

		@Override
		protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			HttpSession mySession = request.getSession(true);	//true:无建有取  false:有取不创建
			Integer accessCount = (Integer)mySession.getAttribute("accessCount");
			String heading;
			if(accessCount==null) {
				accessCount = new Integer(0);
				heading="Welcome,Newcomer";
			}else{
				heading="Welcome back";
				accessCount = new Integer(accessCount.intValue()+1);
			}
			mySession.setAttribute("accessCount", accessCount);
			response.setContentType("text/html");
			PrintWriter pw = response.getWriter();
			pw.println("<html>");
			pw.println("<head>");
			pw.println("<title>Session info Servlet</title>");
			pw.println("</head>");
			pw.println("<body>");
			pw.println("<h3>Session Infomation</h3>");
			pw.println("New session: "+ mySession.isNew());
			pw.println("<br>Session ID: "+mySession.getId());
			pw.println("<br>Session creation time: "+new Date(mySession.getCreationTime()));
			pw.println("<br>Session last accessed time: "+new Date(mySession.getLastAccessedTime()));
			pw.println("<h3>Request Infomation</h3>");
			pw.println("SessionID from request: "+request.getRequestedSessionId());
			pw.println("<br>SessionID via Cookie: "+request.isRequestedSessionIdFromCookie());
			pw.println("<br>SessionID via URL: "+request.isRequestedSessionIdFromURL());
			pw.println("<br>Valid SessionID: "+request.isRequestedSessionIdValid());
			pw.println("<br><a href="+response.encodeURL("SessionInfoServlet")+">refresh</a>");
			pw.println("<h3>Session Tracking</h3>");
			pw.println("<h4>"+heading+"</h4>");
			pw.println("Number of Previous Access: "+accessCount.intValue());
			pw.println("</body></html>");
			pw.close();
		}
