##### [How to setup DHCP server on proxmox provide ip for vm ](https://pve.proxmox.com/wiki/Setup_Simple_Zone_With_SNAT_and_DHCP)
### Installation

In order to use the automatic DHCP feature you need to additionally install dnsmasq. You can do this via the following command:
```bash
apt install dnsmasq
```
Additionally, you should disable the default dnsmasq service:
```bash
systemctl disable --now dnsmasq
```
### Configuration

Navigate to 'Datacenter > SDN > Zones' and create a new Simple zone with an ID of your choice. For activating DHCP, also tick the 'automatic DHCP' option in the advanced settings. As IPAM we select pve, which is available by default in SDN. In order to use the IPAM features described below, you need to use the 'pve' IPAM.

Navigate to the VNet panel to create a new VNet with an ID of your choice (`vnet0` in our example). When creating the VNet, select the zone we created in the previous step.

In the same view, create a Subnet in the previously created VNet. This can be done by selecting the VNet and clicking 'Create' in the Subnet panel on the right-hand side. Enter a local Subnet of your choice (in our example `10.0.0.0/24`). You also need to define an IP for the gateway, otherwise DHCP will not work (`10.0.0.1` in our example). Tick the 'SNAT' option in order to enable SNAT for this VNet.

To use DHCP we also need to create a DHCP range for this Subnet. This can be done by switching to the Tab 'DHCP Ranges' in the Subnet creation dialog. I picked `10.0.0.50` and `10.0.0.200` as my start and end addresses for the DHCP range.

- [![Zone Configuration](https://pve.proxmox.com/mediawiki/images/thumb/8/84/Simple_zone_dhcp.png/120px-Simple_zone_dhcp.png)](https://pve.proxmox.com/wiki/File:Simple_zone_dhcp.png "Zone Configuration")
    
    Zone Configuration
    
 - [![VNet Configuration](https://pve.proxmox.com/mediawiki/images/thumb/2/25/Vnet_configuration.png/120px-Vnet_configuration.png)](https://pve.proxmox.com/wiki/File:Vnet_configuration.png "VNet Configuration")
    
    VNet Configuration
    
 - [![Subnet Configuration](https://pve.proxmox.com/mediawiki/images/thumb/e/ee/Subnet_configuration.png/120px-Subnet_configuration.png)](https://pve.proxmox.com/wiki/File:Subnet_configuration.png "Subnet Configuration")
    
    Subnet Configuration
    
 - [![DHCP range configuration](https://pve.proxmox.com/mediawiki/images/thumb/0/02/Dhcp_range_configuration.png/120px-Dhcp_range_configuration.png)](https://pve.proxmox.com/wiki/File:Dhcp_range_configuration.png "DHCP range configuration")
    
    DHCP range configuration
    

Now that everything is set up, we need to apply the changes. This can be done by navigating to the SDN panel and clicking 'Apply'. Make sure that the network reload task finishes successfully. If it does complete without any errors or warnings you should be able to use your newly created VNet.

 ##### **Setup static ip address for VM and setup network card follow with image** ![[Pasted image 20241124233235.png]]

##### [Create folder for mount extend hdd or usb for copy data direct form usb to proxmox server](https://thehomelab.wiki/books/promox-ve/page/add-external-usb-storage-to-proxmox)

**Figure out what drive it is**:

Find the drive device ID running this command
```bash
fdisk -l
```
**Create folder for mounting**:

Now we have to make a folder where the drive will be mounted. I used the following folder.
```bash
mkdir /mnt/USB_Data
```
Now we mount the drive to it
```bash
mount /dev/sdb1 /mnt/USB_Data
```

Example: copy iso file from extend hdd storage to iso directory on proxmox 
```bash
root@paulproxmox:/mnt/USB_Data# cp WServer_2022_DC-016.ISO /mnt/pve/EXT/template/iso/ "copy iso image source form extend hdd mount to directory iso off extend storage setup on proxmox "

root@paulproxmox:/mnt/USB_Data# cp WServer_2022_DC-016.ISO /var/lib/vz/template/iso "copy iso image source from extend hdd mount to iso directory on proxmox"
```
##### [How to install 7zip to extract zip/rar file](https://askubuntu.com/questions/348173/how-to-install-7zip-to-extract-rar-files)
To unrar files with... 7zip: 
```bash
sudo apt-get install p7zip-full
7z x some.rar
```
and `x` mean extract obviously.

##### [How to install extension copy&paste for Proxmox VE console](https://gist.github.com/amunchet/4cfaf0274f3d238946f9f8f94fa9ee02)

1. Install Tampermonkey for your browser ([https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en))
2. Open up this script in "Raw" (or go to this url: [https://gist.github.com/amunchet/4cfaf0274f3d238946f9f8f94fa9ee02/raw/0b84970f89e1f282f09b86d46227eda71178c040/noVNCCopyPasteProxmox.user.js](https://gist.github.com/amunchet/4cfaf0274f3d238946f9f8f94fa9ee02/raw/0b84970f89e1f282f09b86d46227eda71178c040/noVNCCopyPasteProxmox.user.js))
3. Click install
4. [Optional] if you have a non-US keyboard layout, you may need to modify the script.
5. Copy some text to your clipboard (highlight some text and Ctrl + C)
6. Open up your Promox console
7. Right click to paste the text into the console (you may need to restart your browser, depending on your tampermonkey installation)

Note: if you use LXC containers, you do not need this script, as you can just copy and paste normally from that terminal.

If you have any other questions, please feel free to ask!

##### [How to import file .OVA to Proxmox VE server](https://www.vinchin.com/vm-backup/proxmox-import-ova-ovf.html)
##### [Refer link 2:](https://www.youtube.com/watch?v=EqGJYU96l0Q)

##### [ How to Import VMDK to Proxmox?](https://vinchin.com/vm-migration/import-vmdk-proxmox.html)

```bash
root@paulproxmox:/mnt/pve/EXT/template/iso/cd# qm importdisk 105 Parrot.qcow2 local-lvm
```
- Double click **Unused Disk** just create choose same with image.  
![[Screenshot 2024-12-12 at 15.58.41.png]]
- Tab **Options** -> **Boot Order** move up disk to first boot. 

 
 ##### **[How to use a Proxmox script to create a VM?](https://www.vinchin.com/vm-tips/proxmox-create-vm.html)**
```bash
qm create <vmid> --name <vm_name> --memory <memory_size> --net0 <model=e1000,bridge=vmbr0> --cores <number_of_cores> --sockets <number_of_sockets> --cpu <cpu_type>
```

```bash
qm create 1233456 --name 12345 --memory 1024 --net0 model=e1000,bridge=vmbr0 --cores 2 --sockets 1 --cpu host
```

vmid: Unique identifier of the VM

<memory_size>: Specify the size of the VM's memory, in MB or GB

<model=e1000,bridge=vmbr0>: Network configuration, which can be modified as needed. Specify the network model of the VM as e1000 and the network bridge to the VM as vmbr0.

<mark style="background: #FFB86CA6;">**How to list all VMID**</mark>

```bash
# cat /etc/pve/.vmlist
```

##### **[The Complete Beginner's Guide to LVM in Linux](https://linuxhandbook.com/lvm-guide/#2-volume-groups)**

##### **[How to config storge in proxmox](https://www.youtube.com/watch?v=HqOGeqT-SCA)**

```bash
fdisk dev/sd_name_of_disk/

command m for help: g (format GPT for partition)

command m for help: w (write config)

```
![[Pasted image 20241129152602.png]]

##### **[Extend Boot Drive Storage In Proxmox (Resize)](https://www.youtube.com/watch?v=SsOLRiN4l9c)**
Refer command script and solution other with many systems very useful: [link](https://github.com/webmentordev/linux-bash-scripts-and-solutions/blob/master/Extend%20Proxmox%20Storage.txt)

##### **[How to copy source with SCP protocal](https://www.veerotech.net/kb/how-to-use-secure-copy-protocolscp-to-transfer-files-securely-on-linux-and-mac/)**
**Quick Steps**

1. 1. Open a terminal window.
    2. Upload files to your VeeroTech Hosting account with this command
        
        **scp -P 22 filename _username@example.com_:~/destination**
        
    3. Enter your password.
    4. Download files from your VeeroTech Hosting account with this command
        
        **scp -P 22 _username@example.com_:~/file destination**
        
    5. Enter your password when prompted, and SCP will download the file to the specified destination directory.

**Fix error when install windows 11 on proxmox: need chosses CPU Processors 2 or more than**
- Allway add TPM State and EFI Disk
- Chooses Hard Disk (IDE or SATA)
![[Pasted image 20241129124248.png]]
**How to fix warning 'neither intel VT-x or AMD-V' on VM run EVE-NG on Proxmox server**
- If warning occurs devices on EVE-NG (SW, Router, Windows) not start (satart and stop immediated)
![[Pasted image 20241129134552.png]]

##### **How to setup start vm auto after turn on proxmox ve**
![[Pasted image 20241211160947.png]]

##### **[How to fix error after remote via ssh to proxmox vm](https://forum.proxmox.com/threads/perl-warning-setting-locale-failed.94218/) '<mark style="background: #FFF3A3A6;">Falling back to a fallback locale ("en US.UTF-8")</mark>'**
![[Screenshot 2024-12-12 at 15.11.35.png]]
```bash
export LC_CTYPE=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

##### **[How to resize partition install linux os and windows os](https://www.system-rescue.org/Download/)**

Refer : [Link youtube](https://www.youtube.com/watch?v=cwsSAzzaXIw)

<mark style="background: #FFF3A3A6;">Note: don't use pci graphic card for vm, error occurs when boot form iso file systemresuce cd </mark>

- Download **Systemrecuce CD** and mount cd drive on vm need resize partition
- Press **ESC** on keyboard while turn on vm for boot to **Bios** system -> turn off by uncheck **secure boot** , save setting with **FN + F10 on macos** -> boot again and choose **boot form cd** choose the first tab **Default** off systemsecuce -> root screen command type **startx** for login screen desktop -> choose application resize partition delete partition among **main partition** and **partition  unlocated**    
- Right click on partition need resize pull to end of partition unlocated for extend partition expect choose apply and finish ( in someone case partition noticed incident check in setting **Hardware** and **Options** off **Boot other** and **Type os**  )

##### **[How to resize partition on Ubuntu Server](https://forum.proxmox.com/threads/resize-ubuntu-vm-disk.117810/)**

![[Pasted image 20241214225531.png]]

##### **How to add Linux Bridge card connect direct to extend devices network card (static ip adress)**
![[Pasted image 20241221222509.png]]

##### **How to setup vlan for Linux Bridge** <mark style="background: #FFF3A3A6;">(sync vlan on firewall Pfsense with vlan on Proxmox VM, use for access vlan of vm running on proxmox vm)</mark>
![[Screenshot 2024-12-25 at 23.28.52.png]]
![[Pasted image 20241225232647.png]]
![[Screenshot 2024-12-26 at 11.11.27.png]]
![[Pasted image 20241226111810.png]]
##### **How to connect Vlan on Proxmox VM to EVE-NG <mark style="background: #FFF3A3A6;">(connect any virtual devices on EVE-NG apply with Vlan and Rules on Firewall PFsense </mark>)**

*Link refer:* [[EVE Configure]]
*Key words for find:* `How to add and configure new network card on EVE-NG connect to VLAN of Proxmox->Firewall Pfsense directive`

##### **[How to fix error io-error on vm proxmox](https://www.reddit.com/r/Proxmox/comments/15snbks/running_ioerror_on_vms/?rdt=60482)**
- **Resolve:** Delete snapshot on vm alert io-error for fix accident 
- **Note:** use someone command for show information proxmox vm and vm on proxmox
```bash
pveversion -v "show information proxmox "
qm config <VMID> "example: qm config 104"

```