**Summary:**

Refer link setup: https://www.youtube.com/watch?v=wrlukx-QYRw&t=10s

- VM Ubuntu server install on proxmox vm [[UbuntuLinux]]
	- enable ssh on ubuntu server, advantage for copy and paste command form internet for setup
- Install docker on ubuntu server [link](https://docs.docker.com/engine/install/ubuntu/) 
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
```
- Install portainer on ubuntu server [link](https://docs.portainer.io/start/install-ce/server/docker/linux)
```bash
docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.4
```
- Connect to portainer with web browser https://172.16.18.90:9443 and create user name and password for begin login to manage page

##### **How to connect ssh to container from Ubuntu Server running Docker**
- Install Openssh_server on container with [[$ Comman in terminal macos & linux.]]
![[Pasted image 20241227232830.png]]

##### **How to add new port (22222:22 for ssh container run in Docker/Ubuntu_Server/Proxmox_VE from Macbook same network lan with Proxmox_VE) for container with method commit container already created and running in docker**
Refer: [link](https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-to-an-existing-docker-container)

```bash

docker stop nginx-proxy-manager "stop running container (https://docs.docker.com/engine/reference/commandline/stop/) "

docker commit nginx-proxy-manager nginx-proxy-manager-new "commit the container (https://docs.docker.com/engine/reference/commandline/commit/) "

docker run -p 22222:22 -p 80:80 -p 81:81 -p 443:443 -td nginx-proxy-manager-new "re-[run]  from the commited image (https://docs.docker.com/engine/reference/commandline/run/)"

docker run -p 223:22 -p 85:80 -p 86:81 -p 446:443 -td nginx-proxy-manager-second "you can create new container running same services with port setup manual and sure port manual not us for other services (loadbalance services on docker/container). note: you must commit image container befor create new container"  
```
![[Pasted image 20250101233934.png]]

- Configure Openssh-Server on container (How to Install and Enable SSH with Password Authentication on Ubuntu[[$ Comman in terminal macos & linux.]])
- Test connect ssh with port 22222 on Macbook in VS Code
```bash
sudo service ssh start "start sshd services on container if recive alert (ssh: connect to host 172.16.18.92 port 22222: Connection refused)"
```
![[Pasted image 20250101235510.png]]