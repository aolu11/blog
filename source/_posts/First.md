title: OS X El Captain 升级之路
date: 2015-10-03 12:50:11
tags: Mac
categories: 数码
---
El Captain 终于出来了，可以用上期待已久的 Split View 了，将以前很多编辑器的功能在 OS 层面标准化，越来越多的软件都会支持，赞一个

不过升级后也遇到一些问题：

<!--more-->

第一个比较囧的事情是，DNS 管理又回到了 10.9- 的方式了。。这玩意改来改去的，真是捉急
###### 清 dns cache

```bash
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
say flushed
```
<br />第二个比较囧的事情，El Captain 新加了一层 Rootless Security

[WWDC: Security and Your Apps](https://developer.apple.com/videos/play/wwdc2015-706/)

很多系统级别的目录/文件都被 Lock 起来了（打了 restricted flag），如果你试图向 /usr/bin 以及 /System 等系统级目录写入或者修改，就会报错 `Operation not permitted`，嗯，即使你用了 sudo，即使你是 admin
###### 解决办法有二：

1.直接禁用该机制，重启进入 recovery 模式，然后 bash：

```bash
csrutil disable
```
<br />2.保留该安全机制，不要碰这些 Apple 想为你守护的地方，如果要建 bin 等，用 /usr/local/bin，这个没被锁

还有就是一些软件还需要兼容，QQ 就有黑框 bug，=。=

![QQ黑框bug](http://7xn8l1.com1.z0.glb.clouddn.com/qq_black_bar_bug.png)
