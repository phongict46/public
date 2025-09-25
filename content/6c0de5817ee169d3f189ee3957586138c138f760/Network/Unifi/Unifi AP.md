#####  **[To remove the UniFi Controller software from a Mac completly:](https://community.ui.com/questions/How-do-I-completely-remove-Unifi-Software-from-Mac-to-start-a-brand-new-network/e10c1fbd-e6b2-48ed-aae3-f874e8869961)**

1. backup the UniFi Controller
2. delete the UniFi app
3. delete the folder: ~/Library/Application\ Support/UniFi/ "<mark style="background: #FFF3A3A6;">command + shift+g</mark> and paste path for open"
4. Install the UniFi app again

##### **[How to install unifi controller on Docker with compose file via Ubuntu Server 22.04 via Proxmox VM ](https://pimylifeup.com/unifi-docker/)**

![[Pasted image 20241223231945.png]]

- Compose file
  ```bash
  services:
  unifi:
    user: unifi
    image: ghcr.io/jacobalberty/unifi-docker
    container_name: unifi-controller
    restart: unless-stopped
    ports:
      - "8080:8080"
      - "8443:8443"
      - "3478:3478/udp"
      - "10001:10001/udp"
    environment:
      TZ: "<TIMEZONE>"
    volumes:
      - ./data:/unifi
```

![[Screenshot 2024-12-23 at 23.22.14.png]]

##### **[How to adopt access point with controller unifi install on Docker/Ubuntu Server](https://unifi.paulproxmox.duckdns.org/manage/default/dashboard)**

Refer: [link](https://hub.docker.com/r/linuxserver/unifi-controller)

- For Unifi to adopt other devices, e.g. an Access Point, it is required to change the inform IP address. Because Unifi runs inside Docker by default it uses an IP address not accessible by other devices. To change this go to Settings > System > Advanced and set the Inform Host to a hostname or IP address accessible by your devices. Additionally the checkbox "Override" has to be checked, so that devices can connect to the controller during adoption (devices use the inform-endpoint during adoption).

**Please note, Unifi change the location of this option every few releases so if it's not where it says, search for "Inform" or "Inform Host" in the settings.**

In order to manually adopt a device take these steps:

```bash
ssh ubnt@$AP-IP
set-inform http://$address:8080/inform
```

The default device password is `ubnt`. `$address` is the IP address of the host you are running this container on and `$AP-IP` is the Access Point IP address.

When using a Security Gateway (router) it could be that network connected devices are unable to obtain an ip address. This can be fixed by setting "DHCP Gateway IP", under Settings > Networks > network_name, to a correct (and accessable) ip address.
![[Screenshot 2024-12-24 at 11.24.41.png]]
