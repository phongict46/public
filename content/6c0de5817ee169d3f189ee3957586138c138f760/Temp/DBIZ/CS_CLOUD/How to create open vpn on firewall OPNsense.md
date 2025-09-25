
**Explain:**
- IP firewall local login: [https://172.16.3.1](https://172.16.3.1/ui/core/dashboard)
- IP public: 103.98.160.23
- OPNsense: Open source firewall run on freebsd os, OPNsense is a powerful, customizable, and open-source software that turns a standard computer into a sophisticated network security and routing device. It acts as a vigilant doorman, filtering traffic, protecting against threats, and giving you unparalleled control over your network, all while being free and highly adaptable.
- VPN: connect to private network cloud ClearSky
- Host external: laptop devices Dbiz staff connet to server vm running on cloud ClearSky
- VM: virtual machine run services or apps
![[Pasted image 20250527125354.png]]
**I.** Create new account vpn.
![[Pasted image 20250526175259.png]]
**II.** Create certificate for new user vpn.
![[Pasted image 20250526180111.png]]

**III.** Export information include user and certificate to file configure and send to user.
![[Pasted image 20250526180339.png]]

##### **How to change certificate expire**

**I.** Login account manage, choose User fill account name example "phongnt" , choose figure the same with number 3 describe in image  for search certificate by user.
![[Pasted image 20250527102937.png]]

**II.** You can show information vaild date.
![[Pasted image 20250527103838.png]]

If expire you can create new certificate with option Clone certificate form expire, any information will keep correct only time valid change to new time effective. 
![[Pasted image 20250527103944.png]]
**III.** Delete certificate expire and move to next step.
![[Pasted image 20250527104704.png]]

**IV.** Export information include user and certificate to file configure and send to user.
![[Pasted image 20250526180339.png]]

##### **[[How to add openvpn file on macos]]**
