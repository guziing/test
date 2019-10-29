maven

**（要求java环境）**（jdk）

```
tar -zxvf jdk-8u201-linux-x64.tar.gz 

mv jdk1.8.0_201/ /usr/local/java

echo '
export JAVA_HOME=/usr/local/java
export JRE_HOME=/usr/local/java/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin' >> /etc/profile

source /etc/profile
  java -version
```

解压maven

```
tar -zxvf apache-maven-3.6.0-bin.tar.gz 

mv apache-maven-3.6.0/ /usr/local/maven

配置maven的环境变量
echo "PATH=$PATH:/usr/local/maven/bin" >> /etc/profile

#刷新
source /etc/profile
#查看版本号
 mvn -v 
 
 mkdir /tmp/test
 cd /tmp/test

```

![1571813956602](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571813956602.png)

**#编译打包**

mvn archetype:generate -DgroupId=com.kgc.kgcapp -DartifactId=kgcapp  -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeCatalog=internal

![1571815345019](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571815345019.png)

![](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571814823906.png)



**语法含义**

**生成一个MAVEN项目**

```
mvn archetype:generate   //archetype，是一个maven使用的插件，用来提供项目模板；generate该值表示获取所有的项目模板

 -DgroupId=com.kgc.kgcapp   //指定该项目的公司名称与项目名称，由3部分组成kgc.app为公司名，com为项目名称，这一部分可自定义，但是groupid对应的名字应该是唯一的

 -DartifactId=kgcapp    //构建这个项目的名字

 -DarchetypeArtifactId=maven-archetype-quickstart  //从获取的所有项目模板中使用quickstart 这个模板，这是一个java项目的模板，构建完成之后可以生成jar包

 -DarchetypeCatalog=internal   //不从远程获取项目构建的配置而是以交互方式来确定配置信息，避免项目产生卡在一个界面


```



**mvn compile**





写入镜像

**maven配置文件**

vim /usr/local/maven/conf/settings.xml 

```
     <mirror>
      <id>aliyun</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>
     </mirror>
```

![1571818568042](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571818568042.png)

保存退出

mvn package		打包

ls target/ 			会有一个包

![1571818598121](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571818598121.png)

mvn test    测试

mvn clean   清理 （把所有编译结果清理掉）

mvn install 	上传

![1571818802786](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571818802786.png)

mvn package				//先清理后打包

mvn  clean install 		//先清理后上传

mvn deploy		 		//把包上传到私服里



测试下载需要的时间比加速前快了

**切换到test下**

cd /tmp/test

**执行编译**

```
 mvn archetype:generate -DgroupId=com.kgc.kgcweb -DartifactId=kgcweb -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false 
```













































关于生成的配置文件的详细信息

**vim /test/study/pom.xml**

```
<modelVersion>4.0.0</modelVersion>         //POM的版本，maven2和maven3都使用的是4.0.0版本
  <groupId>edu.lgc.study</groupId>     //公司名称+项目名称这个名称是唯一的
  <artifactId>study</artifactId>        //构建该项目的名称
  <packaging>jar</packaging>           //该项目的打包格式
  <version>1.0-SNAPSHOT</version>    //该项目的版本，组成为主版本。次版本-限定版本名称，SNAPSHOT表示该项目为不稳定版本还在开发阶段
  <name>study</name>           //项目名称，记录在maven文档中
  <url>http://maven.apache.org</url>    //该项目的url也就是记录在maven的文档中

  <dependencies>          
    <dependency>        //该项目的依赖插件
      <groupId>junit</groupId>     //junit是java unix，java测试单元，对项目编译后的字节码进行测试用的一个插件
      <artifactId>junit</artifactId>
      <version>3.8.1</version>   //依赖该插件的的版本
      <scope>test</scope>         //这个依赖模块使用来做测试的，scope也可以由其他的值，比如compile用来做编译的，不同的值对应的依赖插件就会不同
    </dependency>
  </dependencies>
</project>
```













