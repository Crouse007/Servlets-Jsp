# JSP - Java Server Pages
> JSP程序由JSP Engine先将它转换为Servlet代码，接着将它编译成类文件载入执行                    
> 只有当客户端第一次请求JSP时，才需要将其转换、编译                     
## JSP基础语法 -- 四种语法
> JSP传统语法            
>> - Declaration  声明                
>> - Scriptlet  小程序段                
>> - Expression  表达式           
>> - Comment  注释             
>> - Directives  指令           
>> - Action动作指令              
>> - 内置对象            
>                
> JSTL               
> JSF                        
> 其他taglib(如struts)
> ********************
### Declration --- 声明
> 基础语法:
>> - `<%!  %>`    成员变量  Servlet对象只初始化一次，只有一个对象（可能刷新快重置，可能有池也可能重新刷新）
>> - `<%   %>`    局部变量

	  <body>
	    <%!
	      int accessCount = 0;
	      void m() {}
	    %>

	    <%
	      int accessCount2 = 0;
	    %>

	    <H2>Accesses to page since server reboot:<%= ++accessCount %></H2>
	    <%= ++accessCount2 %>
	  </body>

> ****************
### Scriptlet
> 基础语法:
>>　- <%程序代码区%>
>
> 可以放入任何Java程序代码
> *****************
### Comment
> 基础语法：
>> - <%--... ...--%> JSP注释         
>> - <%//... ...%>  JSP注释                     
>> - <%/*... ...*/%>  JSP注释            
>> - <!--... ...--> html注释，可在客户端看到
>******************
### Expression 表达式
> 基础语法:
>> - `<%=... ...%>`
>
> =后面必须是字符串变量或者可以被转换成字符串的表达式           
> 不需要以；结束            
> 只有一行          

	  <%= request.getParamter("Name")%>
	  <%= session.getId()%>
	  <%= request.getRemoteHost()%>

>**************
### Directive 编译指令
> 相当于在编译期间的命令
> 格式:
>> - <%@Directive 属性="属性值"%>
>
> 常见的Directive:
>> - page            
>> - include            
>> - taglib
> **************
#### page
> 指明与JSP Container的沟通方式         
> 基本格式:       
>> - import
>> - errorPage 填写出错后跳转的Url
>> - isErrorPage 出错后显示的界面 <%= exception.getMessage()%>
>> - contentType

   	<%@page language="java"|
			extends="className"|
			import="import list"|  //引用什么包
			buffer="none | kb size "|    //默认8kb
			session="true|false"|       //是否使用session，默认true
			autoFlush="true|false"|         //缓冲器是否自动清除，默认true
			isThreadSafe="true"|      //默认就好，废用
			info="infoText"|        //一般不用
			errorPage="errorPageUrl"|        
			isErrorPage="false|true"|
			contentType="text/html; charset=ISO-8859-1"
			%>
			
> **************
#### include
> 将指定的JSP程序或者HTML文件*包含*进来            
> 格式：            
>> - <%@include file="fileUrl"%>
>
> JSP Engine会在JSP程序的转换时期先把file属性设定的文件包含进来，然后开始执行转换及编译工作,*效率高*                
> 限制：                 
>> - *一般用来包含非动态的代码，不能带参数*
>> - <%@include file="TitleBar.jsp?usr=abc"%>  //错误
>**********************
### Action 动作指令
> 在运行期间的命令               
> 常见的Action:         
>> - jsp:useBean
>>> - jsp:setProperty
>>> - jsp:getProperty
>>
>> - jsp:include 
>> - jsp:forward
>> - jsp:plugin
> ***************
#### jsp:include/jsp:param       
> 用于动态包含JSP程序或HTML文件等                  
> 除非这个指令会被执行到，否则它是不会被Tomcat等JSP Engine编译                  
> 格式:
>> 一：             
>> <jsp:include page="URLSpec" flush="true"/>                 
>> 二：          

	<jsp:inlucde page="TRLSpec" flush="true">	//flush填true                
	      <jsp:param name="ParamName" value="paramValue" />                
	</jsp:include> 
>
> jsp:param用来设定include文件时的参数和对应的值
	
	<% if( request.getParameter("computer").equals("division")) { %>
		<jsp:include page="divide.jsp">
			<jsp:param value="v1" name="<%=value1%>>"/>
			<jsp:param value="v2" name="<%=value2%>>"/>
		</jsp:include>
	<%}else{} %>

> **************
#### jsp:forward/jsp:param
> 用于将一个jsp的内容传到page所指定的jsp程序或者Servlet中处理（URL）                 
> 用于跳转，如登录跳转，用requsest.getParamter()获取参数
> 常用格式：
>> 一：           
>> <jsp:forward page="urlSpec" flush="true"/> flush一直为true                       
>> 二：                 

	<jsp:forward page="urlSpec" flush="true">				                    
		<jsp:param name="paramName" value="paramValue"/>       
	</jsp:forward>           
>> <jsp:param>用于指定参数和其对应值            
>
> **<jsp:forward>与response.sendRedirect的区别**
>> - forward的页面和forward到的页面用的同一个request
>> - response.sendRedirect不是用一个request
>> - <jsp:forward>是在服务器端的跳转，url不变
>> - response.sendRedirect是客户端的跳转，url要改变
> **************
#### jsp:useBean
> 通过jsp:useBean，可以在JSP中使用定义好的Bean
> Bean的基本要素：
>> - 必须要有一个不带参数的构造器。在JSP元素创建Bean时会调用空构造器
>> - Bean类应该没有任何公共实例变量，也就是说，不允许直接访问实例变量，变量名称首字母必须小写
>> - 通过getter/setter方法来读/写变量的值，并且将对应的变量首字母改成大写
>> - 不要使用裸体类
>
> 基本用法:     
>> 一：            
>> <jsp:useBean id="beanName" scope="page|request|session|application" class="package.BeanClass" type="typeName"/>            
>> 二：                 

	<jsp:useBean ...>
		<jsp:setProperty ...>
		<jsp:getProperty ...>
	</jsp:useBean>    
>
> jsp:useBean各项参数及含义           
>> - id:对象实例名称            
>> - scope:Bean的作用范围，默认为page，对整个jsp页面有效            
>> - class:Bean类名称           
>> - type:Bean实例类型，可以是本类或其父类，或实现的接口默认为本类      
>>
>> scope各项参数的意义：           
>>> - page:仅含使用JavaBean的页面                      
>>> - request:有效范围仅限使用javaBean的请求
>>> - session:有效范围在用户整个连接过程中(整个会话阶段均有效)                                            
>>> - application:有效范围涵盖整个应用程序，也就是对整个网站均有效                      

	<h3>Scope=request</h3>
	<b>count:</b><%=cb.getCount()%>
	<jsp:getProperty name="cb" property="count"/>

> **jsp:setProperty**的格式:
>> - <jsp:setProperty name="beanName" property="propertyName"|property="\*" value="property value"|parm="parmName"/>   
>>
>> 相当于调用beanName.setPropertyName(value)方法，setXxx()方法             
>
> **jsp:getProperty**的格式：
>> - <jsp:getProperty name="beanName" property="propertyName" />   
>>
>> 相当于调用beanName.getPropertyName(value)方法，getXxx()方法                   
>
> **建立表单参数和Bean属性之间的关联**
>> - 通过param指定表单元素名称，通过property指定对应的Bean属性的名称，由此建立这两个变量的关联
>> - 通过\*来设置所有属性和输入参数之间的关联
>
> *在建立Bean属性和表单参数之间的对应关系时，服务器会将对应的参数自动转换成和属性类型匹配的数据*
>*************************
### jsp_characterEncoding
> 通用方法,原编码ISO8859_1,新编码GBK

	<%
		String name = request.getParameter("name");
		name = new String( name.getBytes("ISO8859-1"),"GBK");
	%>

> 二：

	<%
		request.setCharacterEncoding("GBK");
	%>

### jsp内置对象 
> - out                        
> - request                         
> - response                        
> - pageContext     page运行的环境，用得也比较少                
> - session                        
> - application                        
> - config         一般指的web.xml               
> - exception                        
> - page    比较少用   
>
> **************
#### out
> 它是javax.servlet.jsp.JspWriter的一个实例
> 常用方法：
>> - println(); 
>> - newLine(); 输出一个换行符
>> - close(); 关闭输出流
>> - flush(); 输出缓冲区里的数据
>> - clearBuffer(); 清除缓冲区里的数据，同时把数据输出到客户端
>> - clear(); 清除缓冲区里的数据，但不把数据输出到客户端
>> - getBufferSize();
>
> ********************
#### request
> javax.servlethttp.HttpServletRequest接口的一个实例
> 可以用此对象取得请求的Header、信息（如浏览器版本、语言和编码等）、请求的方式（get/post）、请求的参数名称、参数值、客户端的主机名称等
> 常用方法：
>> - getMethod(); 返回客户端向服务器传送数据的方法（post/get）
>> - getParamter(String paramName) 返回客户端向服务器传送参数的参数值
>> - getParamterNames() 获得客户端传送给服务器端的所有参数的名字，结果是一个枚举类型数据(Enumeration)
>> - getParamterValues(String name) 
>> - getRequestURL(); 获得发出请求字符串的客户端地址
>> - getRemoteAddr(); 获得客户端的IP地址
>> - getRemoteHost(); 获得客户端的机器名称
>> - getServerName(); 获得服务器的名字
>> - getServletName(); 客户端所请求的脚本文件路劲
>> - getServerPort(); 获取服务器端的端口
>> - setCharacterEncoding();
>
> ***************
#### response
> javax.servlethttp.HttpServletResponse接口的一个实例
> 通常用来设置HTTP标题，添加cookie、设置响应内容的类型和状态，发送HTTP重定向和编码URL
> 常用方法：
>> - addCookie(Cookie cookie); 添加一个Cookie对象，用于在客户端保存特定的信息
>> - addHeader(String name,String value); 添加HTTP头信息，该Header信息将发送到客户端
>> - containsHeader(String name); 判断指定名字的HTTP文件头是否存在
>> - sendError(int); 想客户端发送错误信息，int HTTP错误码 404 500
>> - sendRedirect(String url);  重定向JSP文件
>> - setContentType(String contentType); 设置MIME类型与编码方式，text/html;charset=gb2312
>
> *************
#### session
> <% @page session="true">（默认） --启动session功能
> session的常用方法
>> - void setAttribute(String name,Object value)
>> - Object getAttribute(String name)
>> - boolean isNew();
>> - getId()
>
> *************************
#### application
> ServletContext
> ****************
### Servlet和JSP的通信
#### 从JSP调用Servlet
> <jsp:forward>
> sendRedirect
> *******************
#### 从Servlet调用JSP
> RequestDispatcher接口的forward(req,res)方法
>> getServletConfig().getServletContext().getRequestDispatcher("/xxx.jsp").forward(req,res);                               
> 请求信息需要显示传递（在req,res参数中）                     
> 或者通过sendRedirect


