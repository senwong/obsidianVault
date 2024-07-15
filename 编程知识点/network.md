介绍网络相关的知识点


# virtual machine network
常见的虚拟机的网络模式，virtualBox，
参考： https://www.makeuseof.com/whats-the-difference-nat-bridge-host-only-network-modes/
## NAT

![[network-NAT.png|800]]

虚拟机可以访问外网，访问外网时，虚拟机的IP经过宿主机的NAT mask之后，这种模式下虚拟机之间不能互相访问。

## Bridged
桥接模式，这种模式相当于虚拟机和宿主机处于同一个网络中，虚拟机和物理机之间可以互相访问。宿主机有一个虚拟的DHCP server给虚拟机提供ip。
![[network-bridged.png|800]]

## Host Only
Host-Only模式下，宿主机提供DHCP server给每个虚拟机分配一个IP地址，但是没有NAT去translate虚拟机的地址，所以不能访问外网，虚拟机之间可以互相访问。
![[network-host-only.png|800]]
# docker network
docker network是基于Linux的Network NameSpace 隔离实现的，区别于vm中虚拟出网卡的方式。

## Bridged
docker 默认的模式，类似于vm中的NAT模式，NAT模式中宿主机会有一个NAT服务，docker中会有一个`172.17.0.1`的虚拟网桥承担NAT的功能，每个container有单独的ip `172.17.0.2`, `172.17.0.3`等，



## Host
host模式下，docker容器不会隔离network namespace，容器使用的是宿主机的network是用一个。如果宿主机4000端口被占用，容器里也会被占用。




# IPtables

linux系统的iptables的作用：packet  filterring， NAT， port forwarding.
iptable使用规则实现网络的过滤和导向，这些规则存储在专门的表中，表存储在linux内核中。
netfilter是在内核空间，iptable是在用户空间

## 规则
定义如何处理某些满足条件的数据包，定义中包含两个部分【数据包过滤条件】+【对数据包的操作】，如果数据包满足某个条件，就这样处理处理数据包。
具体的规则条件包含：源地址、目的地址、传输协议（TCP, UDP, ICMP）和服务类型（HTTP, SMTP）。
处理操作包含：放行accept，拒绝reject，丢掉drop。


## 链
每一条链包含多条数据规则，当数据包到大链时，会一次检查每个规则是否符合，如果符合则按次规则定义的操作处理数据包，否则处理下一条规则。



iptables的处理流程：
![a](https://img-blog.csdn.net/20131023184402031)
进入的数据包先进入PreRouting链，经过nat-mangle-raw三个表处理，如果目的地址不是本机，进入forward链，转发出去，否则进入INput链，然后本地进程接受数据包。



