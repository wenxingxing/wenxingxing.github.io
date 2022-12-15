---
title: "解决DNS污染"
date: 2022-12-15T15:16:35+08:00
draft: false
---

由于众所周知的原因，最近访问`raw.githubusercontent.com`, `linkedin.com` 等网站总是出现无法连接或者跳转到其他地方的问题。

一开始的解决方案是在浏览器里面使用DOH，但是访问国内的网站速度会明显下降。

一顿Google之后，使用了以下解决方案：

1. OpenClash不要使用`本地DNS劫持`
2. 使用`AdGuardHome`和`SmartDNS`, 拓扑如下:
```
dnsmasq:53 -> AdGuardHome -> SmartDNS
[no cache]                   [with cache]
```

使用以后明显感觉浏览速度快了不少，也能正常访问`linkedin.com`了。