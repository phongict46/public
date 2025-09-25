**Reference:**

[Link](https://dev.to/brownian77/setting-up-a-local-development-environment-with-docker-on-mac-and-windows-peb)

- ### Setting Up on Mac

Step 1: Install Docker Desktop

1. Download Docker Desktop for Mac from the [official Docker website](https://docs.docker.com/get-docker).
2. Double-click the downloaded `.dmg` file to open the installer.
3. Drag the Docker icon to the Applications folder to install Docker Desktop.
4. Open Docker Desktop from the Applications folder.

Step 2: Pull Ubuntu Image

Open Terminal and run the following command to pull the Ubuntu image from Docker Hub:  

```bash
docker pull ubuntu
```

Step 3: Run Ubuntu Container

Run the following command to start a Docker container based on the Ubuntu image:  

```bash
docker run -it --name my-ubuntu-container ubuntu
```

Step 4: Restarting the Container

To restart the container named "my-ubuntu-container" later, use the following command:  

```bash
docker start my-ubuntu-container
```

Step 5: Login linux container [[$ Comman in terminal macos & linux.]]

Step 6: install libraries nessecseries for linux container
```bash
apt update
apt upgrade
apt install sudo
sudo apt install net-tools
apt install iproute2 -y "install control interface network"
apt install wget
apt install systemctl
apt install coreutils "install chown help change ownership file or directories"
apt install nano
sudo apt install inetutils-ping "install ping services"
sudo apt install openssh-client
sudo apt install openssh-server
mkdir /var/run/sshd && chmod 0755 /var/run/sshd "create sshd directories for sshd servies"
sudo apt _install_ sysvbanner "install banner services"
apt install ufw "install firewall"
```