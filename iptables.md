##  **iptables** 

# 

# iptables防火墙规则整理

​                        ![img](https://ucc.alicdn.com/avatar/avatar3.jpg)              [ 技术小阿哥](https://m.aliyun.com/users/grng7az2sxifq)                        2017-11-27          

​                                    [服务器](https://m.aliyun.com/tags/tagid_372/)                                    [防火墙](https://m.aliyun.com/tags/tagid_441/)                                    [模块](https://m.aliyun.com/tags/tagid_572/)                                    [配置](https://m.aliyun.com/tags/tagid_698/)                                    [input](https://m.aliyun.com/tags/tagid_2765/)                                    [路由器](https://m.aliyun.com/tags/tagid_2929/)                                

**iptables防火墙规则整理**



iptables是组成Linux平台下的包过滤防火墙，与大多数的Linux软件一样，这个包过滤防火墙是免费的，它可以代替昂贵的商业防火墙解决方案，完成封包过滤、封包重定向和网络地址转换(NAT)等功能。在日常Linux运维工作中，经常会设置iptables防火墙规则，用来加固服务安全。以下对iptables的规则使用做了总结性梳理：



iptables由两部分组成： 

1.framework：netfilter hooks function钩子函数，实现网络过滤器的基本框架。 

2.rule utils：iptables 规则管理工具



总体说来，iptables就是由“四表五链”组成。 

四表：



​		filter：过滤，防火墙

​		nat ：network address translation 网络地址转换

​		mangle：拆解报文，作出修改，封装报文

​		raw： 关闭nat表上启用的链接追踪机制

五链：



​		PREROUTING 数据包进入路由之前

​		INPUT 目的地址为本机

​		FORWARD 实现转发

​		OUTPUT 原地址为本机，向外发送

​		POSTROUTING 发送到网卡之前

[![wKiom1nKCOaTRK7SAADROJlJ-cs392.jpg](https://s2.51cto.com/wyfs02/M01/07/75/wKiom1nKCOaTRK7SAADROJlJ-cs392.jpg)](https://yq.aliyun.com/go/articleRenderRedirect?url=https%3A%2F%2Fs2.51cto.com%2Fwyfs02%2FM01%2F07%2F75%2FwKiom1nKCOaTRK7SAADROJlJ-cs392.jpg)

\------------------------------------------------------------------------------------------

**一、iptables首先需要了解的：**

1）规则概念

   规则（rules）其实就是网络管理员预定义的条件，规则一般的定义为“如果数据包头符合这样的条件，就这样处理这个数据包”。规则存储在内核空间的信息 包过滤表中，这些规则分别指定了源地址、目的地址、传输协议（如TCP、UDP、ICMP）和服务类型（如HTTP、FTP和SMTP）等。

 当数据包与规则匹配时，iptables就根据规则所定义的方法来处理这些数据包，如放行(accept),拒绝(reject)和丢弃(drop)等。配置防火墙的主要工作是添加,修改和删除等规则。

 其中：

  匹配（match）：符合指定的条件，比如指定的 IP 地址和端口。

  丢弃（drop）：当一个包到达时，简单地丢弃，不做其它任何处理。

  接受（accept）：和丢弃相反，接受这个包，让这个包通过。

  拒绝（reject）：和丢弃相似，但它还会向发送这个包的源主机发送错误消息。这个错误消息可以指定，也可以自动产生。

  目标（target）：指定的动作，说明如何处理一个包，比如：丢弃，接受，或拒绝。

  跳转（jump）：和目标类似，不过它指定的不是一个具体的动作，而是另一个链，表示要跳转到那个链上。

  规则（rule）：一个或多个匹配及其对应的目标。



2）iptables和netfilter的关系： 

   Iptables和netfilter的关系是一个很容易让人搞不清的问题。很多的知道iptables却不知道  netfilter。其实iptables只是Linux防火墙的管理工具而已，位于/sbin/iptables。真正实现防火墙功能的是  netfilter，它是Linux内核中实现包过滤的内部结构。



3）iptables的规则表和链

  表（tables）：提供特定的功能，iptables内置了4个表，即filter表、nat表、mangle表和raw表，分别用于实现包过滤，网络地址转换、包重构(修改)和数据跟踪处理。

   链（chains）：是数据包传播的路径，每一条链其实就是众多规则中的一个检查清单，每一条链中可以有一  条或数条规则。当一个数据包到达一个链时，iptables就会从链中第一条规则开始检查，看该数据包是否满足规则所定义的条件。如果满足，系统就会根据  该条规则所定义的方法处理该数据包；否则iptables将继续检查下一条规则，如果该数据包不符合链中任一条规则，iptables就会根据该链预先定 义的默认策略来处理数据包。



Iptables采用“表”和“链”的分层结构，在Linux中现在是四张表五个链。下面罗列一下这四张表和五个链。

[![wKioL1nKC9CBg5YkAAE0Flvwx5Y128.png](https://s5.51cto.com/wyfs02/M00/A6/27/wKioL1nKC9CBg5YkAAE0Flvwx5Y128.png)](https://yq.aliyun.com/go/articleRenderRedirect?url=https%3A%2F%2Fs5.51cto.com%2Fwyfs02%2FM00%2FA6%2F27%2FwKioL1nKC9CBg5YkAAE0Flvwx5Y128.png)

规则表：

  1）filter表——三个链：INPUT、FORWARD、OUTPUT

作用：过滤数据包 内核模块：iptables_filter.

  2）Nat表——三个链：PREROUTING、POSTROUTING、INPUT、OUTPUT

作用：用于网络地址转换（IP、端口） 内核模块：iptable_nat

  3）Mangle表——五个链：PREROUTING、POSTROUTING、INPUT、OUTPUT、FORWARD

作用：修改数据包的服务类型、TTL、并且可以配置路由实现QOS内核模块：iptable_mangle(别看这个表这么麻烦，咱们设置策略时几乎都不会用到它)

  4）Raw表——两个链：OUTPUT、PREROUTING

作用：决定数据包是否被状态跟踪机制处理 内核模块：iptable_raw



规则链：

  1）INPUT——进来的数据包应用此规则链中的策略

  2）OUTPUT——外出的数据包应用此规则链中的策略

  3）FORWARD——转发数据包时应用此规则链中的策略

  4）PREROUTING——对数据包作路由选择前应用此链中的规则

（记住！所有的数据包进来的时侯都先由这个链处理）

  5）POSTROUTING——对数据包作路由选择后应用此链中的规则

（所有的数据包出来的时侯都先由这个链处理）





iptables数据包报文的处理过程分为三种类型：

​			流入本机访问某进程的数据报文：

​				PREROUTING --> INPUT

​			由本机某进程发送出去的数据报文：

​				PREROUTING --> OUTPUT --> POSTROUTING

​			经由本机转发的数据报文：

​				PREROUTING --> FORWARD --> POSTROUTING



\------------------------------------------------------------------------------------------

**二、接下来说下iptables，chain规则设置用法及扩展匹配条件**



1）iptables的基本语法格式

​	iptables [-t table] COMMAND CHAIN [-m matchname [per-match-options]] -j targetname [per-target-options]

​	iptables [-t 表名] 命令选项 ［链名］ ［条件匹配］ ［-j 目标动作或跳转］

说明：

​	表名、链名：用于指定iptables命令所操作的表和链；

​	命令选项：用于指定管理iptables规则的方式（比如：插入、增加、删除、查看等；）

​	条件匹配：用于指定对符合什么样 条件的数据包进行处理；

​	目标动作或跳转：用于指定数据包的处理方式（比如允许通过、拒绝、丢弃、跳转（Jump）给其它链处理。）



2）iptables命令的管理控制选项

 链的操作命令：

​	-P：policy，策略，定义指定链的默认策略；一般有两种选择，即：ACCEPT或DROP；

​	-N：new chain，新建一条自定义的规则链；只有被内建链上的规则调用才能使自定义规则链上的规则生效；-j chain_name

​	-X：drop chain，删除被内建链引用次数为0的自定义链；

​	-F：flush，清除指定链上的所有规则；

​	-E：重命名被内建链引用次数为0的自定义链；



 规则的操作命令：

​	-A：append，追加，在指定的链的尾部追加一条规则；

​	-I [#]：insert，插入，在指定的位置插入一条规则，省略了数字表示将规则插入到链的第一条；

​	-D [#]：delete，删除，删除指定的规则

​	-R：replace，替换，用指定的规则去替换目标链中的原有规则；不能仅修改规则中的某一部分，而是整条规则完全替换；



 查看规则的命令：

​	-L：list，列出指定表指定链上的所有规则；

​	-n：numeric，将规则中的信息数字化显示，主要指：主机名和端口号；

​	-v：verbose，显示详细格式的信息，还有-vv，-vvv；

​	-x：exactly，精确的显示计数器的结果；

​	 --line-numbers：显示规则链中的规则编号；



 每条规则都有两个计数器：

​	1.匹配的报文的个数；

​	2.匹配的报文的总的字节数；



 重置规则计数器(将计数器计数归零)：

​	-Z：zero，将指定表指定链上的规则的计数器置0；



3）chain

​	1.内建链：

​	2.自定义链：



 基本匹配条件：

​	默认情况下，多个条件之间是逻辑"与"的关系；

​	!：指对匹配的条件取反；有除了...之意；



​	[!] -s, --source address[/mask][,...]

​		检查报文中的源IP地址是否符合此条件指定的地址或范围；

​	[!] -d, --destination address[/mask][,...]

​		检查报文中的目标IP地址是否符合此条件指定的地址或范围；

​	[!] -p, --protocol protocol

​		检查封装报文的协议是否符合此条件指定的协议；

​		protocol：tcp, udp, ip, icmp, arp, ...

​		只要是TCP/IP协议栈中，传输层和网络层的协议均可；

  tcp：

​	[!] --source-port,--sport port[:port]

​	[!] --destination-port,--dport port[:port]

​		指明此次匹配的源端口或目的端口；

​	[!] --tcp-flags mask comp

​		mask：要检查的标志位的列表，各标志位之间使用逗号分隔；

​		comp：必须置1的标志位列表，其余的在mask列表中的标志位必须为0；



​	TCP协议首部中的标志位：SYN，FIN，ACK，RST，PSH，URG



​	如：--tcp-flags SYN,ACK,FIN,RST SYN  相当于 --syn



  udp：

​	[!] --source-port,--sport port[:port]

​	[!] --destination-port,--dport port[:port]

 icmp：

​	[!] --icmp-type {type[/code]|typename}

​		 8: echo-request

​		 0: echo-reply



​	[!] -i, --in-interface name

​		检查数据报文入站的接口是否对应此条件中指定的接口；

​	[!] -o, --out-interface name

​		检查数据报文出站的接口是否对应此条件中指定的接口；



​	注意：在INPUT链上指定出站接口没有意义；在OUTPUT链上指定入站接口没有意义；



4）防火墙处理数据包的四种方式ACCEPT 允许数据包通过

​	DROP 直接丢弃数据包，不给任何回应信息

​	REJECT 拒绝数据包通过，必要时会给数据发送端一个响应的信息。

​	LOG在/var/log/messages文件中记录日志信息，然后将数据包传递给下一条规则



5）iptables防火墙规则的保存与恢复

  iptables-save把规则保存到文件中，再由目录rc.d下的脚本（/etc/rc.d/init.d/iptables）自动装载使用命令iptables-save来保存规则。一般用：iptables-save >  /etc/sysconfig/iptables生成保存规则的文件/etc/sysconfig/iptables，也可以用：service  iptables save它能把规则自动保存在/etc/sysconfig/iptables中。

  当计算机启动时，rc.d下的脚本将用命令iptables-restore调用这个文件，从而就自动恢复了规则。



6）扩展匹配条件：

 隐式扩展：

  -p tcp  

 显示扩展：

​	1.multiport扩展：

​	 以离散或连续的方式定义多个端口匹配条件：

​	 [!] --source-ports,--sports port[,port|,port:port]...

​	 [!] --destination-ports,--dports port[,port|,port:port]...

​	 [!] --ports port[,port|,port:port]...



​	 21,22,23,80,1000:2000



​	 示例：

​	~]# iptables -I INPUT -d 172.16.72.1 -s 172.16.0.0/16 -p tcp -m multiport --dports 21,22,23,80 -j ACCEPT



​	2.iprange扩展：

​	 以连续的IP地址范围指明连续的多个地址的匹配条件；

​	 [!] --src-range from[-to]

​	 [!] --dst-range from[-to]



​	 172.16.50.1-172.16.72.254



​	 示例：

​	 ~]# iptables -I INPUT 4 -m iprange --src-range 172.16.0.1-172.16.72.254 -p tcp -m multiport --dports 21,22,80 -j ACCEPT



​	3.string扩展：

​	 对报文中的应用层数据做字符串匹配检测；

​	 --algo {bm|kmp}：选择处理字符串的算法；

​	 [!] --string pattern：指明要检查匹配的字符串；



​	 示例：

​	 ~]# iptables -A OUTPUT -s 172.16.72.1 -d 172.16.0.0/16 -m string --string "admin" --algo kmp -j DROP



   4.time扩展：

​	 根据报文到达的时间与指定的时间范围进行匹配度检测；

​	 --datestart YYYY[-MM[-DD[Thh[:mm[:ss]]]]]

​	 --datestop YYYY[-MM[-DD[Thh[:mm[:ss]]]]]



​	 --timestart hh:mm[:ss]

​	 --timestop hh:mm[:ss]



​	 [!] --monthdays day[,day...]

​	 [!] --weekdays day[,day...]



​	 示例：

​	 ~]# iptables -I INPUT -d 172.16.72.1 -p tcp -m multiport --dports 21,23,80  -m time --timestart 09:00:00 --timestop 17:00:00 --weekdays Sat,Sun  --kerneltz -j ACCEPT



​	5.connlimit扩展:

​		 根据每个客户端IP做并发连接数的匹配；

​	 --connlimit-upto n：连接数数量小于等于n，此时规则应设置为允许；

​	 --connlimit-above n：连接数数量大于n，此时规则应设置为拒绝；



​	 示例：

​	 ~]# iptables -I INPUT -d 172.16.72.1 -p tcp --dport 23 -m connlimit --connlimit-upto 2 -j ACCEPT



​	6.limit扩展：

​	 基于收发报文的速率进行匹配；

​	 --limit rate[/second|/minute|/hour|/day]

​	 --limit-burst number



​	 示例：

​	 ~]# iptables -A INPUT -p icmp --icmp-type 8 -m limit --limit 20/minute --limit-burst 8 -j ACCEPT



​	 7.state扩展：

​	 状态检测：基于连接追踪机制实现；conntrack

​	 状态：

​	 INVALID：无法识别的状态；无效状态；

​	 ESTABLISHED：已建立连接的状态；连接态；

​	 NEW：尚未建立连接的状态，新连接态；

​	 RELATED：与其他已建立的连接相关联的状态；关联态或衍生态；

​	 UNTRACKED：未追踪的连接；



​	 追踪到的连接保存的位置：

​		 /proc/net/nf_conntrack



​	 能够被追踪到的最大连接数的定义：

​		 /proc/sys/net/nf_conntrack_max



​	 注意：此最大连接数的数值，建议必要时可以调整到足够大；



​	 不同协议的连接追踪的超时时间：

​		 /proc/sys/net/netfilter/*timeout



​	 示例：

​	~]# iptables -I INPUT -d 172.16.72.1 -m state --state ESTABLISHED,RELATED -j ACCEPT

​	~]# iptables -I INPUT 2 -d 172.16.72.1 -p tcp -m multiport --dports 21,22,23,80,3306 -m state --state NEW -j ACCEPT

​	~]# iptables -A INPUT -j DROP



   注意：默认的规则或最后一条规则拒绝所有主机访问；



​	~]# iptables -A OUTPUT -m state --state ESTABLISHED -j ACCEPT

   ~]# iptables -A OUTPUT -j DROP



​	 放行FTP被动模式的数据连接：

​		 1.装载追踪FTP协议的模块：

​		 # modprobe nf_conntrack_ftp



​		 也可以编辑vim /etc/sysconfig/iptables-config

​		 IPTABLES_MODULES="nf_conntrack_ftp"



\-----------------------------------------------------------------------------------------

**三、IPtables中可以灵活的做各种网络地址转换（NAT）**



网络地址转换主要有两种：SNAT和DNAT

1）SNAT是source network address translation的缩写，即源地址目标转换。

​		比如，多个PC机使用ADSL路由器共享上网，每个PC机都配置了内网IP。PC机访问外部网络的时候，路由器将数据包的报头中的源地址替换成路由器的ip，当外部网络的服务器比如网站web服务器接到访问请求的时候，它的日志记录下来的是路由器的ip地址，而不是pc机的内网ip，这是因为，这个服务器收到的数据包的报头里边的“源地址”，已经被替换了。所以叫做SNAT，基于源地址的地址转换



2）DNAT是destination network address translation的缩写，即目标网络地址转换。

​		典型的应用是，有个web服务器放在内网中，配置了内网ip，前端有个防火墙配置公网ip，互联网上的访问者使用公网ip来访问这个网站。

​		当访问的时候，客户端发出一个数据包，这个数据包的报头里边，目标地址写的是防火墙的公网ip，防火墙会把这个数据包的报头改写一次，将目标地址改写成web服务器的内网ip，然后再把这个数据包发送到内网的web服务器上。这样，数据包就穿透了防火墙，并从公网ip变成了一个对内网地址的访问了。

SNAT：

 --to-source [ipaddr[-ipaddr]][:port[-port]]

  注意：在RHEL系或CentOS系操作系统上，SNAT中指定ipaddr必须为当前主机已经配置的IP地址；

 示例：

  ~]# iptables -t nat -R POSTROUTING 1 -s 192.168.100.0/24 -j SNAT --to-source 172.16.72.50

 MASQUERADE

​	--to-ports port[-port]

 示例：

  ~]# iptables -t nat -R POSTROUTING 1 -s 192.168.100.0/24 -j MASQUERADE



DNAT：

 --to-destination [ipaddr[-ipaddr]][:port[-port]]

 示例：

  ~]# iptables -t nat -A PREROUTING -d 172.16.72.50 -j DNAT --to-destination 192.168.100.2

  ~]# iptables -t nat -A PREROUTING -d 172.16.72.50 -p tcp --dport 80 -j DNAT --to-destination 192.168.100.2:8077



REDIRECT：端口重定向

 --to-ports port[-port]

 ~]# iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8077

 LOG：

  仅仅是开启内核对匹配的数据包做额外的日志记录；

​	--log-level level

​	--log-prefix prefix

​	--log-ip-options

​          

​                   





