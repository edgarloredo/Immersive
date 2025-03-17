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


**General options**

The remainder of the rule falls within a set of parentheses. These options set the metadata element of the rule. They are known as key:value pairs and are terminated with a semicolon. There are several options here. The standard fields are as follows:

- msg is the message displayed in the log/alert
- sid is a unique numerical identifier that identifies the rule and has several reserved ranges
- rev annotates the revision of a rule
- classtype is used to categorize and group common rules and has many defaults


**Detection options**

This is where rules start to get complicated. The previous fields were all used for setting metadata and understanding traffic flow. This set of key:value pairs instructs the scanning engine to detect specific data within packets.


**Content**

The content keyword forms the core of the rule detection. It can include text, binary data, or a mixture of the two. It is important to keep in mind that content keywords are case-sensitive.

The following examples are valid content values:

- ***content: "This is a string of text"***;
- ***content: "|68 65 6c 6c 6f|"***;
- ***content: "Hello |77 6f | rld"***;
- ***content: !"Not this one"***;

The second example uses the hex values enclosed within a pair of pipes ‘|’.
The third example uses a mix of text and hex characters.
The final option shows the use of ‘!’ to perform a negative match.
Each rule can have multiple content patterns.
The behavior of the content field is adjustable with the use of modifiers. Each content keyword can have several modifiers, and the modifiers will only alter the previous content option.
There are many modifiers that are observable in the Snort manual: http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node29.html. Some common modifiers include:

- ***nocase***

This modifier is used for case-insensitive text strings and will not apply to hex values.

- ***depth***

This modifier defines how far into a packet the match must be located. A depth of six would tell Snort to check the first six bytes of the payload.

- ***offset***

This modifier determines where to start searching for a pattern. An offset of 20 would tell Snort to check for the content after the first 20 bytes of the payload.

- ***http_uri***

This modifier will only match the content field if it appears in the NORMALIZED Request URI field.

- ***http_stat_code***

This modifier restricts the search to the extracted Status Code field from an HTTP server response.

- ***file_data***

This is a special mode that applies to HTTP and SMTP traffic. The Snort engine will search for the content inside HTTP response bodies and decoded MIME attachments in email streams.
