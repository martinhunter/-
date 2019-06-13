OSI模型 (open systems interconnection)：还包含表示层和会话层

TCP/IP模型
1. 应用层： 是网络应用程序和它们的应用层协议存在的地方，如http，dns，ftp协议。
2. 传输层： 控制服务器和客户端的点对点传输协议。TCP，UDP协议。传输层的API使用socket函数（winsock）。segment表示传输层封包
3. 网络层： 确定数据的物理路径。将网络层封包（datagram）从一个主机移到其他主机（其间会经过多个节点）。
传输层协议将一个segment和一个目的地址传递到网络层。
网络层有2个基本组件。IP协议（定义了datagram中各域以及终端系统和路由器如何在这些域上操作。），也包含一些路由协议决定datagram走的路径
4. 链路层： 将frame封包从路径上的一个网络节点移动到下一节点。在每个节点，网络层传递datagram到下面的链路层，让它将之发送到路径的下一个节点。
在下个节点，链路层再把datagram传递给网络层。
节点间通信方式，其协议是Ethernet和Point-toPoint（PPP）
  * 将数据发送给它所有相邻节点，即LAN的广播通信
  * WAN中的点对点通信，例如2个路由器或modem和ISP路由间的通信
对于一个给定的连接来说，链路层协议主要实现在适配器中（即NIC network interface card 网卡）。
frame表示链路层封包。datagram传输到适配器，并封装成frame。frame传输到物理层通信链路
5. 物理层：将frame
