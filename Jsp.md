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
