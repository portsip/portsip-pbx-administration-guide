# 11.2 SBC和PBX设置
在配置SBC和PBX的Microsoft Teams Direct Routing之前，请确认以下项目已经完成。
+ PBX已经安装并配置好了。
+ SBC已经安装并配置好。
+ Microsoft Teams Direct Routing已经配置好了。
## 1. 添加Team基础域   
1. 登录到SBC的Web Portal https://sbc.portsip.cc:8883/  
2. 选择菜单 "**设置 > Teams Base Domain**"，点击 "**添加** "按钮，添加SBC域名，SBC将发送OPTIONS消息给MS团队以保持活力，如果它是 "**sbc.portsip.cc**"。

![image](https://user-images.githubusercontent.com/112454775/218358426-a46dec26-fee4-4607-80a0-b3e8de5c2b5b.png)  
  
在添加团队基础域名后，SBC将定期向团队发送OPTIONS；如果它收到200 OK的OPTIONS消息，这个域的 "状态 "将显示为 "活跃"。

如果它显示为 "非活动"，那就是出了问题，请仔细检查每一步。

## 2 在 PBX 中添加 SBC 作为团队中继线   
1. 确保5063端口的TCP传输已经被添加到PBX中。
2. 以 "**System Administrator**"身份登录 PBX web portal，进入 "**租户** "菜单，选择需要管理的租户，点击 "**管理** "按钮；或者以该租户的 "**admin** "角色登录 PBX web portal。
3. 选择 "**通话管理 > SIP中继**"菜单，并点击箭头按钮，选择中继线类型 "**Microsoft Teams**"。
+ DID/DDI Pool : 从Team拨到PBX的电话号码，可以是一个单独的号码，也可以是一个号码范围，例如：331001-331100；33是你在创建租户用户时指定的Team用户的国家代码（33代表法国）；1001-1100是Team用户的电话号码范围。
+ 主机域名或IP不能修改，端口是SBC与PBX通信的传输端口，默认为5069。
+ 在 "**Outbound 服务器**"栏中输入SBC服务器的私有IP 192.168.1.73。如果SBC与PBX安装在同一台服务器上，那么这就是PBX服务器的私有IP。
+ "**Outbound 服务器端口**" "默认为5069，该端口是SBC与PBX通信的传输端口。
+ **传输协议** "为TCP。  
  
![image](https://user-images.githubusercontent.com/112454775/218360129-ea05ebf9-0a79-4b8a-875f-bf5dc8ea0734.png)
    
4. 点击 "**下一步** "按钮
5. 关闭 "当向中继线发送请求时，用公共IP重写Via头的主机IP"，并点击 "**确定**"，完成Teams Trunk的配置。
  
## 3. 创建入站规则   
必须创建入站规则，以便将Microsoft Teams呼叫路由到分机或系统服务，如IVR、呼叫队列和铃声组。
1. 通过 "**System Admin** "身份登录 PortSIP PBX Web Portal，点击菜单 "**租户**"，选择一个租户，然后点击 "**管理** "按钮来管理这个租户，配置呼入规则。 或者将一个拥有租户管理权限的用户登录到Web Portal来管理这个租户。
2. 从Web Portal，选择 "**通话管理 > 接入规则**"，并点击 "**添加** "按钮。
3. 为规则输入一个友好的名称，例如，"**From Teams**"，如下图所示，当从团队拨打号码331001时，呼叫将被转接到PBX分机101。当团队用户拨打1001，一旦呼叫到达PBX，拨出的号码将是+331001，我们应该删除+，只输入数字。
  
![image](https://user-images.githubusercontent.com/112454775/218361700-53de0089-eb11-4fb3-be5f-139dbf594515.png)
  

## 4. 创建呼出规则
我们可以创建一个呼出规则，将PBX的呼叫路由到Microsoft Teams。
   
1. 通过 "**System Admin** "登录 PortSIP PBX Web Portal，点击菜单 "**租户**"，选择一个租户，然后点击 "**管理** "按钮来管理这个租户，配置外拨规则。 或者让一个有租户管理员权限的用户进入Web Portal来管理该租户。
2. 在Web Portal上，选择 "**通话管理> 外拨规则**"，然后点击 "**添加** "按钮。
3. 我们想把分机102打出的所有电话转接到Microsoft Teams，然后配置外拨规则，如下图所示。
![image](https://user-images.githubusercontent.com/112454775/218363599-aa5943b4-f1f9-4335-b554-1a17f4e1a545.png)
  
当分机102拨打一个不存在于PBX内部目录中的号码时，呼叫会被路由到Microsoft Teams。
  
例如，当分机102拨打1001时，PBX将呼叫路由到Microsoft Teams，由于被呼叫者的号码是1001，所以拥有1001电话号码的Teams用户将被呼叫。
