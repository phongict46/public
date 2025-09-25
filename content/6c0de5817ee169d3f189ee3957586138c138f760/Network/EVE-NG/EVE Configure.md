##### **[Guide configure](https://www.eve-ng.net/images/EVE-COOK-BOOK-1.5.pdf)** 
##### [ Set Static IP Address on EVE-NG: Restart the setup process](https://youtube.com/watch?v=rEcYLuaYBGk)
To do this, follow these steps:

1. Login to the EVE-NG VM (default: username: root / password eve)

2. Remove the saved EVE-NG config file: rm -f/opt/of/.configured

3. Restart the setup process: su -

Go through the setup process (select : static IP address using the spacebar)

5. After the reset process is finished, your EVE-NG will be set up with the new static IP address.
##### **How to setup network for virtual devices**

*note:* in this home lab devices connect internet with static ip address example `172.16.18.88 | 255.255.255.224 | GW: 172.16.18.68 | DNS: 172.16.18.81 (my macbook setup Adgurad DNS control all network) `
- bridge: ability connect to internet
- Management(Cloud0): ability connect to internet
![[Pasted image 20241130170157.png]]

##### **[How to fix cisco IOL auto turn off after start 2 second](https://www.eve-ng.net/index.php/documentation/howtos/howto-add-cisco-iol-ios-on-linux/)**

- Use SFTP on apps RemoteDesktopManager(Macos) copy source IOL from extend HDD/DATA (patch:/Volumes/DATA/DATA/Learning/EVE/Image/CiscoIOL) to EVE-EG on Proxmox vm (patch: /opt/unetlab/addons/iol/bin )

![[Screenshot 2024-12-05 at 17.59.52.png]]

- Type following command to fix permissions:
```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

- IOL images must end with the “.bin” extension and must be executable. EVE-NG Pro has not required to generate iourc license.

License must be stored under the same path. IOU/IOL license is bound to the hostname and domain name of the server. A test should be made to check if IOU/IOL images can run properly.

```bash
cat /opt/unetlab/addons/iol/bin/iourc
```

##### **How to add and configure new network card on EVE-NG connect to VLAN of Proxmox->Firewall Pfsense directive**

- Add network card on vm running **Eve-ng** and configure tag to vlan expect on **Proxmox** connect with vlan similar on **Firewall Pfsense**.
![[Pasted image 20241226103517.png]]

- Configure file manage network interfaces on Eve-ng `sudo nano etc\network\interfaces`
```bash
	iface eth1 inet manual
	auto pnet1
	iface pnet1 inet dhcp "setup recive dhcp client"
	    pre-up ip link set dev eth1 up
	    bridge_ports eth1
	    bridge_stp off
```
![[Pasted image 20241226104945.png]]
![[Pasted image 20241226105140.png]]
- Login any lab create virtual devices connect to network just create, check already recived automatic ip address form vlan on firewall pfsense.
![[Pasted image 20241226105722.png]]![[Pasted image 20241226110011.png]]