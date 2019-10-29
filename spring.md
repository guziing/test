基于两台maven

yum -y install git

 **克隆单个仓库** 

git clone https://github.com/luojunyong/spring-cloud-examples.git

 

tree指定文件夹的所以文件用树状罗列出来 （不用安装）

yum -y install tree

cd /root/spring-cloud-examples/eureka-producer-consumer/spring-cloud-eureka/

配置文件

vim src/main/resources/application.properties 

```
spring.application.name=spring-cloud-eureka	//当前应用名称是什么
server.port=8000							//运行端口
eureka.client.register-with-eureka=true		//当前服务要不要注册到注册中心
eureka.client.fetch-registry=true		//当前服务要不要从注册中心获取信息
eureka.client.serviceUrl.defaultZone=http://192.168.1.8:8000/eureka/
	//指定一下注册中心的url
```

打包

mvn clean package

java -jar target/spring-cloud-eureka-0.0.1-SNAPSHOT.jar 

firefox http://192.168.1.8:8000/

登陆页面

![1571994032662](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571994032662.png)



**做注册中心的集群**

修改配置文件

vim src/main/resources/application.properties 

修改为两个注册中心的url即可

![1571994162903](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571994162903.png)



第二台主机相同操作（IP注意）



![1571995418596](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1571995418596.png)

看网页现在运行是两台





第一台主机操作

```
 cd /root/spring-cloud-examples/eureka-prucer-consumer/spring-cloud-producer/

vim src/main/resources/application.properties

mvn clean package

 java -jar target/spring-cloud-eureka-0.0.1-SNAPSHOT.jar	（尽量spring之后table执行）
```





第二台操作：

```
cd  /root/spring-cloud-examples/eureka-producer-consumer/spring-cloud-producer

vim src/main/java/com/neo/controller/HelloController.java 

修改一下测试页面内容（下图一）

修改配置文件：

 vim src/main/resources/application.properties 
 修改一下IP（下图二）
```

![1572242181573](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572242181573.png)



![1572242319812](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572242319812.png)

修改完成后进行打包

mvn clean package

运行：

java -jar target/spring-cloud-producer-0.0.1-SNAPSHOT.jar（尽量spring之后table执行）



完成后看页面

![1572242796443](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572242796443.png)



另开终端

**消费者：**

```
cd /root/spring-cloud-examples/eureka-producer-consumer/spring-cloud-consumer

vim src/main/resources/application.properties 

打包 mvn clean package
运行：
java -jar target/spring-cloud-consumer-0.0.1-SNAPSHOT.jar
```



![1572244567618](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572244567618.png)



curl http://192.168.1.8:9000/hello?name=tiechui

curl http://192.168.1.9:9000/hello?name=tiechui



curl 192.168.1.8:9001/heelo/（上面访问起的名字）

curl 192.168.1.8:9001/heelo/tiechui（刷新可轮询）

![1572245156243](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572245156243.png)

看完可关闭该进程



cd /root/spring-cloud-examples/spring-cloud-hystrix/spring-cloud-consumer-hystrix/

vim src/main/resources/application.properties 

![1572246023684](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572246023684.png)

mvn clean package

java -jar target/spring-cloud-consumer-hystrix-0.0.1-SNAPSHOT.jar

http://192.168.1.8:9001/hello/aaa		（aaa是随便起的名字）

检测可轮询

![1572246208427](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572246208427.png)

![1572246220827](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572246220827.png)



端口

9001是消费者

9000是生产者

8000是注册中心



### **微服务特点：**

1. 访问某个微服务时，http://ip:ort/application name/methood
2. 做访问认证

**第一台主机：（网关）**

cd /root/spring-cloud-examples/spring-cloud-zuul/spring-cloud-zuul/

vim src/main/resources/application.properties （修改注册中心）

![1572249763802](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572249763802.png)

```
mvn spring-boot:run

 curl '192.168.1.8:8888/spring-cloud-producer/hello?name=mario&token=123'
```

暂时停止除了注册中心所有的进程

第一台：（封装）

```
cd /root/spring-cloud-examples/spring-cloud-sleuth-zipkin/zipkin-server/

vim src/main/resources/application.yml 

修改集群IP及其端口

打包及运行：

 mvn spring-boot:run
```



**再打开一台终端：**

```
cd /root/spring-cloud-examples/spring-cloud-sleuth-zipkin/spring-cloud-zuul/

vim src/main/resources/application.yml

mvn spring-boot:run
```

![1572250162557](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572250162557.png)

打包及运行：



再开启一台终端：（跟踪）

```
cd /root/spring-cloud-examples/spring-cloud-sleuth-zipkin/spring-cloud-producer/

vim src/main/resources/application.yml 
mvn spring-boot:run
```

![1572250466079](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572250466079.png)



curl 'http://192.168.1.8:8888/producer/hello?name=wpy'

http://192.168.1.8:9000/zipkin/

![1572252057360](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572252057360.png)





**spring boot admin**

 cd /root/spring-cloud-examples/spring-boot-admin-eureka/spring-cloud-eureka/

 mvn spring-boot:run

新终端

d /root/spring-cloud-examples/spring-boot-admin-eureka/spring-cloud-producer



cd /root/spring-cloud-examples/spring-boot-admin-eureka/spring-cloud-producer-2/

mvn spring-boot:run

新终端/：

 cd /root/spring-cloud-examples/spring-boot-admin-eureka/spring-boot-admin-server/

vim src/main/resources/application.yml 

![1572254358755](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1572254358755.png)

