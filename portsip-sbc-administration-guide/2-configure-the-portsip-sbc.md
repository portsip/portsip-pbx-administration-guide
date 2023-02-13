# 2 配置PortSIP SBC
在本指南中，我们假设 SBC 安装在一台服务器上，其静态内网 IP 为 `192.168.1.73`，静态公网 IP 为 `66.175.221.120`，并且域名 `sbc.portsip.cc` 已被解析为 IP `66.175.221.120`。  
  
PBX与SBC服务器位于同一个局域网内，内网IP为`192.168.1.72`。PBX需要支持Path Header [RFC3327](https://datatracker.ietf.org/doc/html/rfc3327)中定义的。  
  
成功安装[PortSIP SBC](https://www.portsip.com/portsip-sbc/)后，使用你的浏览器访问Web Portal，网址是[https://sbc.portsip.cc](https://sbc.portsip.cc)。登录页面看起来像下面的截图。在用户名和密码中都输入 "**admin**"来登录。  
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FGEDtra311rN7hIBZ2Ojb%2Fsbc_admin_portal.png?alt=media&token=ecbc10db-1f8d-4366-81ce-7c5304a7d010)
## 2.1 TLS证书
请按照此主题[准备TLS证书](https://support.portsip.com/tutorials/preparing-tls-certificates-for-tls-https-webrtc).  
  
从菜单中选择 "**Settings > TLS Certifidates**"，**点击"Add"**按钮，在 **"描述"字段中输入 "SBC Host Name "**为例；在 "**TLS 域名** "字段中填写`sbc.portsip.cc`。在Windows记事本中打开 "**portsip.pem**"文件，将其内容复制到 "**证书内容**"字段。复制并粘贴 "**portsip.key**" 文件的内容到 "**私钥内容**" 栏。单击 "**确定** "按钮以保存证书。  
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FODvS0G8jcboJCc90619z%2Fsbc_certificate.png?alt=media&token=9577a7d4-9616-484c-8cfb-64174ce9caab)
## 2.2 设置网络
从菜单中选择 "**设置 > 网络**"，在 "web 域名"栏中填写 `sbc.portsip.cc`，在 "**SBC 内网 IPv4 地址**"栏中填写 `192.168.1.73`，在 "**SBC 公网 IPv4 地址**"栏中填写 `66.175.221.120`。默认情况下，"**自动创建默认Transport**"选项被打开，SBC将在成功设置SBC IP地址后创建默认transport。
+ `TCP端口 5069`：用于与PBX通信。 
+ `TLS端口 5067`：它用于与Microsoft Teams的通信。
+ `WSS端口 5065`：用于提供WebRTC服务。
+ `UDP端口 5066`：用于提供正常的SIP服务。  -     
  
你可以关闭这个选项，防止SBC自动创建默认的 Transport，但不建议这样做。  
必须输入内网**IPv4地址**；如果你的服务器没有内网IP，请输入公网IP来代替。
默认情况下，SBC使用UDP端口`25000-35000`进行RTP Transport。你可以随意改变端口范围，但要确保其他应用程序不使用这些端口，我们建议保持默认设置。  
当你点击 "**确定** "按钮时，SBC将自动重新启动并立即签出。
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FUuPo0K5Ivvhf6FnSZ5ea%2Fsbc_network.png?alt=media&token=6b5710c3-5676-4132-bcd3-46061e14537f)  
  
>如果你改变了默认的RTP端口，请确保它们不被其他应用程序使用，并为它们添加防火墙规则。
## 2.3 配置 PBX 设置
### PortSIP PBX 配置
如果PBX是[PortSIP PBX](https://www.portsip.com/portsip-pbx/)，请按照以下步骤操作。
登录PBX Web Portal，[https://uc.portsip.cc:8887](https://uc.portsip.cc:8887)，点击菜单 "**高级>SBC**"。点击 "**生成** "按钮，生成SBC的访问令牌。点击 "**复制** "按钮，复制该令牌。  
登录PortSIP SBC Web Portal [https://sbc.portsip.cc.cc:8883](https://uc.portsip.cc:8887)。从菜单中选择**设置 > PBX**。你必须在这里设置PBX信息，然后SBC将与PortSIP PBX通信。将复制的令牌粘贴到 "**PBX Access Token** "区域，并在 "**PBX IPv4 Address** "区域输入PBX的内网IP `192.168.1.72`。由于 PortSIP PBX 的 TCP Transport是在 `5063` 端口创建的，所以请在 "**优先使用下面 Transport 与 PBX 通信** "中选择 TCP，并在 "**PBX 端口** "中输入 "`5063`"。  
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FfP1AQPvpxjpL5GKf1WRM%2Fsbc_sync_pbx.png?alt=media&token=370b3cc7-444b-4ac6-91c3-35595dadb97c)  
### 为第三方PBX进行配置
如果你的PBX**不是**PortSIP PBX，请按照以下步骤操作。   
1. 在你的PBX上创建一个端口为**5063的TCP Transport**。
2. 登录PortSIP SBC Web Portal [https://sbc.portsip.cc:8883]。在菜单中选择**设置 > PBX**. 你必须在这里设置PBX信息，然后SBC将与PortSIP PBX通信。 在 "**PBX IPv4 地址** "栏中输入 PBX 的内网IP `192.168.1.72`。如果第三方PBX的TCP transport端口为`5063`，请在 "**优先使用下面 Transport 与 PBX 通信** "中选择**TCP**，并在 "**PBX端口** "中输入 "`5063`"。如果你的PBX的TCP Transport不是`5063`，而是`5061`，那么你就需要在SBC Web Portal的 "**PBX端口** "中输入`5061`。
>如果你的 PBX TCP Transport不是 5063，而是 5061，那么你就需要在 SBC Web Portal 中的 "PBX 端口 "中输入 5061。
### 2.4 Transports
从菜单上选择 "**设置>Transports**"。SBC需要在这里添加四种类型的Transport。   
+ 一个TCP Transport，用于SBC与PBX的通信
+ TLS Transport，用于SBC与Microsoft Teams通信
+ 一个WSS Transport，用于SBC提供WebRTC服务
+ 一个UDP Transport，用于SBC提供正常的SIP服务  
 
一个用于SBC与PBX的通信，一个用于Microsoft Teams，一个用于WebRTC客户端。在上面的步骤**2.2**完成后，打开 "**自动创建默认Transport**"选项，SBC将自动创建默认Transport。  
  
>这些默认的Transport不建议改变，我们建议在上述步骤2.2中保持 "**自动创建默认Transport** "选项的开启。  
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FiWDaUKX0bTdqtfpeGtoY%2Fsbc_transports_1.png?alt=media&token=3e44e879-7beb-4412-b4e0-873ae534c841)  
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FAWXmifssGzQFmVnxps67%2Fsbc_transports_2.png?alt=media&token=e48b51a5-4366-42b0-a778-909888db1307)
### 创建Transports
**如果你继续使用默认的Transport，这一步是不必要的**。如果你想创建你自己的Transport，你可以删除现有的Transport。   
+ 为WebRTC客户端添加一个 "**WSS**"Transport，如下图所示，请在 "**网卡接口**"中选择 "**SBC公网IP**"。
+ 为SBC添加一个 "**TLS**"Transport，以便与Microsoft Teams通信，如下图所示，请在 "**网卡接口**"中选择 "**SBC公网IP**"。
+ 为SBC添加一个 "**UDP**"Transport，以提供正常的SIP服务，如下图所示，请在 "**网卡接口**"中选择 "**SBC公网IP**"。
+ 为SBC添加 "**TCP**"Transport，用于与PBX通信，请参考下面的截图，并在 "**网卡接口**"中选择 "**SBC内网IP**"。
！[](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FAWXmifssGzQFmVnxps67%2Fsbc_transports_2.png?alt=media&token=e48b51a5-4366-42b0-a778-909888db1307)  
### 添加防火墙规则
如果你创建了自己的Transport，而不是使用默认的Transport，请在防火墙上用以下命令打开你的Transport端口。例如，如果你在**5066**端口创建了**UDP** Transport，在**5071**端口创建了**TCP** Transport，在**5072**端口创建了**TLS** Transport，在**5075**端口创建了**WSS** Transport，请执行以下命令。  

#### Linux 防火墙
```
firewall-cmd --permanent --service=portsip-sbc --add-port=5066/udp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5071/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5072/tcp --set-description="PortSIP SBC"
firewall-cmd --permanent --service=portsip-sbc --add-port=5075/tcp --set-description="PortSIP SBC"
firewall-cmd --reload
```
#### Windows 防火墙
按照Windows指南创建Windows防火墙规则。  
>一旦添加了新的Transport，你必须改变防火墙规则以允许Transport端口。IP电话客户端应用程序将通过Transport和端口连接到PBX。  

>如果SBC运行在云平台上，如AWS，并且云平台本身有防火墙，你必须在云平台的防火墙上也打开端口。  
## 2.5 添加SIP域
### 将SIP域名与PortSIP PBX进行同步
如果PBX是[PortSIP PBX](https://www.portsip.com/portsip-pbx/)，请按照以下步骤操作。 

选择菜单 "**设置 > PBX**"，点击 "**立即同步**"按钮，PBX 将自动同步 PBX 的 SIP 域。你可以刷新页面来查看同步结果，"**200 OK**"表示同步成功。  
！[](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2Fx4CuRrbBYOnzIxxc6x9f%2Fsbc_sync_pbx.png?alt=media&token=e6d2e332-42bc-4562-9821-7ee06ab883bf)  
### 为第三方PBX添加SIP域
如果该PBX**不是**PortSIP PBX，请按照以下步骤操作。   
1. 选择菜单 "**设置 > 域名**"，点击**添加**按钮，在 "**描述**"栏中输入一个好的名称。
2. 复制你的PBX的SIP域并粘贴到 "**SIP域名**"字段中，然后点击 "**确定**"按钮，保存SIP域名。
3. 如果您的 PBX 是多租户 PBX，请按照上述方法将所有租户的 SIP 域添加到 SBC 中。
！[](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FYHYkgvSFJtQsynnP2kA7%2Fsbc_add_sip_domain.png?alt=media&token=a9afbcae-f9a3-4ef7-bceb-ca6edb5a92ff)
## 2.6 重塑品牌
[PortSIP SBC](https://www.portsip.com/portsip-sbc/)允许您以简单的方式重塑SBC的品牌，您只需点击几下，就可以得到自己的SBC。  
  
选择菜单 "**设置 > 品牌定制**"，你可以改变公司名称、产品名称和喜好，并上传你的标志和favicon，点击 "**确定**"按钮，保存设置。
！[](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FqwnfKWtIJItANpWNt1cZ%2Fsbc_rebranding.png?alt=media&token=e1c78f04-39fb-413d-869f-5c77225868dc)
