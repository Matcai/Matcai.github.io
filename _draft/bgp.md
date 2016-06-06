# BGP 路由协议

### BGP 的基本特性

1. BGP 是一个外部网关协议，即EGP，与RIP，OSPF，IS-IS 等 IGP 协议不同的是  
   BGP 的任务不在于发现和计算路由，而在于选择不同AS间 的最佳路由，  
   并控制路由的传播。
2. BGP 使用TCP 协议的179 端口进行通讯，提高了协议的可靠性。  
   BGP 是4层的路由协议。
3. BGP 支持CIDR 技术，以实现超网的汇总。
4. BGP 网络更新在初始化后仅发送更新的路由。
5. 携带AS 路劲信息用于解决环路问题，同时提供丰富的路由策略。  
6. 扩展性强，支持ipv4 、ipv6 、 以及ISO 网络的CLNS 三层协议。

### BGP 的AS （自治系统）

> BGP 的AS 概念与 OSPF 、 IS-IS 相同。  
> BGP 的AS 用于划分整个外部网络为一个个应用本地路由策略的路由子域。  
> BGP 每个子域可以支持多个不同的路由协议，但是每个路由协议都是独立管理的  
> 其他路由协议可以通过重发布（redistribution） 功能可以与BGP 路由动态交换路由信息。  

- 不同AS 的BGP 路由器需要通过eBGP对等会话交换路由信息
- 同一个AS 内部的BGP 路由器之间通过iBGP 对等会话交换路由信息
- BGP 是基于Internet 的公网连接，所以它的AS 应用与内部网络不一样
- 在公网使用的AS 称之为（公用AS），必须是在公网注册的，由ISP 统一分配
- 公网AS 具有唯一性
- iBGP 的AS 不需要进行公网注册，不具有唯一性。用于公司内部网络的使用。


### BGP 的AS 号码格式

> BGP 主要应用与庞大的Internet，所以需要的AS 号非常多，  
> 必须保证其唯一性

- Asplain （无格式的AS）

> Asplain AS 号的格式是一个十进制记数法所表示的十进制值，  
> 可以是2 个字节，也可以是4 个字节。  
> 例如： 65526 是 2 个字节的AS 号，234523 则是 4 个字节的AS号  
> **BPG 默认使用Asplain AS 号码格式。**  

- `bgp asnotation dot` 命令可以修改AS 号码格式  
  cisco 默认使用Asplain 的方式显示，
- Asdot （点分AS）

> Asdot 采用点分记数法所表示的十进制值  
> 如果是2 个字节的AS 号，则直接用十进制表示。  
> 如果是4 个字节的AS 号，则采用点分计数法。  
> 例如：234567 是 4 个字节的AS 号，则表示为  
> 1.169031 计算方法  
> 将十进制转换成二进制，11010001111100111  
> 然后从右往左边开始，2个字节分为一段，即16位  
> 以点分开，最后转换成十进制。


- 保留和私有的AS 号。

> RFC 4893 BGP 标准中支持2 个字节AS 向4 个字节的过渡。  
> 新增了保留AS 号23456， 不能在ISO CLI 下配置使用。  
> RFC 5389 中又规定了新的保留AS 号，它们是64956～64511 之间  
> 的 2个字节AS 号和65536～65551 直接的4 个字节AS 号。  

> **私有AS 号：64512～65534 之间的2个字节为私有。**


### BGP speaker 与 peer 的关系

- BGP speaking 、speaker

1. 在BGP 网络中，所有参与BGP 进程的路由器称之为BGP-speaking 路由器  
   （可以看做是bgp 会话的意思）
2. 每个BGP-speaking 不能自动发现其他的BGP-speaking设备。
3. 需要通过手动的配置BGP-speaking 之间的关联。
4. BGP-speaker是指本地BGP 路由器
5. BGP-speaking 是指任何其他的开启BGP进程的路由器

- BGP peer

1. BGP peer 设备是指其他BGP-speaking 之间存在一个活动的TCP  
   连接 的BGP-speaking路由器设备。
2. 两者之间可以看做是邻居。
3. 邻居的建立是通过TCP 单播建立，两台BGP 路由器直连不一定是邻居。
4. 两台BGP 路由器直连并且通过TCP 建立了bgp的会话，可以称之为peer（对端）的关系描述。
5. BGP-speaker 与 BGP-speaking 建立BGP TCP 活动会话后称之为peer
6. peer 之间交换keepalive（存活）消息，用于保持会话连接。
7. 相同AS 的peer 称之为内部peer，不同AS 的peer 称之为外部peer


- BGP Peer 的会话建立过程

1. Idle（空闲）
> 路由进程启动或者路由器复位时，进入路由进程的初始状态。  
> BGP 路由进程处于等待远程Peer 的TCP 连接请求的状态。

2. connect （连接）
> BGP 进程检测到一个Peer 正在试图与本地BGP speaker  
> 建立一个TCP 会话连接。

3. Active （活动）
> 在这个状态下，本地的BGP speaker 的BGP 路由进程试图  
> 使用重试连接计时器（ConnectRetry timer） 与一个Peer路由器  
> 建立TCP 会话连接。  
> **一旦该路由器处于Active** 的状态，来自远程的Peer 连接启动事件  
> 将被忽略，不会响应远程Peer 的任何TCP 重新连接请求。  
> 如果路由进程被重置或发生错误，将回到Idle 状态

4. OpenSent（打开发送）
> TCP 连接建立好之后，BGP 路由进程将发送一个open 的消息到远程Peer  
> 进入OpenSent状态。  
> 该状态下可以接受远程的Open 消息，如果失败则进入Active 状态

5. OpenReceive（打开接收）
> 发送所完Open 消息后，同时进入OpenReceive 状态，用于接收远程Open消息  
> 等待远程Peer 的初始keepalive消息。如果接收到keepalive 消息则进入  
> established 状态。否则进入Idle 状态

6. Established（建立）
> 从远程Peer 接收到初始的keepalive 消息后，BGP 路由进程开始与远程Peer  
> 交换消息。如果收到更新或者keepalive 消息时，连接保持计时器（hold timer）  
> 开始计时；如果收到错误消息则进入Idle 状态。


### BGP 的路由属性

> BGP 的路由属性是一组参数，对特定的路由进行了进一步的描述。  
> BGP 的路由有很多属性，可以分为四类：

- Well-Known mandatory（公认必须遵循）

> 所有的路由器都能够识别该属性，并且必须存在与Update 消息中

- Well-Known discretionary（公认可选）

> 所有BGP 路由器都可以识别出来，但不是必须要求在Update 消息中携带

- Optional transitive（可选传递）

> 在AS 之间具有可传递行的属性，BGP 路由器可以不支持该属性，但仍然会接收  
> 并传递给下一个路由器。

- Optional non-transitive（可选非传递）

> 如果BGP 路由器不支持该属性，则该属性被忽略，且不会通告给其它Peer。


