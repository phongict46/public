Refer: [link1](https://www.rapidseedbox.com/blog/nginx-proxy-manager#04) [link2](https://nginxproxymanager.com/guide/)
- Install container Nginx-Proxy-Manager and Source for NGiNX Server Manager [link](https://hub.docker.com/r/jc21/nginx-proxy-manager)
```bash
services: 
  nginx-proxy-manager:
      image: jc21/nginx-proxy-manager
      container_name: nginx-proxy-manager
      ports:
        - 80:80
        - 81:81
        - 443:443
      volumes: 
        - /home/tech/files/docker/volumes/nginx-proxy-manager/data:/data
        - /home/tech/files/docker/volumes/nginx-proxy-manager/letsencrypt:/etc/letsencrypt
      restart: unless-stopped


docker compose up -d "create container form docker compose file.yml"
```
![[Pasted image 20250101231643.png]]

##### **How to configure Nginx translate ip devices in system to domain running with ssl local** 

Refer link setup: https://www.youtube.com/watch?v=acturgE4TmE&t=36s

- Free domain for local Duck DNS [link](https://www.duckdns.org/domains)

*Note:* only use five sub domain for maximum.

![[Screenshot 2024-12-09 at 16.25.02.png]]

- Add SSL Certificate sync to Dynamic Duck DNS, maybe need sync three session for connect success.
![[Pasted image 20241227115133.png]]

- Create Proxy Host for devices/vm in you systems.
![[Pasted image 20241227115031.png]]

##### **How to NAT Nginx from local to internet**

<mark style="background: #FFF3A3A6;">**Note:** everything (vm, devices, services) add to host proxy off Nginx can access via internet with domain name respectively.</mark>

- Login website manage Duckdns update information sub domain and ip public internet 
![[Pasted image 20241225102211.png]]

- Nat services port **(81 and 443)** on vm devices running **Nginx Proxy Mannager** (install with patch: Proxmox->UbuntuServer->Docker->Container Nginx Proxy Manager)
![[Pasted image 20241225102655.png]]
![[Pasted image 20241225102614.png]]

- Add **IP address of vm Nginx** and **Appname** already setup up above 
![[Pasted image 20241225103117.png]]

- Access to Nginx vm on web browser, sure everything success
![[Pasted image 20241225103736.png]]

###### **Case Study:**
- **Accident**: container running Nginx_Proxy_Manager start but not running correct with config setup ssl certificate for ip replay by domain name.
	**-> Fix:**   update and upgrade all packet nescessary for os (ubuntu/debian/readhat/openpsd....). 

