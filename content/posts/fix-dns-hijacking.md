---
title: "解决DNS污染"
date: 2022-12-15T15:16:35+08:00
draft: false
---

~~由于众所周知的原因，一直以来访问`raw.githubusercontent.com`, `linkedin.com`
等网站总是出现无法连接或者跳转到其他地方的问题。~~

~~一开始的解决方案是在浏览器里面使用DOH，但是访问国内的网站速度会明显下降。~~

~~一顿Google之后，使用了以下解决方案：~~

- 使用`AdGuardHome`和`SmartDNS`:
```
Client 
  -> dnsmasq:53 (转发/上游?)
  -> OpenClash:7874 
  -> AdGuardHome:5553 
  -> smartdns:6053 [with cache]
```

~~这样既可以使用OpenClash的分流，也能使用AGH和SmartDNS的功能。~~

~~使用以后明显感觉浏览速度快了不少，也能正常访问`linkedin.com`了。~~

~~目前出现了一个问题, `*.cn`全部都会解析成`127.0.0.1`...~~, 尝试清理本地cache: `resolvectl flush-caches`

~~使用了一段时间，还是会间歇性地跳到`linkedin.com`, 有空试试`mosdns`~~, try Domain Address Rules:

```
~~nameserver /linkedin.com/fq_dns~~
```


目前解决方案：
DNS:
```
Clent
    -> dnsmasq:53
    -> OpenClash:7874
    -> AdGuardHome:5553 [with cache]
        -> doh-0
        -> doh-1
        -> doh-2
```

Clash: redir-host wth rules

虽然是redir-host，但由于AdgHome有Cache，所以效果和fake-ip差不多其实。
就算国内doh的dns返回了污染后的ip，但是这个ip只在mapping的时候被用到，所以无所谓，（只要没有两个域名被分到一个ip上）。
