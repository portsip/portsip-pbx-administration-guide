# 坐席状态和工作模式
坐席状态和指定了坐席所处的状态。例如，一个处于 "redy"状态的坐席可以处理来自ACD队列的呼叫。对于不同的ACD设备，一个坐席可以有几个状态，或者他可以用一个状态来描述他与所有ACD设备的关系。坐席的状态在[queue events](https://support.portsip.com/portsip-pbx-administration-guide/going-real-time-with-portsip-pbx-pub-sub#queue_events)中报告。  
  
下面的坐席状态图显示了坐席的状态。状态之间的转换，用箭头表示，显示了从一个给定状态可能进入的后续状态。你可以使用[WSI](https://support.portsip.com/portsip-pbx-administration-guide/going-real-time-with-portsip-pbx-pub-sub)来接收状态事件。  
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FP32DhMJ1Us7AP1t6QSTF%2FAgent_status.png?alt=media&token=081cdcfb-b4e0-4323-a24a-ffba671d2281)  
## 1 坐席状态
### Logged Out
坐席从呼叫队列中签出的状态，ACD将不再向该坐席分配呼叫。
### Not Ready
坐席登录到呼叫队列中，但不准备处理ACD分配的呼叫的状态。在这种状态下，坐席可以接收ACD不处理的呼叫。
### Ready
坐席登录到呼叫队列并准备处理ACD分配的呼叫的状态。
### Queue Call / Other Queue
坐席在当前呼叫中，不能处理ACD分配的新呼叫的状态。排队呼叫表示坐席在ACD呼叫中，而其他呼叫表示坐席在非ACD呼叫中。
### Wrap Up
也被称为 "After Call Work"。坐席不再参与ACD呼叫的状态。在这种状态下，座席正在为以前的呼叫执行管理职责，不能再接受来自ACD的呼叫。
## 2 管理坐席状态
PortSIP提供了一些选项，使坐席能够控制其状态。使用这些选项来改变坐席的状态。  
  
>如果您在创建呼叫队列时打开了 "**自动设置队列成员为 "准备接听" 状态**"，您就不需要手动将座席状态设置为 "**ready**"。
## FAC - 功能访问代码
坐席可以拨打后面的功能访问代码来改变他们在该呼叫队列中的状态，或改变他们在所有他们是成员的呼叫队列中的状态。   
1. 将坐席签入到队列中。拨`*38`可签入到该坐席所属的所有队列；拨`*388000`可签入到号码为`8000`的队列。签入后，如果队列勾选了 "**自动设置队列成员为 "准备接听" 状态**"，坐席状态将改为 "**ready**"，否则将为 "**Not ready**"。
2. 将坐席从队列中签出。拨`*39`，签出所有队列；拨`*398000`，迁出号码为`8000`的队列。
3. 将坐席设置为ready。将坐席状态改为队列的 "**ready**"。拨`*36`，将坐席所属的所有队列的坐席状态改为 "**ready**"；拨`*368000`，将号码为`8000`的队列的坐席状态改为 "**ready**"。
4. 将坐席设置为Not ready状态。将坐席状态改为队列的 "**Not ready**"。拨`*37`，将坐席所属的所有队列的坐席状态改为 "**Not ready**"；拨`*378000`，将号码为`8000`的队列的坐席状态改为 "**Not ready**"。
## REST API
PortSIP还支持通过调用REST API改变坐席状态   
1. [将坐席签入队列](https://www.portsip.com/pbx-rest-api/v16/html/index.html#tag/Call-Queue/operation/loginQueueAgent)
2. [将坐席签出队列](https://www.portsip.com/pbx-rest-api/v16/html/index.html#tag/Call-Queue/operation/logoutQueueAgent)
3. [设置坐席状态](https://www.portsip.com/pbx-rest-api/v16/html/index.html#tag/Call-Queue/operation/setQueueAgentStatus)
## 3 实时接收坐席状态事件
PortSIP PBX 提供了一个实时机制，允许管理员和队列管理员订阅队列和坐席状态；PBX 将通过 WebSocket 将相关事件推送给订阅者；更多细节请参考[Going Real-Time with PortSIP PBX Pub/Sub](https://support.portsip.com/portsip-pbx-administration-guide/going-real-time-with-portsip-pbx-pub-sub#queue_events).

