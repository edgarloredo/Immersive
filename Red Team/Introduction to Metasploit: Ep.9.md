We are going to start with the Reconnaissance by scanning the target IP by running nmap however it looks the device is down however nmap is trying to validate if the device is up and running, as we know it is running we can skip "Host Discovery" by using -Pn.

```nmap
sudo nmap -T5 -v -n -Pn <IP>
```

![Image Description](Uploads/Initial%20Scan.png)

![Image Description](Uploads/Second%20Scan.png)

Once we have identified the open ports, we can perform an scan over those ports trying to find more information like versions and potential vulnerabilities.

```nmap
sudo nmap -sC -sV  -p3389,8009,9090 -Pn <IP>
```

![Image Description](Uploads/targeted.png)
