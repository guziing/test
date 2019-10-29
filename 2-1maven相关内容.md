**maven相关内容**

> >>功能

1. maven模型的java项目对源代码、单元测试代码、资源、jar包等有规范的目录规划
2. 解决项目见的依赖关系、版本不一致、版本冲突问题
3. 合理的jar包管理机制

**应用场景---从运维角度理解**

1. 原来项目中的jar包必须手动复制、粘贴到WEB-INF/1ib项目下而借助maven.可以将jar包仅仅保存在仓库中，有需要使用的工程只需要引用这个文件，并不需要重复复制到工程中

2. 原来的项目中所需要的jar包都是提前下载好的，而maven在联网状态下会自动下载所需要的jar包。首先在本地仓库中找，找不到就在网上进行下载

3. 原来的项目中一个jar包所依赖的其他jar包必须手动导进来，而maven会自动将依赖的jar包导进来

   **目录结构	举例：/tem/kgcapp**

   #### **目录**									**说明**

   /tmp/kgcapp 							项目的根目录，存在pom. xml和所有的子目录

   tmp/kgcapp/src/ main/resources		项目的资源，比如说property文件，

   ​								springmvc . xml

   /tmp/kgcapp/src/test/java 		项目的测试类，比如说Junit代码

   tmp/ kgcapp/src/test/ resources 		测试用例	用的资源

   tmp/ kgcapp/ src/main/webapp/WEB INE 		web应用文件目录，比如存放web.xml、本地图片、jsp视图页面

   ​		**默认情况没有target目录**

   / tmp/ kgcapp/target								打包输出目录，如打包好的jar或war文件

   / tmp/ kgcapp/ target/classes				 编译输出目录用于保存输出的class文件

   / tmp/ kgcapp/target/test-classes		 测试编译输出目录

**pom. xml文件说明**

> ```
> project:根，这是对Project添加一些根元素的约束
> modelversion:指定当前maven模型的版本号
> > packaging:指定生 成包的格式(jar/war/rar/pom/eat)
> parent标签: 指定父级pom文件引用(继承)
> properties: 定义配置属性、约定开发规范
> dependencies标签:指定开发构建(jar包)
> >grouopId: 应该是公司名或者组织名。一般来 说groupID
> 有三个部分组成，每个部分之间以“."分隔，
> 第一部分是项目的用途，比如用于商业的就是com,用于非商业盈利性组织的就是org,
> 第二部分是公司名，比如tecent/baidu/alibaba, 
> 第三部分是你的项目名
> >arti factId:
> 可以认为是maven构建的项目名，比如你的项
> 目中有子项目，就可以使用“项目名一子项目的命名方式
> version:版本号，SNAPSHOT意为快照，说明该项目还在开发中，是不稳定的版本
> ```

关于仓库(本地仓---远程仓库)

**本地仓库**

maven会将工程依赖的构件(iar包) 从远程下载到本机一个目录下管理，每个电脑默认的仓库是在“用户家目录下的.m2/ repository"

**第三方仓库**

第三方仓库又称为内部中心仓库，也成为私服

私服:一般是由公司自己设立的，只为本公司内部共享使用。

它既可以作为公司内部构件协作和存档，也可以作为公用类

库镜像缓存，减少在外部访问和下载的频率(使用了私服就减少了对中央仓库的访问)

**中央仓库**

maven官方仓库

内置了远程公用仓库:

http://repo1.maven.org/maven2这个公共仓库是由maven自己维护。里面有大量的常用类库，并包含了世界上大部分的流行的开源项目构建，目前是以java为主，工程需要的jar包如果本地仓库没有，默认从中央仓库找

