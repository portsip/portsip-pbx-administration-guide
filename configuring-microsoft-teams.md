# 10.1 TEAMS Office 365设置
## 1. 安装团队模块
+ 运行以下命令以确认你是否已经成功安装了Teams PowerShell模块。`Get-Module -ListAvailable -Name MicrosoftTeams`. 如果安装成功，你会看到下面的结果:
![image](https://user-images.githubusercontent.com/112454775/218044094-bff5f34f-14e4-4fc1-91d4-edc44765d81c.png)
+ 如果你看不到结果，这意味着PowerShell模块没有正确安装，请访问 https://www.powershellgallery.com/packages/MicrosoftTeams/ ，手动安装它。
+ 如果有新的Teams模块，你可以使用下面的命令来升级它。
```
 Install-Module -Name MicrosoftTeams -Force
 ```
![image](https://user-images.githubusercontent.com/112454775/218044389-52ea5b02-4a77-4423-b737-a6760f2b3a7f.png)
  
## 2. 在你的Office 365租户中创建你的域名
+ 使用你的浏览器登录到你的Office 365管理账户，并创建你的域名。在Microsoft 365管理中心，进入设置，然后按查看按钮，获取你的自定义域名设置，然后按管理按钮。在那里添加一个域名。
+ 选择一个你拥有的域名（如portsip.cc）。因为它将被微软验证，例如，让你在你的DNS的TXT值字段中粘贴一个值，或者登录到你获得该域名的账户等。重要的是，你拥有该域名并能执行验证过程。一旦完成，该域名将被创建，你可以向其添加用户。
  
![image](https://user-images.githubusercontent.com/112454775/218044630-804c4d87-5eef-410f-a7e0-18362f14d724.png)
  
## 3. 在该域名中创建用户
现在你可以在上面第1步刚刚创建的域名下创建用户。同样，在Microsoft 365管理中心，进入用户下的活动用户，并添加一个用户。给出用户的名字和姓氏（比如 "Thomas Oliveri"），并在用户名下给出一个独特的名字，这将是他/她的电子邮件，但要确保在用户名的域名部分选择你上面创建的域名（本例中是portsip.cc），而不是任何其他*.onmicrosoft.com。这样，用户将居住在你在上面第1步中刚刚创建的域名中（在这个例子中说：thomas@portsip.cc）。
  
![image](https://user-images.githubusercontent.com/112454775/218045014-119ab33a-6e4d-4b53-8c6e-36812b6e2dbb.png)
  
## 4. 使用PowerShell连接到你的Office 365账户
+ 打开一个Windows PowerShell命令提示符窗口，运行以下命令。
```
  Import-Module MicrosoftTeams
  $userCredential = Get-Credential
  Connect-MicrosoftTeams -Credential $userCredential
```
在Windows PowerShell凭证请求对话框中，输入管理员账户名和密码，然后选择 "确定"。除非你想做得更多，否则只需`使用Skype for Business Online管理员帐户名和密码连接`的部分就足够了。
如果你得到的错误是`cannot be loaded because running scripts is disabled on this system`，那么运行以下命令来调整权限:
```
  Set-ExecutionPolicy RemoteSigned
```
![image](https://user-images.githubusercontent.com/112454775/218046221-63659116-53b9-4433-ae25-f743d703e7e7.png)
  
## 5. 创建一个将连接到SBC的PSTN网关
确保它是正确的，并且像你在上面的步骤中那样连接到正确的账户。要确认这些步骤是否正确，请使用以下命令:
```
Get-Command *onlinePSTNGateway*
```
你的命令将返回这里显示的四个功能，让你管理SBC。
![image](https://user-images.githubusercontent.com/112454775/218047205-1a16a667-e127-4eff-96ea-5c88927f2584.png)
  
使用 PowerShell，创建一个 PSTN 网关，它将使用域名的 FQDN 连接到 PortSIP SBC，例如：`sbc.ortsip.cc`。注意，你必须有域名 `sbc.portsip.cc` 的有效证书，或者 `portsip.cc` 的通配符证书。
  
这个 FQDN 的域名部分必须与在你的租户中注册的域名相匹配，就像上面的`在域名中创建用户`的步骤一样（例如，它可以是：sbc.portsip.cc，因为你创建的域名是 portsip.cc）。同样重要的是，在该域名中有一个Office 365用户（正如你在上面的步骤中所做的，在portsip.cc中的一个用户）和一个分配的E3或E5许可证。如果没有，你会收到一个错误。不能使用 `sbc.portsip.cc` 域名，因为它不是为这个租户配置的。
  
这个 FQDN（例如 sbc.portsip.cc）必须解析到 PortSIP SBC 的一个可达 IP。另外，这个 FQDN（在这个例子中是 sbc.portsip.cc）必须是 PortSIP PBX 中的一个 SIP 域名。你必须确保为这个域名建立一个DNS A记录（例如：sbc.portsip.cc），以便于到达PortSIP 域名 SBC添加一个有效的证书。
创建PSTN网关的PowerShell命令示例:
```
New-CsOnlinePSTNGateway -Identity sbc.portsip.cc -Enabled $true -SipSignalingPort 5067 -MaxConcurrentSessions 1000
```
>That port 5067 in the above commands refers to the PortSIP PBX(SBC) TLS port; by default, the PortSIP PBX(SBC) creates TLS transport on port 5067 for Teams; if you changed the TLS default port for Teams in the PortSIP PBX(SBC), please replace the 5067 by the new TLS port in the above commands.
  
你可以在任何时候用检查网关参数:
```
Get-CsOnlinePSTNGateway -Identity sbc.portsip.cc
```
![image](https://user-images.githubusercontent.com/112454775/218050497-90861ad8-b827-4901-afcf-2699f9ca0f7e.png)
  
## 6. 启用用户的直接路由服务
直接路由要求用户在Skype上的商业在线。可以通过查看RegistrarPool参数来检查这一点。它需要在infr.lync.com域名中有一个值。
  
PowerShell命令:
```
Get-CsOnlineUser -Identity "email" | fl RegistrarPool
```
在我们的例子中:
```
Get-CsOnlineUser -Identity "thomas@portsip.cc" | fl RegistrarPool
```
  
使用PowerShell，通过配置电话号码并为用户启用企业语音和语音邮件，使用户获得直接路由服务。

使用PowerShell，通过配置电话号码并为用户启用企业语音和语音邮件，使用户获得直接路由服务。注意，在我们的例子中，这是一个4位数的号码，但它可以是一个完整的E.164号码。在这个例子中，我们想把用户作为分机（这里1001是这个用户的分机），可以连接，可以作为TEAMS和PortSIP PBX之间的分机连接。PSTN或SIP中继线的呼入和呼出可以由PortSIP PBX中继线处理，就像任何PBX一样，TEAMS的用户可以作为PBX的分机。但你可以轻松地使用其他设置和方案，并为用户分配完整的E.164号码（含国家代码）。
```
Set-CsPhoneNumberAssignment -Identity thomas@portsip.cc -PhoneNumber +1001 -PhoneNumberType DirectRouting
Set-CsPhoneNumberAssignment -Identity thomas@portsip.cc -EnterpriseVoiceEnabled $true
```
**注意**：从在Teams中创建一个用户到能够用`Set-CsUser`或`Set-CsPhoneNumberAssignment`改变其设置可能需要一些时间，所以如果出现用户不存在等错误，你可能需要等待一两个小时，而且你已经确保用户的电子邮件（或名字）是正确的。
  
用户现在应该能够接收来自PortSIP PBX的呼叫，可以是来自PortSIP PBX本身的分机，也可以是来自PSTN和SIP中继线（在PortSIP PBX内部设置），视情况而定。
  
## 7. 配置呼出电话到PortSIP PBX的语音路由
现在你可以配置规则，以确定何时使用PSTN网关设置将呼叫路由到PortSIP PBX。
  
正如微软文件中所解释的，微软团队有一个路由机制，允许根据以下情况将呼叫发送到特定的SBC。
+ 被叫号码模式
+ 被叫号码模式 + 拨打电话的特定用户
  
呼叫路由涉及:    
+ 在线PSTN网关。它连接到SBC或PBX（在此情况下，PortSIP SBC）。它还储存了通过SBC拨打电话时应用的配置，如转发P-Asserted-Identity（PAI）或Preferred Codecs。它是由语音路由使用的。
+ 语音路由。它使用在线PSTN网关，用于呼叫号码符合模式的呼叫。
+ PSTN使用。它使用语音路由和其他PSTN使用方式。不同的语音路由策略可以使用它。
+ 语音路由策略。它使用PSTN使用量。它可以分配给一个用户或多个用户。
  
综上所述:
    
**用户 > 语音路由策略 > PSTN用量 > 语音路由 > PSTN网关**  
  
在我们的例子中，由于我们把用户作为PortSIP PBX的分机，我们希望呼叫首先进入那里，然后使用PortSIP PBX的拨号计划来路由呼叫。也可以有其他的场景，很容易实现。对于这个简单的场景，我们来创建。  

**Thomas Oliveri (用户) > MyPolicy (语音策略) > MyUsage (PSTN使用) > sbc1.portsip.com **(PSTN网关)。
  
+ 让我们先创建用法，因为它将在路由中使用:
```
  Set-CsOnlinePstnUsage -Identity Global -Usage @{Add="MyUsage"}
```
+ 让我们使用PortSIP PSTN网关和上述用法来创建路由:
```
New-CsOnlineVoiceRoute -Identity "MyRoute" -NumberPattern "^\+(\d{4})|^(\d{4})" -OnlinePstnGatewayList sbc.portsip.cc -Priority 1 -OnlinePstnUsages "MyUsage"
```
  
正如你所看到的，该命令创建了一个路由`MyRoute`，如果拨出的号码至少有4个数字（这当然是基于我们这里的例子，我们想给每个用户一个4位数的分机，但这取决于你的设置），有或没有 "+"号，TEAMS将把呼叫传到PSTN网关`sbc.portsip.cc`（PortSIP PBX的一个域名）。这样，你就可以拨打一个4位数的分机或一个带有国家代码和 "+"号的E.164号码。
  
当然，可以创建多个具有不同优先级的路由。
  
+ 要找出不同的路线，请使用:
```
  Get-CsOnlineVoiceRoute
```
  
让我们创建一个语音策略，使用上面我们的PSTN网关所链接的相同用法:
```
  New-CsOnlineVoiceRoutingPolicy "MyPolicy" -OnlinePstnUsages "MyUsage"
```
该例子的结果是:
```
  Identity         : Tag:MyPolicy
  OnlinePstnUsages : {MyUsage}
  Description      :
  RouteType        : BYOT
```
  
当然，你可以创建一个有多个PSTN使用权的策略，但我们的例子将保持简单而有针对性。
  
让我们授予我们的用户`thomas@portsip.cc` 我们的语音策略:
```
  Grant-CsOnlineVoiceRoutingPolicy -Identity "thomas@portsip.cc" -PolicyName "MyPolicy"
```
  
你可以用这个命令检查策略分配:
```
  Get-CsOnlineUser "thomas@portsip.cc" | select OnlineVoiceRoutingPolicy
```

![image](https://user-images.githubusercontent.com/112454775/218058681-ad88ca77-cfe3-4b3a-a4b7-762a6675d5c9.png)

现在呼出拨号策略和路由已经完成，`thomas@portsip.cc `可以呼出。在这个例子中，这个呼出的电话最终会打到你的PortSIP PBX SIP域名`sbc.portsip.cc`。
  
在我们的例子中，我们可以在PortSIP PBX中设置你的域名（在PortSIP PBX部分解释），这样：
  
+ 如果该分机（本例中为4位数）存在于你的PortSIP PBX域名中并被注册，它将被你的Office 365用户调用，无论它是注册在你的PortSIP SIP域名、PortSIP桌面应用、PortSIP网络应用还是PortSIP移动应用的SIP桌面电话。
+ 如果该分机不存在（比如说E.164的10位数号码等），那么PortSIP PBX将把呼叫路由到SIP中继线，以便该呼叫可以出去。


