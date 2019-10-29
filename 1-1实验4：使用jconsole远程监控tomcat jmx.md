 **实验4：使用jconsole远程监控tomcat jmx** 



vim /usr/local/tomcat/bin/catalina.sh：

**310行**

添加：

```
CATALINA_OPTS="-Djava.rmi.server.hostname=192.168.1.1
```

​			  （rmi是远程模块，hostname是远程主机允许访问的主机）

```
-Dcom.sun.management.jmxremote
```

​				

```
-Dcom.sun.management.jmxremote.port=8080
```

​				（客户机监控tomcat的端口，8080）

```
-Dcom.sun.management.jmxremote.ssl=false
```

​				（不进行身份验证）**

```
-Dcom.sun.management.jmxremote.authenticate=true"
```

![1571726884549](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571726884549.png)

保存退出

```
cd /usr/local/java/jre/lib/management

cp jmxremote.password.template jmxremote.password

vim jmxremote.password
```

![1571726928301](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571726928301.png)



chmod 600 jmxremote.*

关闭再重启服务

```
/usr/local/tomcat/bin/shutdown.sh 
 /usr/local/tomcat/bin/startup.sh 
```

**配置客户端（192.168.1.20）**

**安装jdk环境**

```
cd /root

tar -zxvf jdk-8u201-linux-x64.tar.gz

mv jdk1.8.0_201/ /usr/local/java
```

 **编辑环境变量** 

```
echo '

export JAVA_HOME=/usr/local/java

export JRE_HOME=/usr/local/java/jre

export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib

export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin' >> /etc/profile
```

**更新环境变量**

```
source /etc/profile
```

 

**查看版本是否更改：**

```
java -version
```

**启动**

**/usr/local/java/bin/./jconsole**

此用户名是第一台主机用户、用户口令是设置的123456

![1571727265253](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571727265253.png)

**客户端访问成功**

![1571727328135](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571727328135.png)



