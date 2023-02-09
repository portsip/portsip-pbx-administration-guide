# 使用PortSIP SBC的PBX和SIP中继线
本主题描述了如何设置 PortSIP SBC 来实现通用 SIP 中继线和 PortSIP PBX 之间的互通。
## 1 互操作性环境的拓扑结构
SBC 和通用 SIP 中继线与 PortSIP PBX 之间的互操作性测试是通过以下拓扑结构设置完成的：
+ 企业在其专用网络中部署了 PortSIP PBX，以加强企业内部的通信。
+ 企业希望为其员工提供企业语音功能，并使用SIP中继服务将企业与PSTN网络连接起来。
+ 实施PortSIP SBC，以实现企业局域网和SIP中继线之间的互连。
   - 会话：使用基于IP的会话发起协议（SIP）的实时语音会话。
   - 边界：企业局域网中的PortSIP PBX网络和位于公共网络中的SIP中继线之间的IP对IP网络边界。下图说明了这种互操作性的拓扑结构。
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FaQs6Dt44kqgKqNL0LANY%2Fenterprise_pbx_sbc_trunk.png?alt=media&token=132e018a-c784-4279-87db-cc10bea11f9a)
## 2 配置 PortSIP PBX
请按照以下步骤来安装和配置 PBX：
+ [1 安装PortSIP PBX](https://support.portsip.com/portsip-pbx-administration-guide/1-installation-of-the-portsip-pbx)
+ [2 配置 PortSIP PBX](https://support.portsip.com/portsip-pbx-administration-guide/2-configuring-the-portsip-pbx)
## 3 配置PortSIP SBC
请按照以下步骤来安装和配置SBC：
+ [为WebRTC配置SBC](https://support.portsip.com/portsip-pbx-administration-guide/9-configuring-sbc-for-webrtc)
## 4 添加SIP中继线到PortSIP PB1 安装PortSIP PBX
假设中继线的信息如下：
+ 互联网上的SIP中继线
+ SIP中继线的IP是52.214.181.141，端口是5060。为WebRTC配置SBC]()
+ SIP中继线的传输方式是UDP
+ SIP中继线是**IP授权模式**或**注册授权模式**  
  
将中继线添加到PBX中:   
1. **使用“ System Admin ”凭据**登录 PortSIP PBX Web Portal，然后单击菜单"**Call Manager > Trunks**"。
2. 点击箭头按钮，选择要添加的基于**IP**的或基于**注册的**中继线类型。
3. 为该中继线输入一个友好的名称，并在 "**Host Domain or IP**"中填写SIP中继线IP `52.214.181.14`，"**port**"中填写5060。如果中继端口不是5060，请输入实际端口。
4. 在 "**出站代理服务器**"中填写SBC的私有IP，如果是`192.168.1.73`，在 "**出站代理服务器端口**"中填写SBC的端口`5069`，默认情况下，SBC使用TCP的端口`5069`来接收来自PBX的SIP消息。
5. 选择TCP传输方式，点击 "**下一步**"按钮。
6. 如果中继线是 "**基于注册的**"类型，在这里输入 "**Authorization Name**"和 "**Password**"，这是中继线服务提供商提供的。
7. 打开 "**中继线与PBX位于同一局域网** "选项。
8. 关闭 "**向中继线发送请求时，用公共IP重写Via头的主机IP**"选项，点击 "**下一步** "按钮。
9. 由于该中继线是由 "**system amdin**"添加的，"**system admin** "需要选择一个或多个租户，允许他们访问该中继线。请参考[中继线管理](https://support.portsip.com/portsip-pbx-administration-guide/7-trunk-management)的 "**system admin添加中继线** "部分。  
   
综上所述，只需像添加普通中继一样添加SBC，把中继的IP或域填到 "**主机域或IP**"，把SBC的IP和端口填到 "**出站代理服务器**"，把传输设置为TCP，因为PBX使用TCP与SBC通信，呼叫流程为:
**分机<-->PBX<-->SBC<-->SIP中继**  
  
现在你已经完成了SBC中继设置，您可以按照本指南创建[入站和出站规则](https://support.portsip.com/portsip-pbx-administration-guide/8-call-route-management)，像普通中继线一样。  

