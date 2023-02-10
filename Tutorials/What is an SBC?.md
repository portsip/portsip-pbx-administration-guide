# 什么是SBC？
"SBC "是Session Border Controller的缩写。SBC是一种网络设备/软件，通常部署在网络管理域的边界，控制某些类型的网络流量。在实施面向会话的多媒体通信（如IP语音（VoIP））时，SBC特别关注网络流量。简而言之，会话边界控制器位于网络域的边缘，控制面向会话的流量。

SBC执行许多基本功能，旨在实现和加强多媒体通信。这些功能包括安全、信令协议修复和互通（如IPv4到IPv6互通）、媒体转码、合法拦截等等。

SBC关于VoIP通信的安全功能在概念上与IP通信的防火墙相似。更具体地说，SBC的行为类似于代理防火墙的行为--它终止某些网络协议会话。然后，它将其中的信息传播到另一方的相应会话上。在SBC中，术语Back to Back User Agent（B2BUA）被用来描述这种结构，如下图所示。这种基本方法是SBC的许多安全功能的基础，它在设备上强加了一个 "允许理解的东西，阻止其余的东西 "的结构。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FXL3AZTHvyVwFKw9DNQ1J%2Fsbc_arch.png?alt=media&token=ccaed717-5639-4026-afef-b84df4fe8a37)

除了控制转发（或阻断）信令和媒体信息外，SBC还可以调整内容，如过滤SIP头或对音频进行转码。一些SBC还可以提供更复杂的服务，如呼叫路由。

SBC的B2BUA行为发生在协议栈的第5层。从IP角度看，SBC是一个网络主机；SBC不转发IP数据报，只是具体的内容。
