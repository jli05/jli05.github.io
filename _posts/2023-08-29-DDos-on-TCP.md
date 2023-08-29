---
title: DDoS attack on TCP
layout: post
---

The Distributed Denial-of-Service (DDos) attack could be transported by various Internet procotols, such as ICMP, TCP, UDP, etc.

A simple measure to mitigate the attack on TCP could be to set an overall cap on the inbound and outbound TCP connections respectively, and drop any more once a cap is reached, by using the `iptables` utility,

```sh
iptables -A INPUT -p tcp -m connlimit --connlimit-above 10000 --connlimit-mask 0 -j DROP
iptables -A OUTPUT -p tcp -m connlimit --connlimit-above 10000 --connlimit-mask 0 -j DROP
```

Depending on your Linux installation, the `iptables` command may be installed from the `iptables` or `iptables-legacy` software package.
