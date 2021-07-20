介绍：

通过 caddy 或 nginx 前置（监听443端口）实现 WebSocket（WS） 反向代理，tls 由 caddy 或 nginx 提供及处理，包括 vless+ws+tls 与 vmess+ws+tls 两种应用。

原理：

默认流程：web client <------ https（http/1.1+tls） ------> caddy\nginx（web server）  
匹配流程：Xray\v2ray client <------ WebSocket+tls ------> caddy\nginx <-- WebSocket --> Xray\v2ray server

注意：

1、若采用 caddy 反向代理，本示例中 caddy 的 Caddyfile 格式配置与 json 格式配置二选一即可（效果一样）。支持自动 https，即自动申请与更新证书与私钥，自动 http 重定向到 https。

2、若采用 nginx 反向代理，如果系统版本过低，其对应发行版仓库自带 nginx 预编译程序包可能不支持 tls1.3；如需要支持 tls1.3，必须先升级 OpenSSl 版本大于 1.1.1，再进行 nginx 源代码编译和安装。

3、若采用 nginx 反向代理，本示例配置不要使用 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需要占用或监听80端口（或443端口），从而与当前应用端口冲突。

4、配置1：采用端口转发。配置2：采用进程转发。
