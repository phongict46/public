**Document online :** [link](https://docs.netgate.com/pfsense/en/latest/recipes/freebsd-pkg-repo.html)

| Web page communication     | [link](https://www.reddit.com/r/PFSENSE/comments/1co8f1o/pfsense_iso_download_requires_an_account_and/) |
| -------------------------- | ------------------------------------------------------------------------------------------------------- |
| Download source:           | [link](https://repo.ialab.dsu.edu/pfsense/)                                                             |
| Video training             | [link](https://www.youtube.com/watch?v=mwDv790YoZ0)                                                     |
| Devices Name on proxmox vm | pfsense                                                                                                 |
| IP Wan                     | 172.16.18.94 /27                                                                                        |
| IP Lan                     | 192.168.1.1 /24                                                                                         |
<mark style="background: #FFF3A3A6;">Note: Lan card on pfsense proxmox vm (**enp4s0** create Linux Bridge **vmbr1**) can connect direct to external pc/laptop/sw recived dhcp client provide form pfsense</mark>.

****
![[Pasted image 20241221221345.png]]

setup hardware for pfsense.
![[Pasted image 20241221214011.png]]


##### **How to connect pfsense from ip wan on lab local** 

**note:** with **Lan** setup similar step by step.

On top off main pages choose **Interfaces** croll down the end off pages unstick two options (**Block private networks and loopback addresses** and **Block bogon networks**).

![[Pasted image 20241221223713.png]]

Create rules permit protocol via **Wan** interface.
![[Pasted image 20241221223740.png]]


##### **How to configure https, ssh login on pfsense**

![[Screenshot 2024-12-21 at 22.44.35.png]]![[Pasted image 20241221224727.png]]

##### **[How to configure dns resolver](https://172.16.18.94/services_unbound.php)** 

Note: default already on for this function on fw, you can disable or use access lists deny/permit for network you expect.
![[content/6c0de5817ee169d3f189ee3957586138c138f760/PFSense/images/Screenshot 2024-12-23 at 12.19.13.png]]
![[Pasted image 20241223122746.png]]

##### **[How to setup VLAN on for local LAN network and trunk vlan on Access point Unifi AP AC Pro](https://www.youtube.com/watch?v=SlkAB1nBLB0)**

Create two **Vlan 10,20** from Lan local setup on proxmox vm via pfsense  (enp4s0/vmbr1/re0). 

![[Pasted image 20241224155529.png]]
![[Pasted image 20241224155358.png]]

Create **dhcp server** on two vlan just created, you can setup range suiltabe with spect.
![[Pasted image 20241224155733.png]]

On controller unifi create two vlan **OPT1/20 and OPT2/40** (choose same name with vlan created on fw pfsense).
![[Pasted image 20241224160412.png]]

Create **ssid** same with each vlan tab **Network**.
![[Pasted image 20241224160758.png]]

Connect wifi on two ssid and sure connected to internet and assign correct ip with each vlan.
![[Pasted image 20241224161609.png]]

##### **How to setup Rules on firewall Pfsense**
- Rules access internet (TCP/UDP) and rules ping (ICMP) on Vlan OPT1, OPT2 of Lan Network.
![[Pasted image 20241225225719.png]]

- Rules access all services on Lan Network.
![[Pasted image 20241225230438.png]]

##### **How to setup Vlan sync with interface vlan on Proxmox VM** (sync vlan on firewall Pfsense with vlan on Proxmox VM, use for access vlan of vm running on proxmox vm)

**Link refer:** [[Proxmox 8]]

<mark style="background: #FFF3A3A6;">Key word for find:</mark> `How to setup vlan for Linux Bridge`.

##### **[How to setup PFBlockerNG on fw pfsense ](https://github.com/ahuacate/pfsense-pfblockerng)**

Download and install Packages of PFBlockerng with patch in image flow.
![[anydesk00005.png]]

##### **How to configure VPN with WireGurad Tunnels**
Link Refer:[ youtube1](https://www.youtube.com/watch?v=he3ENpMLMsc&t=1822s) [youtube2](https://www.youtube.com/watch?v=IvGjWndvTk0)

Install wiregurad from package manager.
![[Screenshot 2024-12-26 at 17.19.52.png]]

Permit protocol ipv4 udp running on wan address with port 51820.
![[Screenshot 2024-12-26 at 17.21.12.png]]

Permit any services running thought WireGurad VPN. 
![[Screenshot 2024-12-26 at 17.22.24.png]]

Confirm Interface Group WireGurad  created.
![[Screenshot 2024-12-26 at 17.23.17.png]]

Implement setup Nat Outbound with Hybrid Outbound NAT and Mapping Interface WAN.
![[Screenshot 2024-12-26 at 17.23.49.png]]

Create Tunnels for WireGurad VPN.
![[Screenshot 2024-12-26 at 17.24.46.png]]

Create Peers for devices connect thought Wiregurad VPN.
![[Screenshot 2024-12-26 at 17.24.55.png]]

Enable WireGurad.
![[Screenshot 2024-12-26 at 17.25.04.png]]

Pin status service on home page Firewall PFsense.
![[Screenshot 2024-12-26 at 17.26.18.png]]

Setup Dynamic DNS Clients for WAN connection form VPN. Fill in information connect to Domain Name Service Manager `https://www.duckdns.org/update?domains=paulproxmox&token=7816f51f-10a6-4fe9-ad16-59f1cc0a3c54&ip=%IP%`.
![[Screenshot 2024-12-26 at 17.27.48.png]]
![[Pasted image 20241231104603.png]]
![[Screenshot 2024-12-26 at 17.28.26.png]]

Login Router ISP configure NAT Port two session (double NAT).
![[Screenshot 2024-12-26 at 17.29.08.png]]
![[Screenshot 2024-12-26 at 17.29.16.png]]

Connect to vpn on Macos/Windows and test speed internet.
```bash

**[Interface]**

**PrivateKey** = wBJ4xqKyZ+A1xXn4eKuT/Pd0UvxRzaz/GAjMcNcy0UI= "key auto create on local app device"

**Address** = 10.0.0.3/32 "ip lan vpn provide and allow from config Peers "

**DNS** = 127.0.0.1 "dns server Adgurad_local setup on Macbook_PaulPhan use connect vpn wiregurad"

  

**[Peer]**

**PublicKey** = zhwUs2tauviw8D4z4SPK17UOfzAcAKa2wfCyWOmmz3w= "provide from Tunnels of Wiregurad/Pfsense"

**PresharedKey** = QEm6lyNb6+uXLFZqV1xmWp/I/kuRWNudYu3J9Q12aRI= "Provide from Wiregurad_Peers/pfsense of devices connection"

**AllowedIPs** = 0.0.0.0/0 "all traffic via vpn"

**Endpoint** = paulproxmox.duckdns.org:51820 "Domain name reply for ip WAN (configure on Duckdns services provider free , port 51820 this is port open on router ISP and router pfsense)"

```
![[Pasted image 20241231105825.png]]
![[Screenshot 2024-12-26 at 17.29.33.png]]

Check traffic flow via DNS setup on WireGurad client (can use dns server Adgurad_setup_local,  Cloudflare, Google ), and check Gateway directive to Gateway off Wiregurad_VPN/PFsense/Router_ISP.
![[Screenshot 2024-12-26 at 17.33.15.png]]
