*私服：nexus**

 先部署java+maven环境（略） 



mkdir /usr/local/nexus

tar -zxvf nexus-3.14.0-04-unix.tar.gz  -C /usr/local/nexus/

启动服务

/usr/local/nexus/nexus-3.14.0-04/bin/nexus  start

 查看端口号：（等一会）（1~2分钟） 

![1571898812019](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571898812019.png)

192.168.1.8:8081

![1571898834353](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571898834353.png)

 

![1571898918975](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571898918975.png)cd /usr/local/nexus/nexus-3.14.0-04/

java -jar ./lib/support/nexus-orient-console.jar 

```
 **orientdb>** connect plocal:../sonatype-work/nexus3/db/security admin admin 

update user SET password="$shiro1$SHA-512$1024$NE+wqQq/TmjZMvfI7ENh/g==$V4yPw8T64UQ6GfJfxYq2hLsVrBY8D1v+bktfOxGdt4b/9BthpWPNUy/CBk6V9iA0nHpzYzJFWO8v/tZFtES8CA==" UPSERT WHERE id="admin"  
```

 创建用户权限组（我们需要一个用户来上传我们的资源到私有仓库，但是要先创建权限组） 

![1571899337696](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571899337696.png)



 \#创建登陆用户（创建上传用户） 将用户添加到用户权限组

![1571899306591](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571899306591.png)

### **创建一个新的仓库**￠选择仓库类型（ proxy是代理仓库 ）

![1571899583244](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571899583244.png)

ulimit -HSn 65536

ulimit -n

### **创建私有仓库**

![1571899711347](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571899711347.png)

我们使用阿里云的仓库作为代理，去阿里云获取jar包资源

http://maven.aliyun.com/nexus/content/groups/public



**将创建的仓库加入到maven-public组中**保存退出

![1571899753934](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571899753934.png)

![1571899916400](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571899916400.png)

 



开一台maven

vim /usr/local/maven/conf/settings.xml 

添加：

```
 <mirror>
        <id>nexus kgcconf</id>
        <name>nexus-kgcconf</name>
        <url>http://192.168.1.8:8081/repository/maven-public/</url>
        <mirrorOf>*</mirrorOf>
        </mirror>
```

![1571901349658](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571901349658.png)

**生成一个MAVEN项目**

mvn archetype:generate -DgroupId=com.kgc.kgcweb -DartifactId=kgcweb -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false









vim /usr/local/maven/conf/settings.xml 

```
 <profile>
      <id>kgcconf</id>
      <repositrises>
        <repository>
          <id>nexus</id>
          <name>public</name>
          <url>http://192.168.1.8:8081/repository/maven-public/</url>
        </repository>
      </repositrises>
      <pluginRepositories>
        <pluginRepository>
          <id>nexus</id>
          <name>plugin</name>
          <url>http://192.168.1.8:8081/repository/maven-public/</url>
        </pluginRepository>
      </pluginRepositories>
    </profile>
```

<img src="C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571905518842.png" alt="1571905518842" style="zoom: 50%;" />

激活才能使用

![1571904586331](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571904586331.png)



```
     <server>
       <id>nexus</id>
       <username>kgcdev</username>
       <password>admin123</password>
     </server>
```

![1571904726438](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571904726438.png)



写入上传的仓库在那

vim /tmp/test/kgcweb/pom.xml 

```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <distributionManagement>
   <snapshotRepository>
    <id>nexus</id>
    <name>Snapshot Repository</name>
    <url>http://192.168.1.8:8081/repository/maven-snapshots/</url>
   </snapshotRepository>
   <repository>
    <id>nexus</id>
    <name>Release Repository</name>
    <url>http://192.168.1.8:8081/repository/maven-releases/</url>
   </repository>
  </distributionManagement>
```

![1571905767789](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571905767789.png)



打包

mvn clean 

上传







如果上传第三方插件





用户认证：







```
 <server>
          <id>111</id>
           <username>kgcdev</username>
           <password>admin123</password>
     </server>

另一个更改地方：

<repository>
               <id>111</id>
              <name>111</name>
           <url>http://192.168.1.8:8081/repository/maven-public/</url>
         </repository>
```

![1571907722194](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571907722194.png)

![1571907785061](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571907785061.png)





