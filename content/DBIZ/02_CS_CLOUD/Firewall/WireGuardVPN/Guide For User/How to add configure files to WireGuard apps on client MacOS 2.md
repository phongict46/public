### How to add configure files to WireGuard apps on client MacOS
#### Link Download Apps: [link](https://www.wireguard.com/install/) 
#### Explain: 
- OpenVPN is an application used to establish a connection from the user's computer to the storage of virtual machines running software and databases for the PRO(Production) environment in ClearSky Cloud.

#### Step by step:

- **I.**  Find the email information account WireGuard VPN System Administrator sends to you > copy all information in config file column for next step. ![[Pasted image 20250701101833.png]]

- **II.**  Open WireGuard apps > choose **Manage Tunnels** > choose <mark style="background: #FF5582A6;">1</mark> **Add Tunnels** > <mark style="background: #FF5582A6;">2</mark> **Add Empty Tunnels**.  ![[Pasted image 20250701103613.png]]

- **III.** Paste all information already copy in the past (**step I**), to configure the box in the WireGuard app. Double check information with source and remember to put a suggested name for new tunnels, click **Save** for the next step.  ![[Pasted image 20250701104331.png]]

- **IV.** Check connection status, if you see some information the same with the image below. You are connect to success VPN.  ![[Pasted image 20250701105446.png]]

- **V.** Implementation ping or telnet for test connect to vm on CLearSky Pro Cloud. ![[Pasted image 20250701105938.png]]
**The end.**
