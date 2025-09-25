![[Pasted image 20250604155854.png]]
The purpose of assigning a static IP address to the OpenVPN account is to avoid the situation of randomly assigned IPs when the host server experiences service interruptions such as: updates, application crashes, server downtime due to power outages, server restarts, etc. .The situation of changing the IP requires the system administrator to update the new IP in the hosts file on the Proxy server in order to access the service through the domain (provided by Mat Bao provider).

To add a static IP for the VPN account in the opnsense firewall environment, the account must have been created and the certificate must be valid.

I. Create account open vpn follow guide [[How to create open vpn on firewall OPNsense]]
![[Pasted image 20250604114813.png]]

II. Determine the Common Name of the account by accessing the VPN tab <mark style="background: #FF5582A6;">1</mark> => OpenVpn => Connection Status <mark style="background: #FF5582A6;">2</mark> => the Common Name <mark style="background: #FF5582A6;">3</mark> field to remember the exact name of the VPN account that needs to be assigned a static IP address.
![[Pasted image 20250604115110.png]]

**III.** To assign a static IP address for the OpenVPN account, follow these steps: select the VPN tab <mark style="background: #FF5582A6;">1</mark> => Client Specific <mark style="background: #FF5582A6;">2</mark> => click on the add new icon <mark style="background: #FF5582A6;">3</mark>.
![[Pasted image 20250604115306.png]]

**IV.** Fill in the information as shown in the picture below, making sure to select the correct VPN server 'SSG DIAL IN (11194 / UDP)', Common Name, IP/Subnet. Proceed to save the information and move on to the check step.
![[Pasted image 20250604125548.png]]

**V.** Account has been successfully assigned an IP.
![[Pasted image 20250604120015.png]]

**VI.** Establish the VPN client connection on the server to ensure that the IP information is assigned correctly.
![[Pasted image 20250604134541.png|300]]
The end.

[[How To Setting Openvpn Auto Start When Turn Off Or Shutdown OS]]
