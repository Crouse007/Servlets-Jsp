# Servlets&Jsp

> #### 返回码 
>> - 200 正常
>> - 404 文件没找到
>> - 403 拒绝访问
>> - 500 服务器内部异常
****
> #### Web Application 
>> **Web Application Name**
>> ###### WEB-INF *必有*
>>> web.xml   *该web app的配置文件*
>>> ###### lib   *该web app用到的库文件*
>>> ###### classes  *存放编译好的servlet*
>> ###### META-INF *存放web app的上下文信息，符合J2EE标准，可无*

    <?xml version="1.0" encoding="ISO-8859-1"?>

    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                          http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
      version="3.1"
      metadata-complete="true">

    </web-app>
