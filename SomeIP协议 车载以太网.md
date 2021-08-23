# Some/IP协议 车载以太网

**全称：**Scalableservice-Oriented Middleware over IP，基于IP的可扩展面向服务的中间件

个人理解就是中间商。



是车载以太网通信引入的一个概念，位于OSI 7层模型的层4之上。在以CAN总线为主的车载网络中，通信过程是面向信号的（除了诊断通信之外），这是一种根据发送者需求实现的通信过程，当发送者发现信号的值变化了，或者发送周期到了，就会发送信息，而不考虑接收者是否有需求。而SOME/IP则不同，它是在接收方有需求的时候才发送，这种方法的优点在于总线上不会出现过多不必要的数据，从而降低负载。

## 作用

为应用层提供API接口，创建CS接口，通过TCP/IP协议通信。

**优点:**总线上不会出现过多不必要数据，从而降低负载。

## 访问方式

### 事件通知

与传统的CAN通信相似，服务的**周期性或者事件变化时**向客户端发送特定的数据。

<img src="https://img-blog.csdnimg.cn/img_convert/1e4653708df5ef627553e4a46647633d.png" alt="img" style="zoom: 50%;" />

### 远程过程调用(RPC)

当**客户端有请求**的时候，向服务端发送请求命令，服务端解析命令，并作出相应的响应。

![img](https://img-blog.csdnimg.cn/img_convert/26be63364dc7e010d7196f2d7467e4b7.png)

### 访问进程数据

使客户端面向服务器写入(Setter)或者读取(Getter)数据。

![img](https://img-blog.csdnimg.cn/img_convert/64c4178804a403172c16e6f8457f51a9.png)

## Some/IP协议数据格式

<img src="https://pic1.zhimg.com/80/v2-6909da98c767908bd1386a78789c4328_1440w.jpg" alt="img" style="zoom: 80%;" />

- **Message ID(Server ID)** ：16bit，服务的ID，标识出一个服务
- **Message ID(Method ID)** ：16bit，方法的ID，表示出一个方法；
- Length：报文长度，32bit，标识从request ID到报文结束的总长度；
- Request ID(Client ID) ：客户端ID，16bit。区分不同的客户端；
- Request ID(Session ID) ：会话ID，区分同一个客户端的多次调用；
- Protocol Version ：协议的版本号，固定值为x01;
- Interface Version：服务接口版本；
- **Message Type** ：报文类型，在AUTOSAR(汽车开放系统架构)中，总共包含五种，包括REQUEST，REQUEST_NO_RETURN，NOTIFICATION，RESPONSE，ERROR；
- Return Code ：返回码，包括四种，REQUEST，
- REQUEST_NO_RETURN，NOTIFICATION，RESPONSE；
- Payload ：数据段，用于放置需要传输的数据。

除了Playload之外的都属于Header

**MessageType**[8 bit]

- REQUEST 期待响应的请求
- REQUEST_NO_RETURN 不期待响应的请求
- NOTIFICATION 事件通知
- RESPONSE 响应消息
- ERROR 报错消息

REQUEST  REQUEST_NO_RETURN  RESPONSE 都是属于**远程过程调用的方式**，当客户端发送REQUEST(请求)的时候，服务端根据这个Message Type来决定是否发送Response消息

NOTIFICATION属于**事件通知类**的服务，首先由client向server订阅内容，然后server向client自动发布服务内容。

NOTIFICATION分为**Event和Field两类**，这两类通知都需要Some/IP-SD(serverice discovery)来进行订阅,然后才能发布通知。

**区别:**Event是某一时刻的快照，只是事件通知，而Field除了事件通知外，还具有Getter和Setter的功能。

**Some/IP-SD**是一种特殊的some/IP格式，他是对Playload的一种定义和实现。如图：

![img](https://pic2.zhimg.com/80/v2-d02a182b9f0b5a0cb1b3556d6df34b19_1440w.jpg)