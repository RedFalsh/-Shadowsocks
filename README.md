# -Shadowsocks
Shadowsocks翻墙上网

## 官网
https://github.com/shadowsocks

## android下载
https://github.com/shadowsocks/shadowsocks-android/releases

## 配置参考链接-archlinux
https://wiki.archlinux.org/index.php/Shadowsocks_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#.E5.AE.89.E8.A3.85

## centos 安装配置服务器开机启动
https://morning.work/page/2015-12/install-shadowsocks-on-centos-7.html

pip install --upgrade pip
pip install shadowsocks
### 服务器配置文件
server.json
```
{
  "server": "0.0.0.0",
  "server_port": 8388,
  "password": "you password",
  "method": "aes-256-cfb"
}
```
### 说明：

    method为加密方法，可选aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table
    server_port为服务监听端口
    password为密码，可使用密码生成工具生成一个随机密码


以上三项信息在配置 shadowsocks 客户端时需要配置一致，具体说明可查看 shadowsocks 的帮助文档。
### 配置自启动

新建启动脚本文件/etc/systemd/system/shadowsocks.service，内容如下：
```
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target
```
### 执行以下命令启动 shadowsocks 服务：

`systemctl enable shadowsocks`
`systemctl start shadowsocks`
为了检查 shadowsocks 服务是否已成功启动，可以执行以下命令查看服务的状态：

`systemctl status shadowsocks -l`

如果服务启动成功，则控制台显示的信息可能类似这样：
```
● shadowsocks.service - Shadowsocks
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2015-12-21 23:51:48 CST; 11min ago
 Main PID: 19334 (ssserver)
   CGroup: /system.slice/shadowsocks.service
           └─19334 /usr/bin/python /usr/bin/ssserver -c /etc/shadowsocks.json

Dec 21 23:51:48 morning.work systemd[1]: Started Shadowsocks.
Dec 21 23:51:48 morning.work systemd[1]: Starting Shadowsocks...
Dec 21 23:51:48 morning.work ssserver[19334]: INFO: loading config from /etc/shadowsocks.json
Dec 21 23:51:48 morning.work ssserver[19334]: 2015-12-21 23:51:48 INFO     loading libcrypto from libcrypto.so.10
Dec 21 23:51:48 morning.work ssserver[19334]: 2015-12-21 23:51:48 INFO     starting server at 0.0.0.0:8388
```

## 客户段配置参考
https://zzz.buzz/zh/gfw/2017/08/14/install-shadowsocks-server-on-centos-7/

### client.json文件如下：
```
{
	"server": "ss.zzz.buzz",
	"server_port": 8388,
	"local_address": "0.0.0.0",
	"local_port": 1080,
	"password": "you password",
	"method": "aes-256-cfb",
	"mode": "tcp_and_udp"
}
```
