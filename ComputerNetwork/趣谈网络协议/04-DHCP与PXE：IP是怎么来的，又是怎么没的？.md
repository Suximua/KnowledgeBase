## 如何配置IP地址？
1. 使用net-tools：
   ```
   $ sudo ifconfig eth1 10.0.0.1/24
   $ sudo ifconfig eth1 up
   ```
2. 使用iproute2：
   ```
   $ sudo ip addr add 10.0.0.1/24 dev eth1
   $ sudo ip link set up eth1

  <mark>:exclamation:**只要是在网络上跑的包，都是完整的，可以有下层没上层，绝对不可能有上层没下层**<mark>  

  **Linux默认的逻辑是，如果这是一个跨网段的调用，它便不会直接将包发送到网络上，而是企图将包发送到网关。**  

  **不同系统的配置文件格式不同，但是无非就是CIDR、子网掩码、广播地址和网关地址。**  

## 动态主机配置协议（DHCP）
> 每一台新接入的机器都通过DHCP协议，来这个共享的IP地址里申请，然后自动配置好就可以了。等人走了，或者用完了，还回去，这样其他的机器也能用。  

**如果是数据中心里面的服务器，IP一旦配置好，基本不会变，这就相当于买房自己装修。**  
**DHCP的方式就相当于租房。你不用装修，都是帮你配置好的。你暂时用一下，用完退租就可以了。**  

## 解析DHCP的工作方式
> 当一台机器新加入一个网络的时候，肯定一脸懵，啥情况都不知道，只知道自己的MAC地址。怎么办？先吼一句，我来啦，有人吗？这时候的沟通基本靠“吼”。这一步，我们称为**DHCP Discover**
```mermaid
flowchart LR
    A[我的MAC是这个，
我还没有IP] -->|BOOTP头| B[Boot request]
    B -->|UDP头| C[源端口：68
目标端口：67]
     C -->|IP头| D[新人的IP：0.0.0.0
广播IP：255.255.255.255]
     D -->|MAC头| E[新人的MAC
广播MAC：（ff:ff:ff:ff:ff:ff）]
```

> 一个网络管理员在网络里面配置了DHCP Server，他就相当于IP的管理员。  
> 因为MAC唯一，IP管理员知道他是一个新人，需要租给他一个IP地址 ===> **DHCP Offer**的过程  
> DHCP Server为此客户保留为它提供的IP地址，从而不会为其他DHCP客户分配此IP地址  
- :arrow_down:DHCP Offer的格式
  ```mermaid
  flowchart LR
    A[这是你的MAC，
   我分配了这个IP，租给你，你看如何？] -->|BOOTP头| B[Boot reply]
    B -->|UDP头| C[源端口：67
   目标端口：68]
     C -->|IP头| D[DHCP Server IP：192.168.1.2
   广播IP：255.255.255.255]
     D -->|MAC头| E[DHCP Server的MAC
   广播MAC：（ff:ff:ff:ff:ff:ff）]
   ```
