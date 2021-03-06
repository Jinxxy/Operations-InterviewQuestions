1. LVS 负载均衡有哪些策略？分别有什么特点？
LVS一共有三种工作模式： DR，Tunnel,NAT
| 类目       | NAT        | TUN           | DR                  |
| ---------- | ---------- | ------------- | ------------------- |
| 操作系统   | 任意       | 支持隧道      | 多数（支持non-arp） |
| 服务器网络 | 私有网络   | 局域网/广域网 | 局域网              |
| 服务器数目 | 10-20      | 100           | 大于100             |
| 服务器网关 | 负载均衡器 | 自己的路由    | 自己的路由          |
| 效率       | 一般       | 高            | 最高                |

2. 负载均衡的原理是什么？
当客户端发起请求时，请求直接发给Director  Server（调度器），这时会根据设定的调度算法，将请求按照算法的规定智能的分发到真正的后台服务器。以达到将压力均摊。
但是我们知道，http的连接时无状态的，假设这样一个场景，我登录某宝买东西，当我看上某款商品     时，我将它加入购物车，但是我刷新了一下页面，这时由于负载均衡的原因，调度器又选了新的一台服  务器为我提供服务，我刚才的购物车内容全都不见了，这样就会有十分差的用户体验。
所以就还需要一个存储共享，这样就保证了用户请求的数据是一样的

3. 负载均衡的作用有哪些？
- 转发功能
按照一定的算法【权重、轮询】，将客户端请求转发到不同应用服务器上，减轻单个服务器压力，提高  系统并发量。
- 故障移除
通过心跳检测的方式，判断应用服务器当前是否可以正常工作，如果服务器期宕掉，自动将请求发送到  其他应用服务器。
- 恢复添加
如检测到发生故障的应用服务器恢复工作，自动将其添加到处理用户请求队伍中。

4. 谈谈你对LVS的理解？
LVS是一个虚拟的服务器集群系统，在unix系统下实现负载均衡的功能；采用IP负载均衡技术和机遇内容请求分发技术来实现。
LVS采用三层结构，分别是：
第一层： 负载调度器
第二层： 服务池
第三层：共享存储
负载调度器（load balancer/Director），是整个集群的总代理，它有两个网卡，一个网卡面对访问网站的客户端，一个网卡面对整个集群的内部。负责将客户端的请求发送到一组服务器上执行，而客户也认为服务是来自这台主的。举个生动的例子，集群是个公司，负载调度器就是在外接揽生意，将接揽到  的生意分发给后台的真正干活的真正的主机们。当然需要将活按照一定的算法分发下去，让大家都公平  的干活。
服务器池（server pool/Realserver），是一组真正执行客户请求的服务器，可以当做WEB服务器。就是上面例子中的小员工。
共享存储（shared  storage），它为服务器池提供一个共享的存储区，这样很容易使得服务器池拥有相同的内容，提供相同的服务。一个公司得有一个后台账目吧，这才能协调。不然客户把钱付给了A，而  换B接待客户，因为没有相同的账目。B说客户没付钱，那这样就不是客户体验度的问题了。

5. LVS由哪两部分组成的？
LVS由2部分程序组成，包括ipvs和ipvsadm。
  1. ipvs(ip virtual server)：一段代码工作在内核空间，叫ipvs，是真正生效实现调度的代码。
  2. ipvsadm：另外一段是工作在用户空间，叫ipvsadm，负责为ipvs内核框架编写规则，定义谁是集 群服务，而谁是后端真实的服务器(Real Server)

6. 与lvs相关的术语有哪些？
DS：Director Server。指的是前端负载均衡器节点。
RS：Real Server。后端真实的工作服务器。
VIP：Virtual IP 向外部直接面向用户请求，作为用户请求的目标的IP地址。
DIP：Director Server IP，主要用于和内部主机通讯的IP地址。
RIP：Real Server IP，后端服务器的IP地址。
CIP：Client IP，访问客户端的IP地址。

7. LVS-NAT模型的特性
RS应该使用私有地址，RS的网关必须指向DIP
DIP和RIP必须在同一个网段内
请求和响应报文都需要经过Director Server，高负载场景中，Director Server易成为性能瓶颈支持端口映射
RS可以使用任意操作系统
缺陷：对Director Server压力会比较大，请求和响应都需经过director server

8. LVS的负载调度算法
- 轮叫调度
- 加权轮叫调度
- 最小连接调度
- 加权最小连接调度
- 基于局部性能的最少连接
- 带复制的基于局部性能最小连接
- 目标地址散列调度
- 源地址散列调度

