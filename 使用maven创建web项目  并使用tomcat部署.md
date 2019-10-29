使用maven创建web项目  并使用tomcat部署

使用java环境基础 

rm -rf 

tar -zxvf  jdk-8u201-linux-x64.tar.gz

 mv apache-maven-3.6.0/ /usr/local/maven

echo '
PATH=$PATH:/usr/local/maven/bin/' >> /etc/profile

source /etc/profile

mvn -v

mkdir /tmp/test

cd /tmp/test

mvn archetype:generate -DgroupId=com.kgc.kgcapp -DartifactId=kgcapp  -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeCatalog=internal



mvn archetype:generate -DgroupId=com.kgc.kgcweb -DartifactId=kgcweb -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false 

cd /kgcweb

mvn package

cd /t,p/test/kgcweb/target/

### **安装tomcat**

**修改配置文件**

tar -zxvf apache-tomcat-8.5.35.tar.gz 

mv apache-tomcat-8.5.35 /usr/local/tomcat

vim /usr/local/tomcat/webapps/manager/META-INF/context.xml 

​		allow="^.*$" />		//改为所有人可访问

vim /usr/local/tomcat/conf/tomcat-users.xml

<role rolename="manager-gui"/>

<role rolename="manager-script"/>

<user username="manager" password="123" roles="manager-gui"/>

<user username="script" password="123" roles="manager-script">

重启tomcat，访问IP+端口 192.168.1.10:8080/manager

如成功验证用户和密码

登陆网页查询war包

返回命令行

 cd /tmp/test/kgcweb/target/

unzip kgcweb.war  -d /usr/local/tomcat/webapps/app

访问192.168.1.10:8080/app

完成访问







