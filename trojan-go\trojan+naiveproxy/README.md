介绍：

本示例包括 trojan-go\trojan 与 naiveproxy 应用。trojan-go\trojan 以 h2 或 http/1.1 自适应协商连接，非 trojan-go\trojan 的 https 连接回落给 caddy；caddy 再处理，若是 naiveproxy 数据就进行正向代理。其应用如下：

1、trojan-go或trojan（回落配置，tls由自己提供及处理。）

2、naiveproxy（带有forwardproxy插件的caddy才支持naiveproxy应用。tls由trojan-go\trojan提供及处理，不需配置。）

注意：

1、caddy 不小于 v2.3.0 版才支持 Caddyfile 配置开启 h2c server。

2、caddy 支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）。

3、使用本人 Releases 中编译好的 caddy 文件，可支持 naiveproxy 等应用。

4、本示例中 caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可（效果一样）。

5、因 trojan-go\trojan 不支持 Unix Domain Socket，故回落仅端口回落。

6、因 trojan-go\trojan 不支持 PROXY protocol（发送），故回落不能启用此项应用。

7、trojan-go 完全兼容原版 trojan，trojan-go 应用（服务端）还有自己的特色：支持同时提供原版 trojan 应用与自己的 Websocket 应用；支持 CDN 流量中转(基于 WebSocket over TLS)；支持使用 AEAD 对 trojan 流量二次加密(基于 Shadowsocks AEAD )。

8、trojan-go 的 CDN 流量中转（基于 WebSocket over TLS）与 trojan 流量同时使用，仅支持使用通配符证书或 SAN 证书的不同域名实现，因为 trojan-go 不支持设置多组证书及密钥。

9、本示例配置不要使用非 caddy（自带 ACME 客户端） 的 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需占用或监听80端口（或443端口），从而与当前应用端口冲突。
