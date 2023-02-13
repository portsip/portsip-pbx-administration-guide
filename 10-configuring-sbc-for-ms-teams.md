希望优化对Office 365和Microsoft Teams投资的企业可以利用PortSIP的强大通信解决方案。PortSIP PBX for Microsoft Teams是一个原生的直接路由集成，提供呼叫功能，使Teams用户能够通过业界领先的云或内部部署的PBX完成更多工作，同时使Teams处于协作体验的中心。
  
# 特点和优点 
为您的Microsoft Teams用户提供企业级的电话服务，具有本地用户体验。丰富的PBX功能:  
+ 基本的呼叫控制
+ IVR和呼叫处理
+ 自动呼叫记录
+ 呼叫报告和分析
+ 呼叫队列
+ 完整的PSTN呼叫
+ 开箱即用的本地集成，提高用户的生产力。
+ 为您的团队提供增强的PBX和电话工具，为您的项目提供真正的生产力提升。
+ 简化您的用户需要使用的工具数量，减少成本、管理和培训。
+ 利用最新的增强型通信，节约成本，提高你的用户享受的服务水平。
  
# 前提条件  
+ 需要微软TEAMS（Office 365）账户，并在Office 365中拥有FQDN（完全合格域名）的域名。
+ 希望打电话或被打电话的Teams用户的Microsoft 365 E5订阅计划或Microsoft Teams + Microsoft Phone System（附加）。
+ 确保防止收费旁路和通过PSTN发送呼叫的设置在用户的呼叫策略中被设置为关闭。这将允许直接路由，特别是如果你也在使用MS电话系统。在微软团队管理中心的左侧菜单项目语音>呼叫策略下可以找到。然后选择用户正在使用的呼叫策略，并确保该设置被关闭。
+ 为你的域名提供一个可信的TLS/SSL证书，例如DigiCert、Thawte、Geotrust、Versign，建议使用通配符证书。
+ 如果 PBX 运行在一个公共 IP 地址上，设置就会容易很多。也可以在局域网内使用私有 IP 进行设置，但这需要对防火墙进行适当的设置，并在 PBX 上进行额外的步骤，以便能够从微软的服务器访问 PBX。
  
# 操作性环境的拓扑结构  
在本指南中，SBC和Microsoft Teams与PortSIP PBX之间的互通是通过以下拓扑结构设置完成的。
![image](https://user-images.githubusercontent.com/112454775/218037911-ad9e312b-8ef3-4caa-9e94-6683bf8b10e3.png)
+ 企业在其专用网络中部署了PortSIP PBX，以加强企业内部的通信。该PBX有一个静态的私有IP地址192.168.1.72，没有DMZ，也没有分配公共IP地址。
+ 企业希望为其员工提供企业语音功能并连接微软团队。
+ SBC有一个静态私有IP地址192.168.1.73，部署在企业网络的边缘，DMZ是静态公共IP地址66.175.221.120。
+ 域名 sbc.portsip.cc 已经解析到 SBC 的静态公共 IP 66.175.221.120。
+ 域名 sbc.portsip.cc 的可信 TLS 证书，或者域名 portsip.cc 的通配符 TLS 证书。
+ 对于 PortSIP PBX 的租户，SIP 域名是 sip.portsip.cc。
+ PBX 用户拨打 Teams 用户的电话号码，PBX 路由呼叫到 SBC，SBC 路由呼叫到 Microsoft Teams。
+ 当Teams用户拨打该号码时，呼叫被路由到SBC，然后到PBX，最后根据PBX的呼入规则到分机。
