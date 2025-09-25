#####  **Ubuntu desktop Information:**

| **Username:**     | paullinuxdesktop@paullinuxdesktop-Standard-PC-i440FX-PIIX-1996 |
| ----------------- | -------------------------------------------------------------- |
| **IP:**           | 172.16.18.90/27                                                |
| **Network Name:** | ens19                                                          |
| **Status:**       | Static ip address setup on file  with information below<br>    |
| DNS               | 1.1.1.1                                                        |

- Directory content file
```bash
cd /etc/netplan/
sudo nano 01-network-manager-all.yaml
nmcli device show ens19 | grep IP4.DNS "show information dns using on ubintu"
ip r | grep default "show information default network card using on ubintu"
```

- **Configure file information:**
![[Screenshot 2024-12-09 at 09.07.14.png]]
       
- ##### Connect remote thought Teamviewer 
```bash
id: 1072 505 988
Pass: 
```

- ##### **Connect remote thought Anydesk**
```bash
id: 1 343 214 487
pass: 
```

- ##### **How to connect Ubuntu Desktop thought teamviewer + anydesk**

<mark style="background: #FF5582A6;">*Note:* <mark style="background: #FF5582A6;">no clock Ubuntu Desktop vm</mark>  only restart, logout if you want connect again with temviewer.</mark>

- connect teamviewer with id and password below 
	- login password on ubuntu desktop
	- open anydesk
- connect anydesk with id and password below 
	- accept connecting for anydesk (login form outsite devices to ubuntu desktop vm on proxmox)
	- <mark style="background: #FFF3A3A6;">close connection teamview on outside devices</mark>

##### **How to shutdown Proxmox server thought remote anydesk from Ubuntu Desktop vm on proxmox** 
- open shell from website manage proxmox vm type comman
```bash
sudo shutdown now
```
- shutdown ubuntu desktop
```bash
sudo shutdown now
```
- close anydesk remote to ubuntu desktop.

------------------------------------------------------------------------

[How to add new device network card and put ip address on ubuntu server](https://ubuntu.com/server/docs/configuring-networks)

- add new network card on vm ubuntu server on proxmox vm
![[Pasted image 20241203222347.png]]

- add ip static for network card
```bash
sudo ip addr add 172.16.18.90/27 dev ens19
```
- apply setup on network card
```bash
sudo netplan apply
```

- show information interface network card
```bash
ip a
```
- turn on network card running on ubuntu server 
```bash
sudo ip link set dev ens19 up
```
- add route gateway for network card
```bash
sudo ip route add default via 172.16.18.68 dev ens19
```
- show ip information specify interface network
```bash
ip addr show dev ens19
```
- refresh dns for network card
```bash
sudo ip addr flush ens19
```
- show ip route
```bash
ip route show
```

How to install and enable ssh server on Ubuntu
```bash
sudo apt install openssh-server
sudo systemctl status ssh
sudo ufw allow ssh 'if have configure firewall in system'
```

##### **Ubuntu Server Information:**

| **Username:**     | paul@linuxserver |
| ----------------- | ---------------- |
| **IP:**           | 172.16.18.92     |
| **Network Name:** | ens18            |
| **DNS:**          | 1.1.1.1<br>      |
##### **[How to setup static ip on ubuntu server with file config](https://krishnendubhowmick.medium.com/how-to-configure-a-static-ip-address-on-ubuntu-20-04-22-04-lts-step-by-step-4a35d6662083)**

1. Copy file config for backup.
```bash
paul@linuxserver:~$ sudo cp /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.copy
```
 2. Edit Netplan configuration.
 ```bash
 paul@linuxserver:~$ sudo nano /etc/netplan/50-cloud-init.yaml
```
![[Screenshot 2024-12-09 at 09.00.27.png]]
3. Apply new ip address
```bash
paul@linuxserver:~$ sudo netplan apply
```
4. Add dns server to file config 

`paul@linuxserver:~$ sudo rm /etc/resolv.conf`

`paul@linuxserver:~$ sudo nano /etc/resolv.conf`

```bash
paul@linuxserver:~$ nameserver 1.1.1.1 'add dns server to file resolv.conf'
```

##### **[How to find one file on linux ubuntu](https://www.tutorialrepublic.com/faq/how-to-find-a-file-by-name-using-command-line-in-ubuntu.php)**

```bash
find /path/to/directory -type f -name filename.ext
```

*ex:* For example, to find the file named "sample.txt" within the `/var/www` you can use:

`find /var/www -type f -name sample.txt `

##### **How to show version with comman**
```bash
sudo lsb_release -a
```

[How to show port open and configure firewall on Ubuntu Server ](https://linuxconfig.org/how-to-check-for-open-ports-on-ubuntu-linux)

```bash
sudo ss -ltnp "show port and services open on ubuntu server with command ss"
sudo nmap localhost "show port and services open on ubuntu server with command nmap"
sudo ufw status verbose "show status action off fw on ubuntu server"
sudo ufw allow 80/tcp "permit port 80/tcp access via fw"
```

##### **[How to show list user using cat command on ubuntu server](https://www.hostinger.com/tutorials/how-to-see-system-users-in-ubuntu-linux-vps/)**

```bash
cat /etc/passwd "show all user on create and save in file passwd"
```

**How to switch login among users in linux**

```bash
su - "login user root"
su - paul "login user paul"
whoami "check user login present"
```

##### [**How to login and change password and disable user root in linux**](https://www.cyberciti.biz/faq/change-root-password-ubuntu-linux/)
```bash
sudo -i "login user root no ask password when login by account administrators/root permission"
passwd "change password user root"
sudo passwd -dl root "disable user root"
sudo passwd --delete --lock root "delete password user root ability login with passwordless"
```

##### **[Understand meaning of information account in linux ubuntu server](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)**
![[Pasted image 20241229130544.png]]
From the above image:

1. **Username**: It is used when user logs in. It should be between 1 and 32 characters in length.
2. **Password**: An x character indicates that encrypted and salted password is stored in [/etc/shadow file](https://www.cyberciti.biz/faq/understanding-etcshadow-file/ "Understanding /etc/shadow file format on Linux"). Please note that you need to use the passwd command to computes the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file.
3. **User ID (UID)**: Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
4. **Group ID (GID)**: The primary group ID (stored in /etc/group file)
5. **User ID Info (GECOS)**: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6. **Home directory**: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7. **Command/shell**: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell. For example, sysadmin can use the nologin shell, which acts as a replacement shell for the user accounts. If shell set to /sbin/nologin and the user tries to log in to the Linux system directly, the /sbin/nologin shell closes the connection

##### **[How to fix ssh root user permission deny on ubuntu server 22.04](https://www.youtube.com/watch?v=XYx9vi0XEi8)**

**Note:** can use this method [[$ Comman in terminal macos & linux.]] with key word (<mark style="background: #FFF3A3A6;">How to Install and Enable SSH with Password Authentication on Ubuntu</mark>)

```bash
paul@linuxserver:/$ sudo nano /etc/ssh/sshd_config.d/mycf.conf "create file mycf.conf on ubuntu_server running ssh_serverfor ssh and type command '**PermitRootLogin yes**' "
paul@linuxserver:/$ sudo systemctl restart ssh "restart services ssh"

ssh root@172.16.18.92 "login ssh on macbook to ubuntu_server with root account and sure success same image below"

```
![[Pasted image 20241229225324.png]]

##### **How to add more interfaces network and disable/enable interface in Ubuntu-Server/Proxmox-VE**
```bash
sudo inconfig -a "show all information interfces"
sudo netplan apply "apply action file configure interfaces in directory /netplan"
sudo ifconfig -s "show status all interfaces"
sudo traceroute cnn.com "show route traffic"
sudo ifconfig <name-interface> down "disable interface you expect (ex: sudo ifconfig ens18 down) "
sudo ifconfig <name-interface> up "enable interface you expect"
sudo netplan apply "apply action file configure interfaces in directory /netplan"
```