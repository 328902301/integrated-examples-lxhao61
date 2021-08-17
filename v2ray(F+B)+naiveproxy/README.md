介绍：

Xray\v2ray 前置（监听443端口），利用 trojan+tcp+tls 强大的回落/分流特性，实现与 WebSocket（WS）类应用及 naiveproxy 应用共用443端口。trojan+tcp+tls 以 h2 或 http/1.1 自适应协商连接，分流 WebSocket（WS） 连接，非 Xray\v2ray 的连接回落给 caddy；若有 naiveproxy 就进行正向代理。其应用如下：

1、F=trojan+tcp+tls（回落/分流配置，tls由自己提供及处理。）

2、B=vless+ws+tls（tls由trojan+tcp+tls提供及处理，不需配置；另可改成或添加其它WS类应用，参考对应的服务端单一应用配置示例。）

3、naiveproxy（带有forwardproxy插件的caddy才支持naiveproxy应用，否则仅上边应用。tls由trojan+tcp+tls提供及处理，不需配置。）

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。

2、caddy 不小于 v2.3.0 版才支持 Caddyfile 配置开启 h2c server。

3、caddy 支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

4、使用本人 Releases 中编译好的 caddy 文件，可同时支持 naiveproxy、h2c server 及 PROXY protocol 等应用。

5、本示例的 naiveproxy 仅支持 http/2 代理应用，即 HTTPS 协议传输。

6、本示例中 caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可。若使用 caddy 申请证书及密钥，推荐使用 json 格式配置，优化更好。

7、本示例配置不要使用非 caddy（自带 ACME 客户端）的 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需占用或监听80端口（或443端口），从而与当前应用端口冲突。

8、Xray 所需证书及密钥推荐使用 caddy 申请，配合 Xray（版本必须不低于v1.3.0）自动重载证书及密钥（OCSP Stapling），可实现证书及密钥申请与更新全自动化。

9、配置1：采用端口回落\分流。配置2：采用进程回落\分流。配置3：采用进程回落\分流，且启用了 PROXY protocol。
