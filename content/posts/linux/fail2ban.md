---
title: "Fail2ban"
date: 2023-05-11 12:23:18 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 安装 Fail2ban

```shell
apt-get install fail2ban
```

Fail2ban 默认启用 `ssh` 的过滤器，可以在 `/etc/fail2ban/jail.conf` 查看，但是 Fail2ban 更新会覆盖此文件，建议复制一个 `jail.local` 进行编辑。

```shell
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

此后建议编辑 `jail.local`

对于 jail.local，你能找到许多 unit

例如 sshd 在文件中如此显示

```text
[sshd]
#mode   = normal
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```

输入

```shell
fail2ban-client status
```

```text
Status
|- Number of jail:      1
`- Jail list:   sshd
```

可以看到，sshd的过滤器已经默认启动了。

## 更改默认配置

默认配置在 `jail.local` 的 `[DEFAULT]` 下，推荐至少在 `ignoreip` 中加入自己常用的 ip，（如果不常用有固定公网 ip 登录的倒是无所谓）其他一些配置可以根据自己的需要酌情修改

> 默认日志路径配置（如mail、authpriv、user、ftp等等），依据系统而定，fail2ban在/etc/fail2ban/目录下提供了多种系统日志配置文件

```ini
before = paths-fedora.conf
# before = paths-debian.conf

[DEFAULT]                      #全局配置

ignoreip = 127.0.0.1           #忽略的IP列表，该Ip表将不受限制

bantime = 600                  #被屏蔽时间， 单位：秒

findtime  = 600                #时间间隔，这个时间段内超过规定次数的主机将会被屏蔽

maxretry = 5                   #最大尝试此时

enabled = false                #全局禁止，在jail.local或jail.d/*.conf中开启相关联的配置


destemail = root@localhost     #邮件接收人
sender = root@localhost        #邮件发送人


# action：触发后的动作

#只屏蔽主机
action_ = %(banaction)s[name=%(__name__)s, bantime="%(bantime)s", port="%(port)s", proto    col="%(protocol)s", chain="%(chain)s"]

#屏蔽主机 并发送邮件
action_mw = %(banaction)s[name=%(__name__)s, bantime="%(bantime)s", port="%(port)s", pro    tocol="%(protocol)s", chain="%(chain)s"]
    %(mta)s-whois[name=%(__name__)s, sender="%(sender)s", dest="%(destemail)s",     protocol="%(protocol)s", chain="%(chain)s"]

.......


action = %(action_)s   #默认动作为只屏蔽主机，如需修改，只需在特定服务下配置该属性即可


#服务配置， 以sshd 为例

[sshd]
# 开启防护
enabled = true
# 动作，只屏蔽（action_为默认动作，可省略）
action = %(action_)s
# 时间范围（s）
findtim = 120
# 时间范围内最大尝试次数
maxretry = 3
# 屏蔽时间（s）
bantime = 180

port = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```

保存后启动fail2ban

```shell
systemctl start fail2ban
```

## 添加配置：

`jail.local` 中本来就预设了一些配置，如需添加只需按照自己服务器的情况添加配置，同时添加 `enabled = true` 以启用此 jail

### nginx 认证登录监控

```
[nginx-http-auth]

enabled  = true
filter   = nginx-http-auth
port     = http,https
logpath  = /var/log/nginx/error.log
```

### 禁止一些恶意机器人

```
[nginx-badbots]

enabled  = true
port     = http,https
filter   = nginx-badbots
logpath  = /var/log/nginx/access.log
maxretry = 2
```

### 如果网络服务没有提供根目录的服务，可以开启以禁止访问根目录

```
[nginx-nohome]

enabled  = true
port     = http,https
filter   = nginx-nohome
logpath  = /var/log/nginx/access.log
maxretry = 2
```

### 防止被当作代理服务器

```
[nginx-noproxy]

enabled  = true
port     = http,https
filter   = nginx-noproxy
logpath  = /var/log/nginx/access.log
maxretry = 2
```

> 需要确认打开 Nginx 的 log 记录，并且正确填写目录  
> http 和 https 对应你在前文 NFW 中设置的 app

### 重启以应用更新

```shell
systemctl restart fail2ban
```

以启用这些 jails

