# How to install GUI & VNC Tiger

## How to install GUI:
### Link refer:
- [link](https://www.youtube.com/watch?v=ws1J9nKpAKc)
- [link](https://docs.oracle.com/en/learn/ol-install-vnc/index.html#install-vnc-server-packages-and-set-the-vnc-password)
- Install GNOME desktop environment include command in "Server with GUI".
  ```bash
  sudo dnf group install -y "Server with GUI"

	```

- Set graphical mode as the default login type for user accounts, then reboot the server.
  ```bash
  sudo systemctl set-default graphical

```

- Update all packages to the latest release.
  ```bash
  sudo dnf upgrade -y
```

- (Optional) Disable Wayland in the graphical desktop.
  ```bash
  sudo sed '/^#WaylandEnable/s/^#//g' /etc/gdm/custom.conf
```
## How to install VNC Tiger:
- Command install tiger vnc-server > choose **Y** for continue install.
  ![[Pasted image 20250709111701.png]]
  ```bash
  sudo dnf install -y tigervnc-server tigervnc-server-module
```

- Create a VNC password for the user account you intend to use for remote sessions.
  ```bash
  vncpasswd
  restorecon -RFv $HOME/.vnc "delete configure password VNC in the past"
```

### Configure the VNC Service:
 I.  Append the user account and the X Server display for the VNC service to the `/etc/tigervnc/vncserver.users` file.
 ```bash
 echo ":1=$(whoami)"| sudo tee -a /etc/tigervnc/vncserver.users > /dev/null
```

II. Append the default desktop and screen resolution to the `/etc/tigervnc/vncserver-config-defaults` file.
```bash
printf 'session=gnome\ngeometry=1280x1024' | sudo tee -a /etc/tigervnc/vncserver-config-defaults > /dev/null
```

III. Reload the `systemd` service.
```bash
sudo systemctl daemon-reload
```

IV. Enable and start the VNC server by using X Server display 1.
```bash
sudo systemctl enable --now vncserver@:1.service
```

V. Enable firewall rules.
```bash
sudo firewall-cmd --zone=public --add-service=vnc-server --permanent
sudo firewall-cmd --reload
```

### Test connection VNC form client.
- Configure VNC app on client.
  ![[Pasted image 20250709161111.png]]
  ![[Pasted image 20250709161136.png]]
  ![[Pasted image 20250709161236.png]]

### Case Study:
- Disable auto logout account (because with method connect to VMs running Oracle Linux via ssh key and don't use password login, default account will auto block in period of time if not use). [link](https://www.cyberciti.biz/faq/linux-tmout-shell-autologout-variable/) refer
  ![[Pasted image 20250709174930.png]]
