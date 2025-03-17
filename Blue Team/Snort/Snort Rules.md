**IDS rules**
For this lab, the scanning engine is Snort version 2.9.7.0 GRE (Build 149).

A standard rule is broken down as follows:
- [action]
- [protocol]
- [ip address] – source
- [port number] – source
- [direction options]
- [ip address] – destination
- [port number] – destination
- [general options]
- [detection options]

alert tcp 10.10.10.0/24 any -> 192.168.0.0/24 443 (msg: “Test Rule”; content: “This is some content”; sid: 5000001; rev: 1;)

**Actions**

The ‘actions’ define which action is taken by the detection engine if a packet matches the rule. There are several options. The most common are as follows:
- ***alert*** generates an alert and logs the packet
- ***log*** logs the packet
- ***pass*** ignores the packet
- ***drop*** blocks and logs the packet
- ***reject*** blocks the packet, logs it and then sends a TCP Reset or ICMP Port Unreachable

**Protocol**

The second component is the protocol. This determines the type of traffic you're looking for, and there are currently four valid options:
- ***TCP***
- ***UDP***
- ***ICMP***
- ***IP***

**IP address**

This is the first IP address that must be matched. The engine understands the following syntax for IP addressing:
- ***any*** – a wildcard for any IP address
- ***10.10.10.23*** – any single valid IP address
- ***10.10.10.0/24*** – CIDR notation for block ranges
- ***!192.168.0.1/24*** – prefixing this field with an exclamation mark means ‘NOT’
- ***[192.168.1.1,192.168.1.2,192.168.1.3]*** – comma-separated lists can use the previous syntax

**Port**

This field is a port number. The engine understands the following syntax:
- ***any*** – a wildcard for any port
- ***443*** – any single port number
- ***1:1024*** – port range

**Direction Options**

This sets the direction of the match and is important when creating rules. The valid options are as shown:
- ***<>*** bidirectional
- ***->*** unidirectional
This is followed by another set of IP addresses and port numbers. Standard notation considers the IP and port on the left-hand side of the direction arrow as the Source, and the IP address and port on the direction arrow’s right-hand side as the Destination.
