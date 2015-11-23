# tcp/ip 协议包头信息

## 以太网帧格式

下面是以太网以及802.3,802.2 LLC,802.2 SNAP 包头信息：
![以太网](http://yinyin-images.stor.sinaapp.com/%E4%BB%A5%E5%A4%AA%E7%BD%91.png)

### Ethertype code对照表

Ethertype code		| 协议 | 
------------		| ------------- 
0x0000 - 0x05DC		| EEE 802.3 长度  
Content Cell		| Content Cell 
0x0101 – 0x01FF		| 实验
0x0600				| XEROX NS IDP
0x0660				| XEROX NS IDP
0x0661				| DLOG
0x0800				| 网际协议（IP）
0x0801				| X.75 Internet
0x0802				| NBS Internet
0x0803				| ECMA Internet
0x0804				| Chaosnet
0x0805				| X.25 Level 3
0x0806				| 地址解析协议（ARP ： Address Resolution Protocol）
0x0808				| 帧中继 ARP （Frame Relay ARP） [RFC1701]
0x6559				| 原始帧中继（Raw Frame Relay） [RFC1701]
0x8035				| 动态 DARP （DRARP：Dynamic RARP） 反向地址解析协议 （RARP：Reverse Address Resolution Protocol）
0x8037				| Novell Netware IPX
0x809B				| EtherTalk
0x80D5				| IBM SNA Services over Ethernet
0x80F3				| AppleTalk 地址解析协议（AARP：AppleTalk Address Resolution Protocol）
0x8100				| 以太网自动保护开关（EAPS：Ethernet Automatic Protection Switching）
0x8137				| 因特网包交换（IPX：Internet Packet Exchange）
0x814C				| 简单网络管理协议（SNMP：Simple Network Management Protocol）
0x86DD				| 网际协议v6 （IPv6，Internet Protocol version 6）
0x880B				| 点对点协议（PPP：Point-to-Point Protocol）
0x880C				| 通用交换管理协议（GSMP：General Switch Management Protocol）
0x8847				| 多协议标签交换（单播） MPLS：Multi-Protocol Label Switching <unicast>）
0x8848				| 多协议标签交换（组播）（MPLS, Multi-Protocol Label Switching <multicast>）
0x8863				| 以太网上的 PPP（发现阶段）（PPPoE：PPP Over Ethernet <Discovery Stage>）
0x8864				| 以太网上的 PPP（PPP 会话阶段） （PPPoE，PPP Over Ethernet<PPP Session Stage>）
0x88BB				| 轻量级访问点协议（LWAPP：Light Weight Access Point Protocol）
0x88CC				| 链接层发现协议（LLDP：Link Layer Discovery Protocol）
0x8E88				| 局域网上的 EAP（EAPOL：EAP over LAN）
0x9000				| 配置测试协议（Loopback）
0x9100				| VLAN 标签协议标识符（VLAN Tag Protocol Identifier）
0x9200				| VLAN 标签协议标识符（VLAN Tag Protocol Identifier）
0xFFFF				| 保留


## ip数据包头部

下面是ip包头信息
![ip数据包头部](http://yinyin-images.stor.sinaapp.com/ip%20header.png)

- 版本:占4位,指IP协议的版本.通信双方使用的IP协议版本必须一致.日前广泛使用的 IP协议版本号为 4 (即 IPv4).IPv6 目前还处于起步阶段
- 首部长度:占 4 位,可表示的最大十进制数值是15.请注意,这个字段所表示数的单位是32位字 (1个32位字长是4 字节),因此,当 IP 的首部长度为 1111 时 (即十进制的 15),首部长度就达到 60字节.当 IP 分组的首部长度不是4字节的整数倍时,必须利用最后的填充字段加以填充.因此数据部分永远在 4字节的整数倍开始,这样在实现 IP协议时较为方便.首部长度限制为 60字节的缺点是有时可能不够用.这样做的目的是希望用户尽量减少开销.最常用的首部长度就是 20 字节 (即首部长度为 0101),这时不使用任何选项.
- 服务:占 8 位,用来获得更好的服务.这个字段在旧标准中叫做服务类型,但实际上一直没有被使用过.1998年IETF把这个字段改名为区分服务 DS(Differentiated Services).只有在使用区分服务时,这个字段才起作用.
- 总长度:总长度指首都及数据之和的长度,单位为字节.因为总长度字段为16位,所以数据报的最大长度为 216-1=65 535字节.在IP层下面的每一种数据链路层都有自己的帧格式,其中包括帧格式中的数据字段的最大长度,即最大传送单元 MTU (Maximum Transfer Unit).当一个数据报封装成链路层的帧时,此数据报的总长度 (即首部加上数据部分)一定不能超过下面的数据链路层的MTU值,否则要分片.
- 标识 (Identification):占 16位.IP软件在存储器中维持一个计数器,每产生一个数据报,计数器就加 1,并将此值赋给标识字段.但这个"标识"并不是序号,因为 IP是无连接的服务,数据报不存在按序接收的问题.当数据报由于长度超过网络的 MTU 而必须分片时,这个标识字段的值就被复制到所有的数据报的标识字段中.相同的标识字段的值使分片后的各数据报片最后能正确地重装成为原来的数据报.
- 标志 (Flag):占3 位,但目前只有2位有意义. 标志字段中的最低位记为 MF(More Fragment).MF=1即表示后面"还有分片"的数据报.MF=0表示这已是若干数据报片中的最后一个.标志字段中间的一位记为DF(Don't Fragment),意思是"不能分片",只有当 DF=0时才允许分片.
- 片偏移:占 13位.较长的分组在分片后,某片在原分组中的相对位置.也就是说,相对用户数据字段的起点,该片从何处开始.片偏移以 8个字节为偏移单位,这就是说,每个分片的长度一定是 8字节(64位)的整数倍.
- 生存时间:占 8位,生存时间字段常用的英文缩写是TTL(Time To Live),其表明数据报在网络中的寿命.由发出数据报的源点设置这个字段.其目的是防止无法交付的数据报无限制地在因特网中兜圈子,因而白白消耗网络资源.最初的设计是以秒作为 TTL的单位.每经过一个路由器时,就把TTL减去数据报在路由器消耗掉的一段时间.若数据报在路由器消耗的时间小于 1 秒,就把TTL值减 1.当 TTL值为 0时,就丢弃这个数据报.
- 协议:占 8 位.协议字段指出此数据报携带的数据是使用何种协议,以便使目的主机的IP层知道应将数据部分上交给哪个处理过程.详细资料请看文章最后的注释.
- 首部检验和:占 16位.这个字段只检验数据报的首部,但不包括数据部分.这是因为数据报每经过一个路由器,都要重新计算一下首都检验和 (一些字段,如生存时间,标志,片偏移等都可能发生变化),不检验数据部分可减少计算的工作量.
- 源地址:占32位.
- 目的地址:占 32位.
- IP数据报首部的可变部分：IP首部的可变部分就是一个可选字段.选项字段用来支持排错,测量以及安全等措施,内容很丰富.此字段的长度可变,从1个字节到40个字节不等,取决于所选择的项目.某些选项项目只需要1个字节,它只包括1个字节的选项代码.但还有些选项需要多个字节,这些选项一个个拼接起来,中间不需要有分隔符,最后用全0的填充字段补齐成为4字节的整数倍.增加首部的可变部分是为了增加IP数据报的功能,但这同时也使得IP数据报的首部长度成为可变的.这就增加了每一个路由器处理数据报的开销,实际上这些选项很少被使用.新的IP版本IPv6就将IP数据报的首部长度做成固定的.


### 协议字段对照表
code				| 协议 
------------		| -------------
0		| 保留字段，用于IPv6(跳跃点到跳跃点选项) 
1 		| Internet控制消息 
2		| Internet组管理 
3 		| 网关到网关 
4 		| IP中的IP(封装) 
5 		| 流 
6 		| 传输控制 
7 		| CBT 
8 		| 外部网关协议 
9 		| 任何私有内部网关(Cisco在它的IGRP实现中使用) 
10 		| BBNRCC监视 
11 		| 网络语音协议 
12 		| PUP 
13 		| ARGUS 
14 		| EMCON 
15 		| 网络诊断工具 
16 		| 混乱(Chaos) 
17 		| 用户数据报文 
18 		| 复用 
19 		| DCN测量子系统 
20 		| 主机监视 
21 		| 包无线测量 
22 		| XEROXNSIDP 
23 		| Trunk-1 
24 		| Trunk-2 
25 		| leaf-1 
26 		| leaf-2 
27 		| 可靠的数据协议 
28 		| Internet可靠交易 
29 		| ISO传输协议第四类 
30 		| 大块数据传输协议 
31 		| MFE网络服务协议 
32 		| MERIT节点之间协议 
33 		| 序列交换协议 
34 		| 第三方连接协议 
35 		| 域之间策略路由协议 
36 		| XTP 
37 		| 数据报文传递协议 
38 		| IDPR控制消息传输协议 
39 		| TP+ +传输协议 
40 		| IL传输协议 
41 		| IPv6 
42 		| 资源命令路由协议 
43 		| IPv6的路由报头 
44 		| IPv6的片报头 
45 		| 域之间路由协议 
46 		| 保留协议 
47 		| 通用路由封装 
48 		| 可移动主机路由协议 
49 		| BNA 
50 		| IPv6封装安全有效负载 
51 		| IPv6验证报头 
52 		| 集成的网络层安全TUBA 
53 		| 带加密的IP 
54 		| NBMA地址解析协议 
55 		| IP可移动性 
56 		| 使用Kryptonet钥匙管理的传输层安全协议 
57 		| SKIP 
58 		| IPv6的ICMP 
59 		| IPv6的无下一个报头 
60 		| IPv6的信宿选项 
61 		| 任何主机内部协议 
62 		| CFTP 
63 		| 任何本地网络 
64 		| SATNET和BackroomEXPAK 
65 		| Kryptolan 
66 		| MIT远程虚拟磁盘协议 
67 		| Internet Pluribus包核心 
68 		| 任何分布式文件系统 
69 		| SATNET监视 
70 		| VISA协议 
71 		| Internet包核心工具 
72 		| 计算机协议Network Executive 
73 		| 计算机协议Heart Beat 
74 		| Wang Span网络 
75 		| 包视频协议 
76 		| Backroom SATNET监视 
77 		| SUN ND PROTOCOL—临时 
78 		| WIDEBAND监视 
79 		| WIDEBAND EXPAK 
80 		| ISO Internet协议 
81 		| VMTP 
82 		| SECURE—VMTP(安全的VMTP) 
83 		| VINES 
84 		| TTP 
85 		| NSFNET—IGP 
86 		| 不同网关协议 
87 		| TCF 
88 		| EIGRP 
89 		| OSPFIGP 
90 		| Sprite RPC协议 
91 		| Locus地址解析协议 
92 		| 多播传输协议 
93 		| AX.25帧 
94 		| IP内部的IP封装协议 
95 		| 可移动网络互连控制协议 
96 		| 旗语通讯安全协议 
97 		| IP中的以太封装 
98 		| 封装报头 
99 		| 任何私有加密方案 
100		| GMTP 
101 	| Ipsilon流量管理协议 
102 	| PNNI over IP 
103 	| 协议独立多播 
104 	| ARIS 
105 	| SCPS 
106 	| QNX 
107 	| 活动网络 
108 	| IP有效负载压缩协议 
109 	| Sitara网络协议 
110 	| Compaq对等协议 
111 	| IP中的IPX 
112 	| 虚拟路由器冗余协议 
113 	| PGM可靠传输协议 
114 	| 任何0跳跃协议 
115 	| 第二层隧道协议 
116 	| D-II数据交换(DDX) 
117 	| 交互式代理传输协议 
118 	| 日程计划传输协议 
119 	| SpectraLink无线协议 
120 	| UTI 
121 	| 简单消息协议 
122 	| SM 
123 	| 性能透明性协议 
124 	| ISIS over IPv4 
125 	| FIRE 
126 	| Combat无线传输协议 
127 	| Combat无线用户数据报文 
128 	| SSCOPMCE 
129 	| IPLT 
130 	| 安全包防护 
131 	| IP中的私有IP封装 
132 	| 流控制传输协议 
133～254	| 未分配 
255 	| 保留 


## arp 协议
![arp header](http://yinyin-images.stor.sinaapp.com/arp%20header.png)

- 以太网目的地址就收接受端的MAC地址，如果是全1表示是一个广播地址。
- 源MAC地址,这是请求端(发送端)的MAC地址。
- 两个字节长的以太网帧类型表示后面数据的类型。对于ARP请求或应答来说,该字段的值为0x0806 。
- 硬件类型和协议类型用来描述ARP分组中的各个字段，也就是ARP请求询问协议地址对应的硬件地址，硬件类型为1表示以太网，协议类型为0x0800表示ip协议
- 硬件地址长度，如果使用以太网硬件地址长度为6字节
- 协议地址长度，如果使用ip协议，协议地址长度为4字节
- 操作类型,在报文中占2个字节,1表示ARP请求,2表示ARP应答,3表示RARP请求,4表示RARP应答
- 发送方(源)MAC地址
- 发送方(源)IP地址
- 目的MAC地址
- 目的IP地址


## ICMP 协议
ICMP是IP层的一个组成部分，它传递查询报文和差错报文，ICMP报文通常被IP层或更高层协议（TCP
或UDP）使用，它是在IP数据包内被传输的

![ip-icmp](http://yinyin-images.stor.sinaapp.com/ip-icmp.png)
![icmp header](http://yinyin-images.stor.sinaapp.com/ICMP%20header.png)

### ICMP报文的类型

ICMP分为查询报文和差错报文，可以通过下面的列表看到：

![ICMP报文的类型](http://yinyin-images.stor.sinaapp.com/icmp%20%E7%B1%BB%E5%9E%8B.png)

下面各种情况都不会导致产生ICMP差错报文,这些规则是为了防止过去允许ICMP差错报文对广播分组响应所带来的广播风暴:

- ICMP差错报文(但是,ICMP查询报文可能会产生ICMP差错报文)。
- 目的地址是广播地址(见图3-9)或多播地址(D类地址,见图1-5)的IP数据报。
- 作为链路层广播的数据报。
- 不是IP分片的第一片(将在11.5节介绍分片)。
- 源地址不是单个主机的数据报。这就是说,源地址不能为零地址、环回地址、广播地址或多播地址。

### ICMP地址掩码请求与应答

ICMP地址掩码请求用于无盘系统在引导过程中获取自己的子网掩码。系统广播它的ICMP请求报文（这一
过程与无盘系统在引导过程中用RARP获取IP地址是类似的）。无盘
系统获取子网掩码的另一个方法是BOOTP协议。

![ICMP地址掩码请求与应答](http://yinyin-images.stor.sinaapp.com/ICMP%E5%9C%B0%E5%9D%80%E6%8E%A9%E7%A0%81%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%BA%94%E7%AD%94.png)

ICMP报文中的标识符和序列号字段由发送端任意选择设定，这些值在应答中将被返回。这样，发送端就可以
把应答与请求进行匹配。


### ICMP时间戳请求与应答

ICMP时间戳请求允许系统向另一个系统查询当前的时间。返回的建议值是自午夜开始计算的毫秒数，协调的统一时间（Coordinated Universal Time, UTC）
![ICMP时间戳请求与应答](http://yinyin-images.stor.sinaapp.com/ICMP%E6%97%B6%E9%97%B4%E6%88%B3%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%BA%94%E7%AD%94.jpg)

请求端填写发起时间戳，然后发送报文。应答系统收到请求报文时填写接收时间戳，在发送应答时填写发送时间戳。但是，实际上，大多数的实现把后面两个字段都设成相同的值。

### ICMP端口不可达差错

ICMP的一个规则是， ICMP差错报文必须包括生成该差错报文的数据报IP首部（包含任何选项），还必须至少包括跟在该IP首部后面的前8个字节（包含源端口和目的端口）。在我们的例子中，跟在IP首部后面的前8个字节包含UDP的首部。

![ICMP端口不可达差错](http://yinyin-images.stor.sinaapp.com/ICMP%E7%AB%AF%E5%8F%A3%E4%B8%8D%E5%8F%AF%E8%BE%BE%E5%B7%AE%E9%94%99.jpg)

### ICMP不可达报文
![ICMP不可达报文](http://yinyin-images.stor.sinaapp.com/ICMP%E4%B8%8D%E5%8F%AF%E8%BE%BE%E6%8A%A5%E6%96%87%E7%9A%84%E4%B8%80%E8%88%AC%E6%A0%BC%E5%BC%8F.png)



## 参考
http://technow.blog.51cto.com/746816/320773

http://blog.csdn.net/ce123/article/details/17453033

