```
root@test:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:c7:79:75 brd ff:ff:ff:ff:ff:ff
    inet 10.100.122.2/24 brd 10.100.122.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::f816:3eff:fec7:7975/64 scope link 
       valid_lft forever preferred_lft forever
```
## IP地址的种类
![IP地址种类](../Image/IP地址的种类.png)
### A、B、C三类地址所能包含的主机数量
|  类别  |  IP地址范围  |  最大主机数  |  私有IP地址范围  |
|  :----:  |  :----:  |  :----:  |  :----:  |
| A | 0.0.0.0-127.255.255.255 | 16777214 | 10.0.0.0-10.255.255.255 |
| B | 128.0.0.0-191.255.255.255 | 65534 | 172.16.0.0-172.31.255.255 |
| C | 192.0.0.0-223.255.255.255 | 254 | 192.168.0.0-192.168.255.255 |
## 无类型域间选路（CIDR）
将32位的IP地址一分为二，前面是**网络号**，后面是**主机号**  
例：10.100.122.2/**24**  
24的意思是：32位中，前24位是网络号，后8位是主机号  

伴随着CIDR诞生的：
- **广播地址**，10.100.122.255，如果发送这个地址，所有10.100.122网络里面的机器都可以收到
- **子网掩码**，255.255.255.0  
**将子网掩码和IP地址按位计算AND，就可得到网络号。**  


- D类：**组播地址**。使用这一类地址，属于某个组的机器都能收到。
    - 例：公司里面大家都加入了一个邮件组，发送邮件，加入这个组的都能收到。
- IP地址后的scope
  - 对于eth0这张网卡来讲，是global，说明网卡是可以对外的，可以接受来自各个地方的包
  - 对于lo来讲，是host，说明这张网卡仅仅可以供本机相互通信
    - lo全称是**loopback**，又称环回接口，往往会被分配到127.0.0.1这个地址。这个地址用于本机通信，经过内核处理后直接返回，不会在任何网络中出现。
