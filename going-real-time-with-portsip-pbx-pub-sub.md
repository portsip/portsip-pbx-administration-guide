# 利用PortSIP PBX的Pub/Sub实现实时通信
PortSIP PBX提供基于WebSocket（PortSIP WSI）的Pub/Sub机制。用户可以用任何编程语言创建WebSocket来订阅PBX事件，一旦订阅的事件发生，PortSIP PBX将自动推送事件信息给订阅者，信息为JSON格式。
  
支持版本：16.0版本或更高
## 服务
PortSIP PBX/UCaaS 提供的 WSI 端口为 8885，服务器必须在防火墙上允许该端口为 TCP，需要使用 SSL。
## 主题和密钥
PortSIP PBX为Pub/Sub提供以下主题和密钥。
## extension_events
所有与分机有关的事件消息都将由` extension_events `发布。下面，是各种消息的键。
+ `extension_register`: 分机在PBX上注册或从PBX上取消注册。  
例如，分机102注册到PBX，用户会收到以下信息:
```
{
    "event_type":"extension_register",
    "extension":"sip:102@sipiw.com",
    "extension_id":"494521070842286080",
    "registration_contacts":[
        {
            "contact_uri":"<sip:102@113.246.194.98:10061>;+sip.instance=\"<urn:uuid:F36DDB5A-5CC3-A59A-0FB2-E53F7A56F245>\"",
            "expires":1632668422,
            "instance":"<urn:uuid:F36DDB5A-5CC3-A59A-0FB2-E53F7A56F245>",
            "last_update":1632668122,
            "public_address":"113.246.194.98:10061",
            "received_from":"113.246.194.98:10061",
            "reg_id":0,
            "user_agent":"Test SIPMaster for IOS"
        },
        {
            "contact_uri":"<sip:102@113.246.194.98:10243>;+sip.instance=\"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>\"",
            "expires":1632668476,
            "instance":"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>",
            "last_update":1632668176,
            "public_address":"113.246.194.98:10243",
            "received_from":"113.246.194.98:10243",
            "reg_id":0,
            "user_agent":"PortSIP UC Client iOS - v16.0.001"
        }
    ],
    "tenant_id":"236047118140313600",
    "time":"1632668176"
}
```
`extension`是表示哪个分机发生了注册事件，`extension_id`是该分机的ID。 该数组·registration_contacts·包括当前的注册信息。在这个例子中，有两个SIP客户端设备/应用程序注册到PBX，它们的代理是：`Test SIPMaster for IOS` 和 `PortSIP UC Client iOS - v16.0.001`，如果这两个客户端都没有在PBX上注册，数组 "`registration_contacts`"将不会出现，见下面的例子：
```
{
    "event_type":"extension_register",
    "extension":"sip:102@sipiw.com",
    "extension_id":"494521070842286080",
    "removed_contacts":[
        {
            "contact_uri":"<sip:102@113.246.194.98:10243>;expires=0;+sip.instance=\"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>\"",
            "expires":1632668724,
            "instance":"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>",
            "last_update":1632668724,
            "public_address":"113.246.194.98:10243",
            "received_from":"113.246.194.98:10243",
            "reg_id":0,
            "user_agent":"PortSIP UC Client iOS - v16.0.001"
        }
    ],
    "tenant_id":"236047118140313600",
    "time":"1632668724"
}
```
在上面的信息中，数组`registration_contacts`不再存在，而是有一个数组`removed_contacts`，表示哪个客户端应用/设备从PBX上取消了注册，比如，客户端`AppPortSIP UC Client iOS - v16.0.001`从PBX上取消了注册，现在PBX上没有任何注册，所以分机102是离线的。   
+ `extension_presence`:分机更改了它的在线状态  
  
一旦一个分机改变了他的存在状态，就会发生`extesnion_presence`事件，见下面的例子：
```
{
    "call_id":"mbZA5vcQHZOkrPZfHjtsdQ..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_noanswer",
    "session_id":"494536565997965312",
    "tenant_id":"236047118140313600",
    "time":"1632671510"
}
```
在上面的例子中，分机102将其存在状态改为 "`Away`"。

+ `call_hold`: 呼叫被保留  
  
一旦一个已建立的呼叫被主叫或被叫者保持，就会发生`call_hold`事件，见下面的例子：
```
{
    "aor":"sip:102@sipiw.com",
    "call_id":"09jV-7QM2B-3E0Q-m20exw..",
    "event_type":"call_hold",
    "peer_aor":"sip:103@sipiw.com",
    "session_id":"494527456623988736",
    "tenant_id":"236047118140313600",
    "time":"1632669389"
}
```
在上面的例子中，呼叫由分机102保持，而呼叫的另一方是分机103
+ `call_unhold`: 呼叫已从保持状态恢复 
   
一旦呼叫从保持状态恢复，就会发生`call_unhold`事件。
```
{
    "aor":"sip:102@sipiw.com",
    "call_id":"09jV-7QM2B-3E0Q-m20exw..",
    "event_type":"call_unhold",
    "peer_aor":"sip:103@sipiw.com",
    "session_id":"494527456623988736",
    "tenant_id":"236047118140313600",
    "time":"1632669492"
}
```
在上面的例子中，呼叫由分机102解挂，而呼叫的另一方是分机103。
+ `call_start`: 呼叫开始。  
  
当呼叫方开始给被呼叫方打电话时，会发生这个事件。
```
{
    "call_id":"rjFyDxQSbXPxXAd1IR8W-g..",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "caller_display_name":"",
    "cdr_id":"494533487185891328",
    "direction":"ext",
    "event_type":"call_start",
    "request_id":"0",
    "session_id":"494533487177502720",
    "tenant_id":"236047118140313600",
    "time":"1632670771"
}
```
在上面的例子中，分机102呼叫被叫者103，`direciton`是`ext`表示呼叫在分机之间。`session_id`是这个呼叫的唯一ID，呼叫完成后，我们可以使用这个session_id通过REST API查询录音文件。
+ `call_established`: 呼叫被接听并成功连接
```
{
    "call_id":"K3jV2vbPzplkrC3cIBTGPg..",
    "call_target":"sip:103@sipiw.com",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_established",
    "session_id":"494535522404798464",
    "tenant_id":"236047118140313600",
    "time":"1632671262"
}
```
在上面的例子中，呼叫是由被叫者分机103接听的。
+ `call_ended`: 呼叫已经结束。
```
{
    "call_id":"K3jV2vbPzplkrC3cIBTGPg..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_ended",
    "session_id":"494535522404798464",
    "tenant_id":"236047118140313600",
    "time":"1632671272"
}
```
在上面的例子中，呼叫已经结束。
+ `call_noanswer`: 在指定的时间（秒）内没有人接听电话，然后由于超时而挂断。
```
{
    "call_id":"mbZA5vcQHZOkrPZfHjtsdQ..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_noanswer",
    "session_id":"494536565997965312",
    "tenant_id":"236047118140313600",
    "time":"1632671510"
}
```
+ `call_reroute`: 呼叫被重新分配到另一个目标。
+ `call_fail`: 呼叫失败了。
```
{
    "call_id":"9SyfcKeZvuIVipZmefINbw..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_fail",
    "fail_code":480,
    "session_id":"494537752386211840",
    "tenant_id":"236047118140313600",
    "time":"1632671788"
}
```
在上面的例子中，102呼叫103，但是103离线，所以呼叫失败，fail_code是480，与SIP标准的状态码相同。   
+ `target_add`: 开始呼叫一个目标。例如，分机101从IP电话或应用程序注册到PBX，当有人呼叫101时，IP电话或应用程序将被添加为目标。(target_add事件将被触发两次）。
+ `target_ringing`: 被叫的目标正在响铃。
+ `target_noanswer`: 被呼叫的目标没有回答。
+ `target_fail`: 来自被叫目标的呼叫失败。例如，应用程序/IP电话拒绝了该呼叫。
+ `target_ended`: 被叫目标的呼叫已经结束。例如，App / IP电话挂断了电话。
## cdr_events
一旦一个呼叫结束，这个呼叫的CDR将被推送给用户，消息主题是：`cdr_events`，消息键如下。
+ `call_cdr`：一旦一个呼叫结束，CDR将被打包成JSON格式并推送给用户。
