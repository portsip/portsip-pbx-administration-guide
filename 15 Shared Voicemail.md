# 15 共享语音邮件
在某些情况下，您公司内的分机或组可能会共享一个语音邮箱，如何将呼叫转到这个语音邮箱 。比如分配给公司主要电话号码、虚拟接待、振铃组或呼叫队列的语音邮件。共享语音邮箱有多种使用方式。下面是一些例子：

例子A：有一个全公司的语音邮箱，当有人拨打主叫号码，到达自动接听，并按下5号键留言，或者当任何一个组（销售组或IT组）没有接听时，就可以进入这个语音邮箱。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FXjgwncASk4mFBoUFob6B%2Fshared_vm.png?alt=media&token=97f25ad0-7532-480d-9d5d-7f9dad435d95)

## 15.1 创建一个共享的语音邮件
- 在 PortSIP PBX Web页面中，选择"**通话管理 > 高级服务 > 共享语音邮件**"，点击"**添加**"按钮。
- 现在输入下面的字段：
    - **分机号码** - 共享语音邮件号码，根据需要指定一个新的号码，请不要指定已有的号码
    - **显示名称** - 为共享语音邮件输入一个友好的名称
    - **提示语言** - 选择提示语言
    - **密码** - 设置一个语音邮件的密码
    - **邮箱** - 设置一个用于接收语音邮件通知的邮箱
    - **语音邮箱密码认证** - 如果这个选项被打开，当用户访问这个共享语音邮件时，PBX将验证语音邮件的密码
    - **在收到新的语音邮件时发送邮件通知** - 设置当有新的语音留言时是否发送电子邮件通知

## 15.2 呼叫转到共享语音邮件
对于以下情况，我们可以将呼叫转到共享语音邮件。

### 呼叫队列
当队列无人接听时转到共享语音邮件。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2F57O2RPanEJGBzFD3shlg%2Fshared_vm_1.png?alt=media&token=00002eed-275d-4bd0-b5f8-5fa365d933b6)

### 振铃组
当振铃组无人接听时转到共享语音邮件。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FXqxjtQmuS58V83AUU3qb%2Fshared_vm_2.png?alt=media&token=0b9d1239-830b-4c33-96b2-2e435f9d6e96)

### 虚拟接待
当虚拟接待呼叫失败时转到共享语音邮件。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2F8iLbGdF0BK2qKX7uXT09%2Fshared_vm_3.png?alt=media&token=926d42fa-b957-452a-8f60-c9b19289d581)

### 分机
当分机无人接听时转到共享语音邮件。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FO8qq3wFsqvojwiFqmsnb%2Fshared_vm_4.png?alt=media&token=5c4b2261-c320-41d1-8bf6-de56c3999d9f)

### 接入规则
将接入规则的来电转到共享语音邮件。

![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2FmNNgVhnnHoM2y4ywe8GN%2Fshared_vm_5.png?alt=media&token=e7473d65-d5a9-47f8-bbea-94377d697503)

## 15.3 如何访问共享语音邮件

### 从IP电话、APP收听共享语音邮件
从任何IP电话或APP，拨打`*56`后面跟共享语音邮件号码（比如 `*5670000`）。在问候语中，按键输入语音邮件密码。

### 从PBX Web页面访问共享语音邮件。
在 PortSIP PBX Web页面中，选择 "**通话管理 > 高级服务 > 共享语音邮件**"，**双击**一个共享语音邮件，或者选择一个共享语音邮件然后点击"**编辑**"按钮，点击 "**语音邮箱**"页面。

在语音邮件列表中，可以下载，播放，或删除语音邮件。
![](https://4230641821-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MfkamWLaD5pcQwlKWwC%2Fuploads%2F5XrtLNs229qC2EWcTQDS%2Fshared_vm_6.png?alt=media&token=fd4f9e45-c9e1-48aa-924a-4664b46bd890)
