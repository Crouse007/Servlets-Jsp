# JSP - Java Server Pages
> JSP程序由JSP Engine先将它转换为Servlet代码，接着将它编译成类文件载入执行                    
> 只有当客户端第一次请求JSP时，才需要将其转换、编译                     

### JSP基础语法 -- 四种语法
> JSP传统语法            
>> Declaration  声明                
>> Scriptlet  小程序段                
>> Expression  表达式           
>> Comment  注释             
>> Directives  指令           
>> Action动作指令              
>> 内置对象            
>                
> JSTL               
> JSF                        
> 其他taglib(如struts)

### Declration --- 声明
> 基础语法
>> `<%!  %>`    成员变量  Servlet对象只初始化一次，只有一个对象（可能刷新快重置，可能有池也可能重新刷新）
>> `<%   %>`    局部变量

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

### Scriptlet
> 基础语法
>>　<%程序代码区%>
>
> 可以放入任何Java程序代码
> 里面可以加**注释**
>> <%--... ...--%> JSP注释           
>> <%//... ...%>  JSP注释                       
>> <%/*... ...*/%>  JSP注释            
>> <!--... ...--> html注释，可在客户端看到

### Expression 表达式
> 基础语法
>> `<%=... ...%>`
>
> =后面必须是字符串变量或者可以被转换成字符串的表达式
> 不需要以；结束
> 只有一行

  <%= request.getParamter("Name")%>
  <%= session.getId()%>
  <%= request.getRemoteHost()%>
  
### Directive 编译指令
> 相当于在编译期间的命令
> 格式
>> <%@Directive 属性="属性值"%>
>
> 常见的Directive
>> page            
>> include            
>> taglib
> 
> **page**
> 指明与JSP Container的沟通方式
> 基本格式
>> import
>> errorPage 填写出错后跳转的Url
>> isErrorPage 出错后显示的界面 <%= exception.getMessage()%>
>> contentType

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
> **include**
> 将指定的JSP程序或者HTML文件*包含*进来
> 格式：
>> <%@include file="fileUrl"%>
>
> JSP Engine会在JSP程序的转换时期先把file属性设定的文件包含进来，然后开始执行转换及编译工作,*效率高*
> 限制：
>> *一般用来包含非动态的代码，不能带参数*
>> <%@include file="TitleBar.jsp?usr=abc"%>  //错误

### Action 动作指令
> 在运行期间的命令
> 常见的Action
>> jsp:useBean
>>> jsp:setProperty
>>> jsp:getProperty
>>
>> jsp:include 
>> jsp:forward
>> jsp:plugin
>
>**jsp:include/jsp:param**
> 用于动态包含JSP程序或HTML文件等
> 除非这个指令会被执行到，否则它是不会被Tomcat等JSP Engine编译
> 格式
>> <jsp:include page="URLSpec" flush="true"/>
>> <jsp:inlucde page="TRLSpec" flush="true">	//flush填true
>> 	<jsp:param name="ParamName" value="paramValue" />
>> </jsp:include>	
>
> jsp:param用来设定include文件时的参数和对应的值
	
	<% if( request.getParameter("computer").equals("division")) { %>
		<jsp:include page="divide.jsp">
			<jsp:param value="v1" name="<%=value1%>>"/>
			<jsp:param value="v2" name="<%=value2%>>"/>
		</jsp:include>
	<%}else{} %>
> **jsp:forward/jsp:param**
> 用于将一个jsp的内容传到page所指定的jsp程序或者Servlet中处理（URL）
> 格式:
>> <jsp:forward page="urlSpec" flush="true"/>
>> <jsp:forward page="urlSpec" flush="true">
>> 	<jsp:param name="paramName" value="paramValue"/>
>> </jsp:forward>
>> <jsp:param>用于指定参数和其对应值
>
> forward的页面和forward到的页面用的同一个request
