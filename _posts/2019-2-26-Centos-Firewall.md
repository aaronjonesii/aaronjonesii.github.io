---
layout: post
title: centOS 7 Firewall
image: https://user-images.githubusercontent.com/18272237/54102823-48664100-43a0-11e9-8c91-0aa2cfd696ad.jpg
---
Block IP Address using `firewall-cmd` on centos


## Block IP Address w/rich rule
```bash
firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.0.11' reject"
```




***
[Reference](https://fedoraproject.org/wiki/Features/FirewalldRichLanguage)
