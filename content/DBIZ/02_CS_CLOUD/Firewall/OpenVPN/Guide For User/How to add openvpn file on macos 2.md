---
tags:
  - OpenVPN
due: 2025-06-22T21:54
stage: Done
status: true
image: ""
---

**Description:** 
OpenVPN is an application used to establish a connection from the user's computer to the storage of virtual machines running software and databases for the DEV environment in ClearSky Cloud.
- Download and install openvpn : [link](https://openvpn.net/client/)
- How to install openvpn : [link](https://openvpn.net/connect-docs/macos-installation-guide.html)
![[Pasted image 20250530113831.png]]

****

**Add the configuration file provided by the System Administrator sent to you via email or contact Zalo.**

**I.** Open the installed OpenVPN application on macOS, select manual settings to the file containing the connection information to the server via OpenVPN.
![[Pasted image 20250530115143.png|300]]

![[Pasted image 20250530115319.png|300]]
![[Pasted image 20250530115547.png]]

**II.** If the information in the openvpn configuration file is correct, you will see the server IP information and the provided account information.
![[Pasted image 20250530120246.png|500]]

**III.** Select the connection to initiate the OpenVPN connection process from the client to the server. If you see a green tick notification as shown in the image below, you have successfully connected to the VPN.
![[Pasted image 20250530120405.png|500]]

**IV.** Perform a ping or telnet test to check the user's computer connection to the internal network Cloud Clear Sky (DEV environment). If the response returns as shown below, you have successfully established a connection.

Ping command
```bash
ping 172.16.3.3 
```
![[Pasted image 20250530140917.png]]

Telnet command
```bash
telnet 172.16.3.3 5601
```
![[Pasted image 20250530141919.png]]
The end.

[[How to setting OpenVPN auto start when turn off or shutdown OS]]
