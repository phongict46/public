**Refer:** [link](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi)
**note:** all traffic will thought adgurad dns with someone methods (server adgurad stop all system not connect to internet):
- Setup dns direct on local device
- setup dns on router isp 
## [Install AdGuard Home](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi#install)

[](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi#install-adguard-home)

Go to [AdGuard Home page](https://github.com/AdguardTeam/AdGuardHome#installation) and download binaries for Macos & Raspberry Pi:

```shell
cd
wget 'https://static.adguard.com/adguardhome/release/AdGuardHome_darwin_amd64.zip'
tar -f AdGuardHome_darwin_amd64.zip -x -v
```

(Replace `armv6` with the ARM version that is best supported by your Pi.)

Note: with macos need move directory Adguradhome to  cd /Application/ befor run comman sudo ./AdGuardHome -s install 

That command unpacks the necessary data into a new directory called `AdGuardHome`. Run this command to install AdGuard Home as a service:

```shell
cd ./AdGuardHome/
sudo ./AdGuardHome -s install
```

Here are the other commands you might need to control the service:
Note: with macos need run comman below in `cd /Application/Adguradhome/ `

- `sudo ./AdGuardHome -s uninstall`: uninstall the AdGuard Home service.
    
- `sudo ./AdGuardHome -s start`: start the service.
    
- `sudo ./AdGuardHome -s stop`: stop the service.
    
- `sudo ./AdGuardHome -s restart`: restart the service.
    
- `sudo ./AdGuardHome -s status`: show the current service status.
    

## [Check the filtering](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi#check)

[](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi#check-the-filtering)

You can verify that it's working properly by running this on your Macos & Pi:

```shell
host doubleclick.net 127.0.0.1
```

If everything works correctly, you will get this output:

```shell
Using domain server:
Name: 127.0.0.1
Address: 127.0.0.1#53
Aliases:

Host doubleclick.net not found: 3(NXDOMAIN)
```

## [Configure your devices](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi#devices)

[](https://github.com/AdguardTeam/AdGuardHome/wiki/Raspberry-Pi#configure-your-devices)

Once it is confirmed that AdGuard Home works on our Macos & Raspberry Pi, you can use it on other computers in your network by changing their system DNS settings to use the Pi's IP address.

Go to the “Setup Guide” page in the web interface and follow the instructions.

![[Pasted image 20241127224600.png]]
![[Pasted image 20241127224639.png]]
![[Pasted image 20241127224723.png]]
![[Pasted image 20241127224804.png]]