### idea 控制台出现乱码 “淇℃伅”

找到Tomcat目录下的**conf/logging.properties**配置文件修改java.util.logging.ConsoleHandler.encoding=GBK

### Context 配置

> War包部署到tomcat下，正常情况下访问地址应该是： http://localhost:端口号/项目名，如果想不带项目名，需要修改Context的设置。
>
> 修改**{TOMCAT_HOME}/conf/server.xml**文件 ，在 Host 节点下增加<Content>节点。
>
> ```xml
> <Host name="localhost"  appBase=""  deployXML ="false" deployOnStartup ="false" 
>       unpackWARs="true" autoDeploy="true">
> 
>     <Context path="" docBase="/usr/local/tomcat/webapps/bd_web_service" reloadable="true" />
> 
>     <!-- SingleSignOn valve, share authentication between web applications
>              Documentation at: /docs/config/valve.html -->
>     <!--
>         <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
>         -->
> 
>     <!-- Access log processes all example.
>              Documentation at: /docs/config/valve.html
>              Note: The pattern used is equivalent to using pattern="common" -->
>     <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
>            prefix="localhost_access_log" suffix=".txt"
>            pattern="%h %l %u %t &quot;%r&quot; %s %b" />
> 
> </Host>
> ```
>
> - path：指定访问该Web应用的URL入口。这里可为 path="/" 或 path=""
> - docBase：指定Web应用的文件路径，可以是绝对路径也可以给定相对于<Host>节点的appBase属性的相对路径。
> - reloadable：如果这个属性为true，tomcat 会监视WEB_INF/classes和WEB_INF/lib目录下class文件的改动，如果有改动服务器会自动重新加载Web应用。
