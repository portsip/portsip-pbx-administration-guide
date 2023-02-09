PortSIP PBX推出了PortSIP SBC，它作为一个组件与提供WebRTC服务的PortSIP PBX一起工作；支持Microsoft Teams Direct Routing。
PortSIP SBC为PortSIP PBX提供灵活性和可扩展性，使我们能够支持大量的WebRTC客户。
在我们开始之前，请阅读"[什么是SBC?](https://support.portsip.com/tutorials/what-is-an-sbc)"。通常情况下，SBC的概念如下所示。
![image](https://user-images.githubusercontent.com/112454775/217741986-8f856139-71fa-4ad8-ad4d-437cdd3daf31.png)
#9.1 部署方案
**带有SIP中继线和互联网用户的企业IP PBX**
本例介绍了在IP PBX、SIP中继和游牧互联网用户（应用程序、WebRTC、IP电话）之间互通时，如何配置SBC。该示例场景包括以下拓扑结构。
企业局域网IP PBX的IP地址，192.168.1.72
一个互联网SIP中继线
游牧的互联网用户
微软团队
