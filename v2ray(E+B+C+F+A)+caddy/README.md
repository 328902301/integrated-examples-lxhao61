介绍：

Xray\v2ray 前置（监听443端口），vless+tcp+tls 以 h2 或 http/1.1 自适应协商连接，分流 WebSocket（WS） 连接，回落给 trojan+tcp，trojan+tcp 处理后再回落给 caddy。其应用如下：

1、E=vless+tcp+tls（回落/分流配置，TLS由自己提供及处理。）

2、B=vless+ws+tls（TLS由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它WS类应用，参考对应的服务端单一应用配置示例。）

3、C=SS+v2ray-plugin+tls（TLS由vless+tcp+tls提供及处理，不需配置。）

4、F=trojan+tcp+tls（TLS由vless+tcp+tls提供及处理，不需配置。）

5、A=vless+kcp+seed（可改成vmess+kcp+seed，或添加它。）

注意：

1、v2ray 版本不小于 v4.31.0 才支持 trojan 协议。

2、caddy 版本不小于 v2.3.0 才支持 Caddyfile 配置开启 H2C server。

3、caddy 支持 HTTP/1.1 server 与 H2C server 共用一个端口或一个进程（Unix Domain Socket 应用）。

4、caddy 发行版不支持 PROXY protocol 接收。如要支持 PROXY protocol 接收需选 caddy2-proxyprotocol 插件定制编译，或下载本人 Releases 中编译好的 caddy 来使用即可。

5、本示例采用的是套娃方式实现共用443端口，支持 vless+tcp+tls 与 trojan+tcp+tls 完美共存，且仅需要一个域名及普通证书即可搞定，但 trojan+tcp+tls 不支持 xtls 应用。

6、本示例 caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可。若使用 caddy 申请证书及密钥，推荐使用 json 格式配置，优化更好。

7、不要使用非 caddy（自带 ACME 客户端）的 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需占用或监听80端口（或443端口），从而与当前应用端口冲突。

8、Xray 所需证书及密钥推荐使用 caddy 申请，配合 Xray（版本必须不低于v1.3.0）自动重载证书及密钥（OCSP Stapling），可实现证书及密钥申请与更新全自动化。

9、配置1：采用端口回落\分流、端口转发。配置2：采用进程回落\分流、端口转发。配置3：采用进程回落\分流、端口转发，且启用了 PROXY protocol。
