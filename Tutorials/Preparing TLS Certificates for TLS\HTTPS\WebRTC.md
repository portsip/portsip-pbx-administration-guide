# 为TLS/HTTPS/WebRTC准备TLS证书
本指南用于解决PortSIP PBX和PortSIP SBC的以下SSL证书问题。
- 当你完成了 PBX 和 SBC 的设置后，当你通过 HTTPS 访问 PBX Web页面或访问 WebRTC 客户端时，如果在浏览器中出现自签名证书的警告，请按照以下步骤进行解决
- SSL 证书已过期

请采取以下步骤：
1. 为你的 PBX 和 SBC 从域名提供商（例如[Godaddy](https://www.godaddy.com/)）购买一个域名（例如 ```portsip.cc```）。
2. 在域名的 DNS 区域添加 A 记录，并将域名解析到你的 PBX IP，例如：将 ```uc.portsip.cc``` 指向 PBX 服务器 IP。
3. 如果你的 SBC 是与 PBX 服务器分开部署的，添加一个 A 记录的 DNS 记录并解析到 SBC 服务器 IP，例如：指向 ```sbc.portsip.cc``` 到 SBC 服务器 IP。
4. 从信任的证书提供商那里为你的域名购买证书，例如 [Digicert](https://www.digicert.com/), [Thawte](https://www.thawte.com/), [GeoTrust](https://www.geotrust.com/)；如果你的 SBC 是与 PBX 服务器分开部署的，请购买```Wildcard Certificate```。
5. 根据证书提供商的指导，生成 CSR 文件和私钥文件，并保存文件。生成私钥文件时请不要设置密码，通常你会有两个文件：证书和私钥。**注意**：请选择Nginx的证书。
6. 将私钥文件重命名为 ```portsip.key```。
7. 向证书提供商提交CRS文件，并在你的证书被批准后下载证书文件。这一步最后会有两个文件：```Intermediate CA 证书```和 ```SSL 证书```。**注意**：有些供应商没有```Intermediate CA 证书```。
 8. 如果你的供应商不提供```Intermediate CA 证书```，请忽略此步骤。使用纯文本编辑器，如Windows记事本（不要使用MS Word）打开```Intermediate CA 证书```和```SSL证书```文件，复制```Intermediate CA ```内容附加到```SSL证书```文件，并将```SSL证书```文件重命名为```portsip.pem```。在 Linux 环境中，你可以使用下面的命令来合并证书文件。
```
// Append intermediate file to certificate file
cat intermediate.pem >> cert.pem
// Rename certifiate file to portsip.pem
mv cert.pem portsip.pem
```
9. 现在你将有两个证书文件，证书文件 ```portsip.pem``` ，和私钥文件 ```portsip.key``` 。
