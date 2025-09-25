##### 1. **Wireshare: Test flow packet data running when connect vpn Wireguard Tunnel use dns Adgurad on local macbook (127.0.0.1 / 172.16.18.81)**
	- Full traffic data via vpn
	- Slipt traffic data via vpn "local traffic access thought vpn Wiregurad, internet traffic access thought dns setup on devices provide maybe form modem/dns_services(adgurad local, 1.1.1.1, 8.8.8.8)"

##### 2. Install Adgurad on Docker 
```bash
sudo docker run --name adguardhome\
    --restart unless-stopped\
    -v /my/own/workdir:/opt/adguardhome/work\
    -v /my/own/confdir:/opt/adguardhome/conf\
    -p 54:53/tcp -p 53:53/udp\
    -p 67:67/tcp -p 67:67/udp\
    -p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp\
    -p 853:853/tcp\
    -p 784:784/udp -p 853:853/udp -p 8853:8853/udp\
    -p 5443:5443/tcp -p 5443:5443/udp\
    -d adguard/adguardhome

```

2. 1/ **Connect to web configure Adgurad but not have information  username/password login**

   <mark style="background: #BBFABBA6;">Resolve:</mark> [how to change password adgurad install on container/ubuntuserver/proxmox](https://hostingcanada.org/htpasswd-generator/)
   - You need to edit the password field in the `AdGuardHome.yaml` file. Use this to generate a new password (you must set it to `BCrypt`): [https://hostingcanada.org/htpasswd-generator/](https://hostingcanada.org/htpasswd-generator/)
   ![[Pasted image 20241230171805.png]]
   Login console of container running Adguradhome, type command 
   ```bash
		su - "login root account"
		cd /
		cd /opt/adguradhome/conf/
		vi Adguradhome.yml
		Paste password hash to file .yml
		i "login write mode in vi"
		esc "exit write in vi"
		:w "save file"
		:q "exit vi"
	```
   ![[Pasted image 20241230171953.png]]
	![[Pasted image 20241230172710.png]]
   
2. 2/ Install new container run web browser **chrome/fire_fox/brave_browser** with same network card Bridge
   2.2.1/ Login Adgruad with **ip card bridge** with port **3000 or 3002** same with configure yml file
##### 3. **Setup KASM_Server  serves for open container running web_browser install on step 2.2**
3. 1/ How to ssh to container install KASM_Server with manual port (2222) , because when connect direct via Portainer/Container Console disadvangeted 
		<mark style="background: #BBFABBA6;">***Resolve:*** connect ssh to container [[How to install Docker and Portainer on Proxmox VM]]</mark>

##### 4. **How to Fix connect container/ubuntuserver/promoxvm with port manual ssh 222  with explain in image below.** 
![[Pasted image 20241227233528.png]]

<mark style="background: #BBFABBA6;">**Resolve:**</mark> create port manual you expect for container (ex:2222), implementation install openssh_server and step by step follow guide [[$ Comman in terminal macos & linux.]] with key word <mark style="background: #FFF3A3A6;">How to Install and Enable SSH with Password Authentication on Ubuntu</mark>

![[Pasted image 20241230155303.png]]

- Login ssh with port manual (ex:2222) on VS Code
![[Pasted image 20241230161339.png]]
##### 5. **How to connect and apply DHCP_server/Vlan form network of Router/SW in Eve-ng environment to Devices/VM on Proxmox VM**
<mark style="background: #BBFABBA6;">Resolve: 
 Provide dhcp form Router on Eve-ng to vm on Proxmox (network card Bridge Cloud2/pnet2/eth2/vmbr2) and vm windows 10 on Eve-ng (network card Cloud2/pnet2/eth2/vmbr2)</mark>
![[Pasted image 20241228172104.png]]
- Command configure on Router R7
```bash

```

##### 6. **[How to ssh to router on eve-ng with teminal macos](https://gulian.uk/8-steps-to-configure-ssh-on-a-cisco-router-or-switch/)**
- **Note:** 
	- Only connect vpn Wiregurad on vlan 20/40/1 for ssh to router with diagram above 
	![[Pasted image 20241228223025.png]]
	- <mark style="background: #FFF3A3A6;">VS Code not connect ssh to router (thought can access to step authenticator)</mark> ![[Pasted image 20241228223509.png]]
```bash
Router(config) enable secret cisco123 "create password for login Privileged mode"
Router(config) username root secret Ptp@#2024 "create ussername and passoword authenticator thought ssh client"
Router(config) line vty ? "show information of vty"
Router(config-line) line vty 0 4 "setup vty 0 4"
Router(config-line) login local "permit login ssh to local devices(router/sw)" 
Router(config-line) Transport ? "show option transport command"
Router(config-line) Transport input ? "show option input command"
Router(config) Transport input ssh "only us ssh"
Router(config) crypto key generate rsa modulus 1024
Router(config) ip ssh version 2 "choose ssh version"

ssh root@192.168.20.101 -oKexAlgorithms=+diffie-hellman-group14-sha1 -oHostkeyAlgorithms=+ssh-rsa "Test ssh connect with rsa syntax"
```
#####  How to add **ssh Key RSA** to file **know_host**

- RSA key auto create when login ssh to router device.
![[Pasted image 20241228211458.png]]

- Remote ssh to file **~/.ssh/Know_host** in **macos** thought **VS Code**. After type information correct in file **Know_host** press **Command + S** for save and connect again router via ssh client.
```bash
ssh root@192.168.20.101 -oKexAlgorithms=+diffie-hellman-group14-sha1 -oHostkeyAlgorithms=+ssh-rsa
```
![[Pasted image 20241228211323.png]]![[Pasted image 20241228212225.png]]

##### 7. **How to create vlan and add vlan for each devices in network lan**
- devices PaulPhan deny social media (facebook, tiktok, pinterest)
- devices TuyetNguyen deny (tiktok, permit bandwith 20mbps)

##### 8. **How to create pencil write on touchpad mac book PaulPhan with touch gloves**

##### 9. **[How to configure 2FA for GUI access in pfSense](https://www.comparitech.com/blog/vpn-privacy/pfsense-two-factor-authentication/)**
**Resolve:**![[Pasted image 20241230114924.png]]

##### 10. [How to configure Wiregurad on DebianOS and configure client vpn on ios](https://wireguard.how/client/ios/)

- **[Link master clearly:](https://www.zenarmor.com/docs/network-security-tutorials/how-to-install-wireguard-on-freebsd)**
```text
# define the local WireGuard interface (client)
[Interface]

# contents of wg-private-client.key
PrivateKey = oBkgA+KZU6mWY5p7d0PEWxnYkihBw9TmHZXEYnQkz3g=

# the IP address of this client on the WireGuard network
Address=10.0.0.4/32

# define the remote WireGuard interface (server)
[Peer]

# from `sudo wg show wg0 public-key`
PublicKey = zhwUs2tauviw8D4z4SPK17UOfzAcAKa2wfCyWOmmz3w=

# the IP address of the server on the WireGuard network 
AllowedIPs = 10.0.0.1/24

# public IP address and port of the WireGuard server
Endpoint = paulproxmox.duckdns.org:51820
```

```text
# define the remote WireGuard interface (client)
[Peer]

# contents of wg-public-client.key
PublicKey = fAuWUuOxLEk3N6YQF16SPGQKMXQh1PD5KTNRHFPLU0c=

# the IP address of the client on the WireGuard network
AllowedIPs = 10.0.0.4/32
```

<mark style="background: #BBFABBA6;">Resolve: install wiregurad on ubuntu run on container/ubuntuserver OR directive on Proxmox VM</mark>

##### 11. **How to use adgurad on container is proxy for network in container/other network in container/ vm on proxmox**
- how to configure dns on container [link](https://forums.docker.com/t/how-to-config-the-dns-for-a-container/52395)
![[Pasted image 20241230181822.png]]

##### [12. **How to install Qemu_Guest_Agent on Ubuntu](https://forum.proxmox.com/threads/solved-guest-agent-not-running.149049/)**

**Note**: Enable Qemu_Guest_Agent on VM form Option->Qemu_Guest_Agent->tick choose enable before install and configure Qemu.
- Follow these steps This is the daemon used to exchange data between the guest and the host.
```bash
apt-get install qemu-guest-agent "command installs it."
systemctl start qemu-guest-agent  "starts the daemon"
systemctl enable qemu-guest-agent "enables it at VM boot"
```
![[Pasted image 20241231222416.png]]

##### **13.[ How to install Qemu_Guest_Agent on Pfsense run FreeBSD OS on Proxmox VE](https://www.reddit.com/r/PFSENSE/comments/18l8ibz/easily_install_qemuguestagent_on_pfsenseproxmox/?rdt=44111)**

- Other method on Github : [link](https://github.com/Weehooey/pfSense-scripts)

1- Take a snapshot on Proxmox for the VM (good to have)  
2- Update VM settings in Proxmox portal - Options --> Qemu... - screenshot attached  
3- Reboot (Stop then Start) the pfSense VM for the change to take effect

[![r/PFSENSE - pfsense-proxmox-settings](https://preview.redd.it/easily-install-qemu-guest-agent-on-pfsense-proxmox-step-by-v0-gc1qyctox17c1.png?width=473&format=png&auto=webp&s=64282a90f7f505d332a0890e91c9c8dbf791f9c7)](https://preview.redd.it/easily-install-qemu-guest-agent-on-pfsense-proxmox-step-by-v0-gc1qyctox17c1.png?width=473&format=png&auto=webp&s=64282a90f7f505d332a0890e91c9c8dbf791f9c7 "Image from r/PFSENSE - pfsense-proxmox-settings")

pfsense-proxmox-settings

4- From UI or CLI, update pfS to the latest version (currently 2.7.2)  
5- login to pfSense console - hit 8 to get to CLI - paste

`pkg install -y qemu-guest-agent`

`cat > /etc/rc.conf.local << EOF`

`qemu_guest_agent_enable="YES"`

`qemu_guest_agent_flags="-d -v -l /var/log/qemu-ga.log"`

`#virtio_console_load="YES"`

`EOF`

`cat > /usr/local/etc/rc.d/qemu-agent.sh << EOF`

`#!/bin/sh`

`sleep 5`

`service qemu-guest-agent start`

`EOF`

`chmod +x /usr/local/etc/rc.d/qemu-agent.sh`  
`service qemu-guest-agent start`

6- Validate from Proxmox summary screen to see IPs - screenshot attached

[![r/PFSENSE - Easily install qemu-guest-agent on pfSense/Proxmox - Step-by-Step](https://preview.redd.it/easily-install-qemu-guest-agent-on-pfsense-proxmox-step-by-v0-oltkx5xuz17c1.png?width=489&format=png&auto=webp&s=b37b1d28da723ec8cf42f7ac22f0489472e3e060)](https://preview.redd.it/easily-install-qemu-guest-agent-on-pfsense-proxmox-step-by-v0-oltkx5xuz17c1.png?width=489&format=png&auto=webp&s=b37b1d28da723ec8cf42f7ac22f0489472e3e060 "Image from r/PFSENSE - Easily install qemu-guest-agent on pfSense/Proxmox - Step-by-Step")

Note: Little trick - If you connecting to a remote pfSense over VPN that you can't reboot or would be locked out to start again - you can run this from the Proxmox shell to make sure it starts in one line.

`qm stop VMID && qm start VMID`.

##### 14. [**How to install Debian 9.2 running on Proxmox VE**](https://www.youtube.com/watch?v=gGsgl0t8py0)
- Configure necessary in initial
```bash
ip a "show information ip address"
apt install sudo "install sudo"
sudo apt update "update debian"
sudo apt upgruade "upgruade debian"
nano /etc/network/interfaces "access file configure network of debian"
iface ens18 inet static "access file interfaces configure static ip for debian"
        address 172.16.18.89
        netsmask 255.255.255.224
        gateway 172.16.18.68
        namserver 1.1.1.1
 sudo nano /etc/resolv.conf "access file resolv.conf configure dns for debian access internet"
	nameserver 1.1.1.1
	
apt-get install ca-certificates "add certificate help debian access some website/ftp_file from internet (ex: Qemu_Guest_Agent)"
```

##### 15. **[How to install Adgurad on Debian 9.2](https://github.com/adguardteam/adguardhome/wiki/VPS)**
```bash
wget https://static.adguard.com/adguardhome/release/AdGuardHome_linux_amd64.tar.gz
tar xvf AdGuardHome_linux_amd64.tar.gz

cd AdGuardHome
pwd
sudo ./AdGuardHome -s install
AdGuardHome -s uninstall "uninstalls the AdGuard Home service"
AdGuardHome -s start "starts the service"
AdGuardHome -s stop "stops the service"
AdGuardHome -s restart "restarts the service"
AdGuardHome -s status "shows the current service status"
host doubleclick.net 127.0.0.1 "If everything works correctly, you will get this output:
Using domain server:
Name: 127.0.0.1
Address: 127.0.0.1#53
Aliases:

Host doubleclick.net not found: 3(NXDOMAIN)"
```

##### 16. **How to install Qemu_Guest_agent on Debian/Promoc_VE**

<mark style="background: #BBFABBA6;">Resolve</mark>

Refer: [link](https://forum.proxmox.com/threads/install-qemu-guest-agent-debian-7-wheezy.71437/)
```bash
apt-cache policy qemu-guest-agent "show version available"
apt install qemu-guest-agent=<versionnumberyouwanthere> 
apt --fix-broken install "use command if accident occurs"
```
![[Pasted image 20250103172919.png]]
##### 17. **How to setup two container Nginx-Proxy-Manager running balance** 

-  why not configure on two Nginx-Proxy-Manager new create but can connect success to domain : https://npm.paulproxmox.duckdns.org/login, https://ads.paulproxmox.duckdns.org/, https://proxmox.paulproxmox.duckdns.org/
- reslove: no accident because docker remember container old, ability create two container running same port but only container initial effective (ex: nginx-proxy-manager:81:81 running correct, while nginx-proxy-manager:86:81 not access login host after configure)
- create new container with other port (85:80, 86:81, 446:443)from container commit, but only configure 
	- ssl certificate (try on two account of DuckDNS paulphan504.duckdns.org and paulproxmox.duckdns.org) -> success
	- host (portainer.paulphan504.duckdns.org) -> success create and nslookup correct domain and ip public but not action login to services (potainer).
##### 18. **Build addition one Ubuntu_Server/Docker/Portainer&Nginx-proxy-manager test balance**

| UbuntuServerSecond                                                                                                                                                                                   | Information                                           |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| Ip address: static                                                                                                                                                                                   | 172.16.18.79/27                                       |
| Gateway:                                                                                                                                                                                             | 172.16.18.68                                          |
| DNS: 107 (Debian9.2)/AdguradHome                                                                                                                                                                     | 172.16.18.89                                          |
| Username/Password: Local<br>Username/Password: root                                                                                                                                                  | paul/...<br>root/...                                  |
| Landscape account manage: [link](https://landscape.canonical.com/account/a0glnmc7)<br>Password: on dashlane <br>Account ubuntu pro personal: [link](https://ubuntu.com/pro)<br>Password: on dashlane | paul-ubuntu-server-secon<br><br>paulphan504@gmail.com |
| **Container**                                                                                                                                                                                        | **Information**                                       |
| Portainer:<br>Username:<br>Password: on dashlane                                                                                                                                                     | 172.16.18.79:9443<br>admin<br>...<br>                 |
| Nginx-proxy-manager-second<br>Username:<br>Password: on dashlane                                                                                                                                     | 172.16.18.79:81<br>paulphan504@gmail.com              |
- build success Ubuntu_Server with pro license for personal and add computer to [Landscape ](https://landscape.canonical.com/account/a0glnmc7) ![[Pasted image 20250103160421.png]]
- configure ssl on Nginx not success, because accident with information "ack certificate too many time" on two account paulphan504.duckdns.org and paulpromox.duckdns.org. (<mark style="background: #FFF3A3A6;">reconfigure after unlock</mark>)
- **How to create container via VS Code with compose file .yml**
	-  Implement ssh to server/vm running Docker in this lab use Ubuntu_Server
	- Create file docker-compose.yml after fill information same in image below press Compose Up, container will create and add to extention Docker in VS Code.
![[Pasted image 20250104222858.png]]
![[Pasted image 20250104223424.png]]
##### **How to fix new container keep old configure** 
- everything you only need delete directory of volume configure 
![[Pasted image 20250108110204.png]]

##### 19. **How to use Treafik services for home lab**

##### **20. Fix Ubuntu-server-second not upgrade**
- change network card recive dhcp 
```bash
nano /etc/netplan/50....

```
**<mark style="background: #BBFABBA6;">Resolve: because conflict ip occurs </mark>**

##### 21. **Build Ubuntu_Server_Second or Ubuntu_Server became Proxy action with Lan_Network Firewall PFsense.**
- not success because not nat port 81 and 443  for ip provide in vlan firewall pfsense frome modem isp.
##### 22. **How to change keypair for aws ec2 free tier on aws**

##### 23. **How to fix domain on duckdns not register with nginx-proxy-manager**

- Use cloudeflare add domain name register form duckdns.
![[Pasted image 20250108224414.png]]
- Choose Domain need resolve accident, move to tab DNS->Records->tick choose all records and delete.
![[Pasted image 20250108224935.png]]

- Realize register domain to ssl-certificate off nginx-proxy-manager again [[How To Setup And Install FREE Domain and SSL for Local Network Nginx Proxy Manager on Docker]] 

24. Build IDS/IPS on firewall pfsense snord and surikata.

25. [How to fix error](https://stackoverflow.com/questions/70818543/mongo-db-deployment-not-working-in-kubernetes-because-processor-doesnt-have-avx) "sql MongoDB 5.0+ requires a CPU with AVX support, and your current system does not appear to have that! "
- change cpu to **host** configure in proxmox ve manage vm running mongodb 
  ![[Pasted image 20250422085719.png]]