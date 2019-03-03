---
sidebar: auto
title: MySQL 管理
---

# 数据库推荐

数据库是一个动态网站网站的最核心部分，数据库一旦出现问题，即便 Web 服务器和脚本程序都正常，网站依旧是无法被打开的。但是如果要自管理数据库则需要极高的学习成本，即把自己变成一个半吊子DBA这也需要经年累月的学习成本。

## 云数据库

云数据库也可以被叫做托管型数据库，一般会是提供图形化的管理界面并由非常好的备份和恢复功能。同时针对不同配置规格，都会默认配置好最好的数据库参数充分发挥数据性能。简单一句话概括就是：**在没有DBA的情况下满足你对数据库所有想象。**

下图是阿里云云数据库控制台的界面截图，可以看到由非常丰富的功能。

![enter description here](https://pics.mf8.biz/xsj/2019/2/1550997599892.png)

当然了，唯一不推荐用云数据库的理由就是贵。一款最基础1核心1G内存的云数据库就会要约1500元/年的价格。也推荐在相关优惠促销的时候入手云数据库。

适合已经有一定流量且带来一定盈利性的论坛、门户、博客等网站。

## 面板管理

如果咱们并没有太大的流量所带来对高配置的需求，那么我们就可以把数据库、Web服务和执行脚本放一起。

不过对于没有任何数据库经验的朋友，我们依旧推荐使用图形化界面，而APPNode就可以提供这样方便的功能。

一、我们需要安装数据库，在 **软件管家** 中安装对应版本的 MySQL 服务器和客户端。

![](https://pics.mf8.biz/picgo20190228234625.png)

二、进入 **MySQL服务器** 选项，我们就可以进入连接数据库了，默认安装好密码是空的，我们先`启动数据库`，然后勾选 `自启` 和 `守护`

![enter description here](https://pics.mf8.biz/xsj/2019/2/1551007601639.png)

三、然后进入**密码设置**，设置一下咱们的管理员密码。 第一次的化 `原密码` 留空即可。 相比后续忘记密码怎么办的入口我们也已经看到了。

![](https://pics.mf8.biz/picgo20190228234956.png)

四、然后就可以在`数据库管理`创建数据库，在`用户管理`创建用户了。 当然也可以用我们默认首页里的 PHP My Admin 或者 Adminer 来管理数据库。

![](https://pics.mf8.biz/picgo20190228235107.png)

五、**参数配置** 可以帮助我们可视化的理解 `my.cnf` 文件，当然了，如果不熟悉的话建议保持默认即可。

![](https://pics.mf8.biz/picgo20190228235329.png)

六、**内存优化器** 会帮助我们根据服务器内存的大小，来提供预设好的参数更好的发挥数据库的性价比。

![](https://pics.mf8.biz/picgo20190228235412.png)

七、设置完了记得重启。

## 命令行管理

LLStack 非面板版默认也提供了MySQL 5.5~8.0 和 MariaDB 5.5~10.3  的支持。这个管理更适合对数据库更理解的同学使用。

| MariaDB 位置 |                     路径                     | 备注 |
| :----------: | :------------------------------------------: | ---- |
|    my.cnf    | /etc/my.cnf.d/server.cnf , mysql-clients.cnf |      |
|     目录     |                /var/lib/mysql                |      |

| MySQL 位置 |      路径      | 备注 |
| :--------: | :------------: | ---- |
|   my.cnf   |  /etc/my.cnf   |      |
|    目录    | /var/lib/mysql |      |

## 站库分离

站库分离即网站服务器和数据库服务器分离，这样的好处是不同软件之间相互隔离，安全性的提升还有就是可以节省成本，例如 MySQL 就可以只买大内存和SSD磁盘实例，来提供更好的性能。 

这样也是和 LiteSpeed 免费版授权只允许最大 2G 内存的情况，进行分离可以有效提升性能。

### 面板站库分离

一、开放防火墙，进入 **防火墙**，然后针对内网开放 `3306` 端口，然后 `重载`

![](https://pics.mf8.biz/picgo20190301000349.png)

二、 然后再 **数据库管理** —— **用户管理** 处创建新用户，然后**主机**处填写`%`，或者欲开放使用数据库的内网IP ，建议先用 % 后面采用改成内网ip，以避免无法连接然后各种排错找不到原因的情况。

![](https://pics.mf8.biz/picgo20190301000618.png)

三、服务器的安全组将 `3306` TCP协议授权给对应使用的内网ip，一定不可以用`0.0.0.0/0` 开放给所有。

四、内网连接数据库进行调试，将数据库驱动配置文件的 `localhost` 或者 `127.0.0.1`修改为指定内网数据库IP地址即可。

### 命令行站库分离

一、开放防火墙，在 SSH 终端中输入：

```bash
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```

同时服务器的安全组将 `3306` TCP协议授权给对应使用的内网ip，一定不可以用`0.0.0.0/0` 开放给所有。

二、输入下面的命令，关闭一些不安全的设置：

```bash
mysql_secure_installation  
```

> Enter current password for root (enter for none):
>
> 解释：输入当前 root 用户密码，默认为空，直接回车。
>
> Set root password? [Y/n]  y
>
> 解释：要设置 root 密码吗？ 输入 n，我都有设置密码了。
>
> Remove anonymous users? [Y/n]  y
>
> 解释：要移除掉匿名用户吗？输入 y 表示愿意。
>
> Disallow root login remotely? [Y/n]  y
>
> 解释：不想让 root 远程登陆吗？输入 n 表示不，DMS就是 root 账号的远程登录，。
>
> Remove test database and access to it? [Y/n]  y
>
> 解释：要去掉 test 数据库吗？输入 y 表示愿意。
>
> Reload privilege tables now? [Y/n]  y
>
> 解释：想要重新加载权限吗？输入 y 表示愿意。

这里第四个选项 **Disallow root login remotely?** 输入 `n`， 其他一律 `y` 即可

三、连接登陆数据库

```bash
mysql -uroot -p你的密码
```

或者登陆 phpMyAdmin

四、修改数据库账户的`主机名`，可以先修改为 `%` 之后调试成功了再修改为指定使用的服务器内网IP。

![](https://pics.mf8.biz/picgo20190301134814.png)

如果使用命令行语句就是：

```bash
GRANT ALL PRIVILEGES ON *.* TO '数据库账户'@'%' IDENTIFIED BY '数据库密码' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

五、修改数据库 `my.cnf` 

将 `bind-address = 127.0.0.1` 修改为

```bash
bind-address    = 0.0.0.0
```

重启数据库即可。

六、内网连接数据库进行调试，将数据库驱动配置文件的 `localhost` 或者 `127.0.0.1`修改为指定内网数据库IP地址即可。