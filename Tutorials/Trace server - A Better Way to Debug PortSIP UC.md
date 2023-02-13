# Trace服务 - 调试PortSIP UC的更好方法
PortSIP一直在开源项目的基础上建立它的SIP Trace服务 [HOMER](https://github.com/sipcapture/homer)，它在排除SIP中继线、SIP端点和其他SIP相关问题的故障方面提供关键信息。它提供了：
- 通过Web UI访问并检索SIP捕获数据
- 集中存储许多主机上的SIP捕获数据
- 更直观地过滤SIP捕获数据，并将数据与每个请求/响应的对话/事务相关联（这非常有用）
- 优雅地保存过时的捕获数据，你不想保存很长时间（尽管可以非常容易地配置长期保存）
- 详细的图表

## 支持的Linux操作系统
- CentOS: 7.9
- Ubuntu: 18.04, 20.04, 22.04
- Debian: 10, 11
- 只支持64位操作系统
### 准备安装的Linux主机 
由于Trace服务通常会占用大量的CPU和内存资源，我们建议SIP Trace服务器的硬件规格如下：
#### 最大100个并发通话
- CPU：2-4核
- 内存：2G - 4G
#### 最大1,000个并发通话
- CPU：4核
- 内存：4G
#### 最大5,000个并发通话
- CPU：8 - 10核
- 内存：8G
#### 最大10,000个并发通话
- CPU：16 - 20核
- 内存：16G
> **重要的是**：不要把 PortSIP Trace服务和 PortSIP PBX 安装在同一台服务器上，因为它会消耗大量资源，导致 PBX 不能顺利工作。我们建议将 PortSIP Trace服务与 PortSIP PBX 安装在同一个局域网内。

### 防火墙规则
防火墙和云平台安全组必须开放下面列出的端口：
- TCP 9061端口
- TCP 9060端口
### 安装 PortSIP Trace服务器
- 确保服务器日期时间同步正确
- 必须由**root**用户执行所有Linux命令，请先**su root**
### 下载安装程序
```
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/trace-server/trace-server.sh -o trace-server.sh
```
### 安装并启动服务
```
/bin/sh trace-server.sh start
```
PortSIP Trace服务安装成功后，您可以通过以下网址访问Trace服务:
```
http://trace-server-ip:9080
```
用户名称是admin，密码是admin

### 在PortSIP PBX中启用SIP Trace功能
登录 PortSIP PBX Web界面，点击 "**高级**">"**设置**"，在 "**常规**"页面，填写Trace服务器信息，如下图所示：

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FmfLmtNNo3IUGjKrFT1Xz%2Fportsip_trance_server.png?alt=media&token=1971b913-0a8d-46df-a701-f7cd9bb65df6)

> **重要提示**：用你的Trace服务器 IP 替换 192.168.1.17

在成功设置了SIP Trace服务器信息并点击 "**确定** "按钮后，PBX将把所有的SIP信息发送到Trace服务器上。

### 禁用PortSIP PBX的SIP Trace功能
登录 PortSIP PBX Web界面，点击 "**高级**">"**设置**"，在 "**常规**"页面，**删除Trace 服务器地址**字段中的信息，并点击 "**确定**"按钮，PortSIP PBX 将停止向Trace服务器发送 SIP 消息。
> **重要提示**：SIP Trace在大多数时候应该是关闭的，只有在需要进行故障诊断的时候才启用。

### 疑难解答
你可以选择在SIP Trace服务器网站上显示注册信息或呼叫SIP信息，如下图所示；"**Home** "是呼叫信息，"**REGISTRATION** "是注册信息。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-Mjn3SxageipLMhw8oi3%2F-Mjn49huTGPmFJiqBKtm%2Fimage.png?alt=media&token=0d067bd4-83f1-410b-aba5-9cba9c5c33aa)

你可以通过点击方法名称来查看消息的细节，如下面的截图所示：

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-MjmziWlNwDwJEAAoaEt%2F-Mjn0MgAK5NtHdanDo8d%2Fimage.png?alt=media&token=a524de87-d115-4603-ad62-cd38d2067c2b)

有一种方法可以检查SIP消息的call-id, X-Session-Id, 和X-CID，如下：

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-MjmziWlNwDwJEAAoaEt%2F-Mjn0mRb8ayUHeQWp_Wa%2Fimage.png?alt=media&token=72911057-a8bb-4d88-8110-731ed8ec0dfa)

call-id, X-Session-Id, 和 X-CID现在可以用来搜索SIP消息：

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-MjmziWlNwDwJEAAoaEt%2F-Mjn1baKD1T9h5CmamZR%2Fimage.png?alt=media&token=2bafc148-0222-44c7-83b4-5595ab002d89)
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-MjmziWlNwDwJEAAoaEt%2F-Mjn2T6_mowzIIO0M0lh%2Fimage.png?alt=media&token=28a4949c-69a9-4265-8a56-14a21a3abef9)

从消息列表中，我们可以通过点击会话ID看到呼叫流：

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-MjmziWlNwDwJEAAoaEt%2F-Mjn2vsbCDRuYbXJzygk%2Fimage.png?alt=media&token=f6470b68-2485-4906-8068-416dd6534b07)
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MfkamWLaD5pcQwlKWwC%2F-MjmziWlNwDwJEAAoaEt%2F-Mjn38Wwkn5LduXA9Tym%2Fimage.png?alt=media&token=b5594243-5848-4c7d-8d23-0b8112b7f8cc)

### 管理Trace服务
#### 启动服务
`/bin/sh trace-server.sh start`

#### 停止服务
`/bin/sh trace-server.sh stop`

#### 移除服务器
`/bin/sh trace-server.sh remove`


