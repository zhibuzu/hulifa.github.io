---
layout: post
title: "php-fpm：PHP FastCGI进程管理器介绍"
date: 2019-10-26 18:33 +0800
tags: ["php", "php-fpm", "fastcgi"]
categories: php
---

FPM（FastCGI 进程管理器）用于替换 PHP FastCGI 的大部分附加功能，对于高负载网站是非常有用的。

### 功能介绍

- 支持平滑停止/启动的高级进程管理功能；

  

- 可以工作于不同的 uid/gid/chroot 环境下，并监听不同的端口和使用不同的 php.ini 配置文件（可取代 safe_mode 的设置）；

- stdout 和 stderr 日志记录;

- 在发生意外情况的时候能够重新启动并缓存被破坏的 opcode;

- 文件上传优化支持;

- "慢日志" - 记录脚本（不仅记录文件名，还记录 PHP backtrace 信息，可以使用 ptrace或者类似工具读取和分析远程进程的运行数据）运行所导致的异常缓慢;

- [fastcgi_finish_request()](https://www.php.net/manual/zh/function.fastcgi-finish-request.php) - 特殊功能：用于在请求完成和刷新数据后，继续在后台执行耗时的工作（录入视频转换、统计处理等）；

- 动态／静态子进程产生；

- 基本 SAPI 运行状态信息（类似Apache的 mod_status）；

- 基于 php.ini 的配置文件。

### 安装&启动

#### 源码编译安装

编译PHP时，设置--enable-fpm配置选项来激活fpm支持。

以下为 FPM 编译的具体配置参数（全部为可选参数）：

- *--with-fpm-user* - 设置 FPM 运行的用户身份（默认 - nobody）
- *--with-fpm-group* - 设置 FPM 运行时的用户组（默认 - nobody）
- *--with-fpm-systemd* - 启用 systemd 集成 (默认 - no)
- *--with-fpm-acl* - 使用POSIX 访问控制列表 (默认 - no) 5.6.5版本起有效

启动PHP

// todo

#### macOS上使用Homebrew安装

首先，我们先安装homebrew/services和homebrew/dupes来方便地管理macOS服务的启动、退出、重启。

> 在macOS中，服务本身存储在 `.plist` 文件中（即 property list），这些文件的位置一般在 `~/Library/LaunchAgents` 或 `/Library/LaunchAgents`。可以使用 `launchctl load $PATH_TO_LIST` 和 `unload $PATH_TO_LIST` 命令来加载/卸载plist相对应的服务。加载就是允许这个程序开机执行，卸载反之。
>
> 比如安装mysql后，开机启动mysql服务
>
> ```
> To have launchd start mysql at login:
>    ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
> Then to load mysql now:
>    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
> Or, if you don't want/need launchctl, you can just run:
>    mysql.server start
> ```
>
> 如果按上面的说明操作的话，未免太麻烦了，而且也很难记住 plist 的位置。还好 Homebrew 提供了一个易用的接口来管理 plist，然后你就不用再纠结什么 `ln`，`launchctl`，和 plist 的位置了。

```bash
$ brew tap homebrew/services; brew tap homebrew/dupes
```

然后，使用Homebrew安装php71，并支持fpm，安装完成后再显示php、php-fpm的版本号。

```bash
$ brew install php71 --with-fpm --without-apache --with-mysql; php -v; php-fpm -v
```

启动PHP

```bash
$ brew services start php71
```

### 配置

FPM 使用类似 php.ini 语法的 php-fpm.conf 和进程池配置文件。使用brew安装php71后，php-fpm的配置文件应该在`/usr/local/etc/php/7.1/php-fpm.d/www.conf`

#### php-fpm.conf 全局配置段

`pid` [string](https://www.php.net/manual/zh/language.types.string.php)

PID 文件的位置。默认为空。

`error_log` [string](https://www.php.net/manual/zh/language.types.string.php)

错误日志的位置。默认：*#INSTALL_PREFIX#/log/php-fpm.log*。 如果设置为 "syslog"，日志将不会写入本地文件，而是发送到 syslogd。

`log_level` [string](https://www.php.net/manual/zh/language.types.string.php)

错误级别。可用级别为：alert（必须立即处理），error（错误情况），warning（警告情况），notice（一般重要信息），debug（调试信息）。默认：notice。

more see 👉🏻 [PHP: 配置 - Manual](https://www.php.net/manual/zh/install.fpm.configuration.php)

#### 运行配置区段

在FPM中，可以使用不同的设置来运行多个进程池。 这些设置可以针对每个进程池单独设置。

- `listen` [string](https://www.php.net/manual/zh/language.types.string.php)

  设置接受 FastCGI 请求的地址。可用格式为：'ip:port'，'port'，'/path/to/unix/socket'。每个进程池都需要设置。

- `listen.backlog` [int](https://www.php.net/manual/zh/language.types.integer.php)

  设置 listen(2) 的 backlog 最大值。“-1”表示无限制。默认值：-1。

- `listen.allowed_clients` [string](https://www.php.net/manual/zh/language.types.string.php)

  设置允许连接到 FastCGI 的服务器 IPV4 地址。等同于 PHP FastCGI (5.2.2+) 中的 FCGI_WEB_SERVER_ADDRS 环境变量。仅对 TCP 监听起作用。每个地址是用逗号分隔，如果没有设置或者为空，则允许任何服务器请求连接。默认值：any。 PHP 5.5.20 和 5.6.4起，开始支持 IPv6 地址。

- `listen.owner` [string](https://www.php.net/manual/zh/language.types.string.php)

  如果使用了 Unix 套接字，表示它的权限。在 Linux 中必须设置读/写权限，以便用于 WEB 服务器连接。 在很多 BSD 派生的系统中可以忽略权限允许自由连接。 默认值：运行所使用的用户和组，权限为 0660。

- `listen.group` [string](https://www.php.net/manual/zh/language.types.string.php)

  参见 *listen.owner*。

- `listen.mode` [string](https://www.php.net/manual/zh/language.types.string.php)

  参见 *listen.owner*。

  more see 👉🏻 [PHP: 配置 - Manual](https://www.php.net/manual/zh/install.fpm.configuration.php)