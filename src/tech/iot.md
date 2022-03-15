# 物联网常见协议
物联网是人工智能落地的一个非常好的应用场景。随着人工智能的迅速发展，物联网这个同样在很多年前就提出的理论和技术，也会迎来新的春天。

端到端的沟通，一直是物联网业务的难点。使用的物联网通讯协议不同，使得这些设备之间的沟通存在巨大的鸿沟。

通讯协议又称通讯规程，是通信双方对数据传送控制的一种约定，双方必须共同准守。约定的内容包括：数据格式、同步方式、传送速度、传送速度、传送步骤、检纠错方式、控制字符定义等。

双方实体需要准守通信协议中既定的规则，才能将有意义的信息传递给对方。

市面上有很多物联网应用层协议，COAP、HTTP、MQTT 是最常见的三种。HTTP 和 MQTT 均使用 TCP 作为传输层协议，而 COAP 则是基于 UDP 传输协议。同时，传输层的 UDP 和 TCP 协议又都是依赖网络层的 IP 技术。

## TCP/IP 网络模型
TCP/IP 模型是互联网的基础，它是一系列网络协议的总称。这些协议可以划分为四层：
- 链路层：负责封装和解封装 IP 报文，发送和接收 ARP 和 RARP 报文等。
- 网络层：负责路由以及把分组报文发送给目标网络或主机。
- 传输层：负责对报文进行分组和重组，并以 TCP 或 UDP 协议格式封装报文。
- 应用层：负责向用户提供应用程序，比如 HTTP、COAP、Telnet、MQTT、DNS 等。

在网络体系结构中，网络通讯的建立必须是在通讯双方在对等层进行，不能交错。

在整个数据传输过程中，数据在发送端时经过各层都要附加上相应层的协议头和协议尾（仅数据链路层需要封装协议尾）部分，也就是要对数据进行协议封装，以标识对应层所用的通讯协议。

TCP（Transmission Control Protocol，传输控制协议），是是一种面向连接的、可靠的、基于字节流得到传输层通信协议，由 IETF 的 RFC 793 定义。

为了防止出现失效的连接请求报文被服务端接收的情况，从而产生错误，建立一个 TCP 连接的需要三次握手的过程：
- 第一次握手：客户端发送 SYN（SEQ=x）报文给服务器，进入 SYN_SEND 状态
- 第二次握手：服务端接收到 SYN 报文，回应一个 SYN（SEQ=y）+ ACK（ACK=x+1）报文，并进入 SYN_RECV 状态。
- 第三次握手：客户端收到服务端的 SYN 报文，回应一个 ACK（ACK=y+1）报文，进入连接（Established）状态。

建立一个连接需要三次握手，而终止一个连接需要进经过四次挥手，这是由于 TCP 的半关闭造成的。

## HTTP 协议
HTTP 协议，即超文本传输协议（Hypertext Transfer Protocol），是一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档得到数据传送协议。

目前 HTTP 协议作为 WEB 的标准协议已被广泛使用，它在一些物联网场景中同样可以使用，例如手机、PC等终端设备。但是作为适应浏览器场景的 HTTP 协议，并不适用于物联网的其他设备。

适用范围：开放物联网中的资源，实现服务被其他应用所调用。

优势：
- 简单的工作模式，请求 / 响应。
- 完整的方法定义。
- 合理的状态码设计。
- 友好的媒体类型支持，如文本、图片、视频等。

缺点：
- 单向传输，可以通过客户端轮询实现类似推送效果或者HTTP2.0
- 安全性不高，HTTP是明文协议，可以使用 HTTPS 传输。
- HTTP 是文本协议，冗长的协议头部，对于运算、存储、带宽资源受限的设备来说开销大。

## CoAP 协议
CoAP（Constrained Application Protocol）即受限的应用协议。CoAP 是为了让低功耗受限设备可以接入互联网，由 IETF 组织制定的，它借鉴了 HTTP 大量成功经验，同样使用请求 / 响应工作模式。

适用范围：适用于互联网环境下一对一 M2M 通讯。

优势：
- 采用和 HTTP 相似语义的请求和响应码，使用二进制报文，报文大小较小。
- 传输层基于 UDP 协议，比 TCP 数据包小，并不需要建立连接带来握手的开销。
- 资源发送支持，通过观察者模式实现类似发布 / 订阅效果。

缺点：
- 基于 UDP 的不可靠传输，但通过四种报文类型的组合及重传机制提高了传输的可靠性。
- 基于 UDP 的无连接传输，不利于不同网络间消息的回传。

## MQTT 协议
MQTT（Message Queuing Telemetry Transport）即消息队列遥测传输。

MQTT 协议最初是在 1999 年由 IBM 公司开发的，用于将石油管道上的传感器与卫星相连接。2014 年正式成为 OASIS 开放标准。

MQTT 使用类似 MQ 常用的发布 / 订阅模式，起到应用程序解耦、异步消息、削峰填谷的作用。很多 MQ 中间件也支持 MQTT 协议，比如 ActiveMQ、RabbitMQ、Kafka 等。

适用范围：在低带宽、不可靠的网络下提供基于云平台的远程设备的数据传输和监控。

优势：
- 使用发布 / 订阅模式，提供一对多的消息发布，使消息发送者和接受者在时间和时空上解耦。
- 二进制协议，网络传输开销非常小（固定头部是2字节）。
- 灵活的 Topic 订阅、QoS 等特性。

缺点：
- 集中化部署，服务端压力大，需要考虑流程控制及高可用。
- 对于请求 / 响应模式的支持需要在应用层根据消息 ID 做发布主题和订阅主题之间的关联。


## MQTT-SN协议
MQTT-SN（MQTT for Sensor Network）协议是MQTT协议的传感器版本。MQTT协议虽然是轻量的应用层协议，但是MQTT协议是运行于TCP协议栈之上的，TCP协议对于某些计算能力和电量非常有限的设备来说，比如传感器，就不太适用了。

MQTT-SN运行在UDP协议上，同时保留了MQTT协议的大部分信令和特性，如订阅和发布等。MQTT-SN协议引入了MQTT-SN网关这一角色，网关负责把MQTT-SN协议转换为MQTT协议，并和远端的MQTT Broker进行通信。MQTT-SN协议支持网关的自动发现。

## LoRaWAN协议
LoRaWAN协议是由LoRa联盟提出并推动的一种低功率广域网协议，它和我们之前介绍的几种协议有所不同。

MQTT协议、CoAP协议都是运行在应用层，底层使用TCP协议或者UDP协议进行数据传输，整个协议栈运行在IP网络上。而LoRaWAN协议则是物理层/数据链路层协议，它解决的是设备如何接入互联网的问题，并不运行在IP网络上。

LoRa（Long Range）是一种无线通信技术，它具有使用距离远、功耗低的特点。 在上面的场景下，用户就可以使用LoRaWAN技术进行组网，在工程设备上安装支持LoRa的模块。

通过LoRa的中继设备将数据发往位于隧道外部的、有互联网接入的LoRa网关，LoRa网关再将数据封装成可以在IP网络中通过TCP协议或者UDP协议传输的数据协议包（比如MQTT协议），然后发往云端的数据中心。

## NB-IoT协议
NB-IoT（Narrow Band Internet of Things）协议和LoRaWAN协议一样，是将设备接入互联网的物理层/数据链路层的协议。

与LoRA不同的是，NB-IoT协议构建和运行在蜂窝网络上，消耗的带宽较低，可以直接部署到现有的GSM网络或者LTE网络。

设备安装支持NB-IoT的芯片和相应的物联网卡，然后连接到NB-IoT基站就可以接入互联网。而且NB-IoT协议不像LoRaWAN协议那样需要网关进行协议转换，接入的设备可以直接使用IP网络进行数据传输。

NB-IoT协议相比传统的基站，增益提高了约20dB，可以覆盖到地下车库、管道、地下室等之前信号难以覆盖的地方。

