# How to create WireGuard VPN

## **Mind Map:** ![[Pasted image 20250630175252.png]]
## **Explain:** 
- IP firewall local login: [https://172.20.149.1](https://172.20.149.1/ui/core/dashboard)
- IP public: 103.98.160.46/24
- OPNsense: Open source firewall run on freebsd os, OPNsense is a powerful, customizable, and open-source software that turns a standard computer into a sophisticated network security and routing device. It acts as a vigilant doorman, filtering traffic, protecting against threats, and giving you unparalleled control over your network, all while being free and highly adaptable.
- VPN: connect to private network cloud ClearSky PRO(Production).
- Host external: laptop devices DBIZ staff connect to server vm running on cloud ClearSky PRO
- VMs: virtual machine running services or apps.

## Video link action: ![[Screen Recording 2025-07-01 at 05.52.38.mov]]
## **Step By Step:**

- **I.**  Create new account VPN.
  ![[Pasted image 20250701060805.png]]
  
- **II.** Fill config information necessary for new user VPN.
  ![[Pasted image 20250701061726.png]]

- **III:** Copy configure information and save to directory on OneDrive individual or SharePoint Team System, or every where you want. In this case i save in my Sharepoint. 
  ![[Pasted image 20250701062106.png]]

- **IV.**  Stick option **storage and generate next** with this option you can save new users to tap **Peer** (remember copy configure information before press it). Choose **Apply** for the next step.  ![[Pasted image 20250701091129.png]]

- **V.** Check the new user already created and add to **Instance** > choose **Save** > **Apply** .![[Pasted image 20250701092458.png]]

- **VI.** Check connection if success we will see stick green in the first column and traffic data send and receive will change each time.  ![[Pasted image 20250701094433.png]]
**The end.**