---
title: 代理
---

## Ubuntu

1. 安装 `shadowsocks-libev`。

   ```bash
   sudo apt install shadowsocks-libev
   ```

2. 配置文件 `~/.shadowsocks`

   ```json
   {
                   "server": "x.x.x.x",
                   "local_address": "127.0.0.1",
                   "local_port": 1080,
                   "timeout": 300,
                   "workers": 1,
                   "server_port": 21259,
                   "password": "123456",
                   "method": "chacha20-ietf",
                   "plugin": ""
   }
   ```

3. 配置 `git` 的 https 代理

   ```bash
   git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
   ```

4. 启动代理

   ```bash
   ss-local -c ~/.shadowsocks
   ```

   

