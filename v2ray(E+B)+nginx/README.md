介绍：

Xray\v2ray 前置（监听443端口），利用 vless+tcp+tls 强大的回落/分流特性，实现了共用443端口。vless+tcp+tls 以 h2 或 http/1.1 自适应协商连接，分流 ws（WebSocket）连接，非 Xray\v2ray 的 web 连接回落给 nginx。其应用如下：

1、E=vless+tcp+tls（回落/分流配置，tls由自己提供。）

2、B=vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加其它grpc类应用，参考对应的服务端单一应用配置示例。）

注意：

1、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故回落配置就必须分成 http/1.1 回落与 h2 回落两部分，以便分别对应 nginx 的 http/1.1 server 与 h2c server。

2、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module（必须加） 及 stream_realip_module（可选加） 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

3、本示例配置不要使用 ACME 客户端在当前服务器上申请与更新普通证书及密钥，因普通证书及密钥申请与更新需要占用或监听80端口（或443端口），从而与当前应用端口冲突。

4、配置1：采用端口回落\分流。配置2：采用进程回落\分流。配置3：采用进程回落\分流，且启用了 PROXY protocol。
