---
title: 配置ProxyChains，使得终端可以使用代理
description: 让无图形机器使用代理
slug: ProxyChains
date: 2023-10-30 22:00:00+0800
image: 2.jpg
categories:
    - proxychains
    - proxy
tags:
    - proxychains
    - proxy 
    - socks5
---

兄弟这两天忽然找我，他们实验室服务器因为配置原因，无法直接访问外网，但是他们需要下载一些东西，于是就想到了使用代理。因为一台机器中存在多个用户，自然不能在系统中直接配置代理，经过一翻查找，就想到了使用proxychains。

# 基本环境
简介也说明了，因为实验室的服务器提供给多人使用，部分用户是没有管理员权限的（避免因此造成不必要的麻烦），所以不能在系统中直接配置代理，这样一方面是对路由资源会造成损耗，另一方面，也不太方便在代理和直连之间切换。所以，使用proxychains是一个不错的选择。

# 安装proxychains
因为实验室服务器使用的是Debian，所以使用apt安装即可。

```
sudo apt install proxychains
```

# 配置proxychains
一般来说，proxychains的配置文件在/etc/proxychains.conf，但是我们强调，因为上述特殊环境，我们需要在不同用户下使用不同的配置，所以我们需要在用户目录下创建配置文件，这样就可以实现不同用户使用不同的配置。

```
vim ~/.proxychains/proxychains.conf
```

在这个配置文件中，我们需添加以下内容:

```
[ProxyList]
socks5 127.0.0.1 1080 username password
```

其中，username和password是你的代理服务器的用户名和密码，如果没有，可以不填写。服务器地址和端口可按照需求修改。

# 使用proxychains

使用proxychains很简单，只需要在命令前加上proxychains即可，例如:

```
proxychains git clone https://github.com/Coldin04/NoteShare.git
```

或者，也有另外一种方式：`proxychains bash`

这样，就可以在一个新的bash中使用代理了。

# 测试连通性：
注意：如果使用的是socks5代理，因为socks5代理处于第五层，所以ping命令是无法使用的，只能使用curl或者wget等命令。
`proxychains curl cip.cc`


# 参考资料

[[1] proxychains-ng](https://github.com/haad/proxychains) <br>
[[2] Linux 配置 ProxyChains 本地代理](https://github.com/haad/proxychains)