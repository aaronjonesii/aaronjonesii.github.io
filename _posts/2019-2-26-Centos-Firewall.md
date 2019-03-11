---
layout: post
title: centOS 7 Firewall
image: https://user-images.githubusercontent.com/18272237/52909263-0d1a9b80-3254-11e9-9a97-3c818005ff0f.png
---
Block IP Address using `firewall-cmd` on centos


## Block IP Address w/rich rule
```bash
firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.0.11' reject"
```




***
[Reference](https://fedoraproject.org/wiki/Features/FirewalldRichLanguage)
