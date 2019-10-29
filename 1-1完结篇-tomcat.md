**完结篇-tomcat**

编辑配置文件：

vim /usr/local/tomcat/conf/server.xml 

```
 <Connector port="8080" 
                protocol="org.apache.coyote.http11.Http11AprProtocol"
                connectionTimeout="20000"		
                maxThreads="1000"		//并发量
                maxProcessors="1000"	//最大线程数
                minProcessors="1000"	//最小线程数
                maxSpareThreads="1000"		//最大空闲线程数
                minSpareThreads="500"		//最小空闲线程数
                acceptCount="1000"			//服务处理请求时最多可派多少
                URIEncoding="utf-8"			//编码格式
                redirectPort="8443" />		//应用于某些需要基于安全通道场合下端口使用
```

![1571809278212](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571809278212.png)

重启服务

![1571810142497](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571810142497.png)