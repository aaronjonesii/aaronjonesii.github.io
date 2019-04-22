---
layout: post
title: Open Systems Interconnection Model
image: https://user-images.githubusercontent.com/18272237/56102870-ce285f80-5efd-11e9-953f-cc3a027834d4.jpg
---
The `Open Systems Interconnection Model (OSI model)` is a model for a telecommunication of computing system without defining its underlying internal structure and technology. 

#### What is it? `Model for communications functions of telecommunication`

#### What does it do? `Standardizes communication functions of telecommunication`

#### How do you use/implement this? `Follow layers to build/troubleshoot`  

#### Why is it useful? `Standardized model for telecommunication`

## Layers:
Examples of each layer
* `Application` - Websocket, DNS, DHCP, HTTP, FTP
* `Presentation` - ASCII, SSL, TLS, MPEG
* `Session` - Sockets(Session Establishment in TCP), NetBIOS, RPC
* `Transport` - TCP, UDP
* `Network` - IP, ICMP
* `Data Link` - IEEE 802.3(Ethernet), IEEE 802.11a/b/g/n(Ethernet MAC), IEEE 802.1Q(VLAN), Frame relay
* `Physical` - RJ45, 802.11a/b/g/n PHY, USB, Bluetooth


{: .mb-0}
#### `OSI Model`
<em> Layers of the OSI Model. </em>

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
|   |   `Layer` | `Protocol Data Unit(PDU)` |  ` Function`    |
| 7 |   Application |   Data    |   High-level APIs, including resource sharing, remote file access |
| 6 |   Presentation |   Data    |   Translation of data between a networking service and an application; including character encoding, data compression and encryption/decryption |
| 5 |   Session |   Data    |   Session	Managing communication sessions, i.e. continuous exchange of information in the form of multiple back-and-forth transmissions between two nodes |
| 4 |   Transport |   Segment/Datagram    |   Reliable transmission of data segments between points on a network, including segmentation, acknowledgement and multiplexing |
| 3 |   Network |   Packet  |   Structuring and managing a multi-node network, including addressing, routing and traffic control |
| 2 |   Data Link |   Frame   |   Reliable transmission of data frames between two nodes connected by a physical layer |
| 1 |   Physical |   Symbol  |   Transmission and reception of raw bit streams over a physical medium |
{: .w-auto .mb-4 .text-center }

Host layers: 7-4

Media layers: 3-1


***

