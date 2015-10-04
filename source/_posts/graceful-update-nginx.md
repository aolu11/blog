title: 平滑升级 nginx
date: 2017-11-22 11:13:51
tags: nginx
categories: 技术
---

虽说升级 nginx 的需求不多，但要干点啥发现 nginx 编译时候少编了个模块，必须重编的需求，还是很多的... -_-!

由于 nginx 重启非常快，以前并不在意 nginx 的重启方式，升级 nginx 都是暴力杀掉，替换文件后再起来，反正机器多，滚动升级没啥影响，最近因为线上服务器数量很少，就稍微看了下 nginx 的平滑升级，尽量少影响服务：

<!--more-->

1. 首先当然是备份原文件，升级 nginx 说白了就是替换可执行文件 sbin/nginx，先 mv 备份之
2. cp 新的 nginx 到 sbin/ 下
3. ``kill -USR2 `cat log/nginx.pid` ``，nginx 主进程吃这个信号后，会再起一个新的 nginx 主进程，log 下会生成新的 nginx.pid，老的会变成 nginx.pid.oldbin，即此时同时有 2 个版本的 nginx 运行
4. ``kill -WINCH `cat log/nginx.pid.oldbin` ``，平滑杀掉老 worker，这会让老 worker 处理完当前请求后再死
5. 下来看新 nginx 是否稳定，如果稳定，ps 几次，看到没有老 worker 之后，就可以 ``kill -QUIT `cat log/nginx.pid.oldbin` `` 杀掉老 master
6. 如果不稳定，需要回滚，则直接 ``kill -TERM `cat log/nginx.pid` `` 终结新 master，新 master 挂掉后，老 master 会自动开始启动 worker 接受请求（如果没有则 kill -HUP 之）

nginx 还是吃很多种信号的...
