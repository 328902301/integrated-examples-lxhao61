介绍：

本配置是 trojan-go 或 trojan 应用，trojan-go 以 http/1.1 协商连接，trojan 以 h2 或 http/1.1 自适应协商连接；非 trojan-go\trojan 的 https 连接回落给 nginx（即解除 tls 后的 web 连接转给 nginx 处理），tls 由 trojan-go\trojan 提供及处理。

原理：

默认流程：trojan-go\trojan client <------ https ------> trojan-go\trojan server  
匹配流程：web client <------------- https ------------> trojan-go\trojan server <-- 回落 --> nginx（web server）

注意：

1、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故回落配置就必须分成 http/1.1 回落与 h2 回落两部分，以便分别对应 nginx 的 http/1.1 server 与 h2c server。又因 trojan-go 开启 Websocket 支持，且目前不支持 http/1.1 回落与 h2 回落分开，故 trojan-go 最终选 http/1.1 协商连接及 http/1.1回落。

2、因 trojan-go\trojan 不支持 Unix Domain Socket，故回落仅端口回落。

3、因 trojan-go\trojan 不支持 PROXY protocol（发送），故回落不能启用此项应用。

4、trojan-go 完全兼容 trojan，还有自己的特色：支持 Websocket，可与一般 Trojan 流量共存；支持 CDN 流量中转(基于 WebSocket over TLS)；支持使用 AEAD 对 trojan 流量二次加密(基于 Shadowsocks AEAD )。

5、trojan-go 的 CDN 流量中转（基于 WebSocket over TLS）与一般 trojan 流量同时使用，仅支持使用通配符证书或 SAN 证书的不同域名实现，因为 trojan-go 不支持设置多组证书及密钥。

6、本示例配置不要使用 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需要占用或监听80端口（或443端口），从而与当前应用端口冲突。
