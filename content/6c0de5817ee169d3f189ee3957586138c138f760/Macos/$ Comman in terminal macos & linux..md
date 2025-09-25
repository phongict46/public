##### [**Show hidden files and folders within Mac terminal**](https://stackoverflow.com/questions/71983948/show-hidden-files-and-folders-within-mac-terminal)

Have you tried using the -a flag for ls?

1. To list all files (including hidden files): `$ ls -a`
2. To list all files in a list with more details: `$ ls -al`
3. Alias (shortcut) for 2. `$ ll`

`ls` itself usually ignores entries starting with '.', and the `-a` or `--all` flag ignores that protection.

Use `ls --help` in your terminal to review all available flags.

##### **[How to CD (move, delete, show) to Your iCloud Drive on a Mac](https://www.wikihow.com/CD-to-iCloud-Drive-on-Mac)**

```bash
cd ~/Library/Mobile\ Documents/
```

![[Pasted image 20241008205715.png]]

##### **Command login ubuntu/linux container with Terminal on macos**.
```bash
docker ps 
docker exec -u 0 -it "INPUT_NAMES_CONTAINER" bash
```
![[Pasted image 20241013120221.png]]


##### **[Show and change permission directory in linux terminal](https://hcc.unl.edu/docs/handling_data/data_storage/linux_file_permissions/#:~:text=Type%20the%20command%20ls%20%2Dl,a%20file%20or%20a%20directory.)**

permission infomation:
- read = 4
- write = 2
- execute = 1

```bash
ls -l
chmod 754 "name directory or file"
```
![[Pasted image 20241013224308.png]]

##### [How To View System Users in Linux on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-view-system-users-in-linux-on-ubuntu)

```bash
less /etc/passwd  "list all user in ubuntu linux"

cut -d : -f 1 /etc/passwd  "list user only name"

less /etc/group  "view all group"

cut -d : -f 1 /etc/group "list group only name"

w "show list user login"

who "show list user login"

sudo whoami "show user running"

groups "show group running"
```

##### [How To Restrict Log In Capabilities of Users on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-restrict-log-in-capabilities-of-users-on-ubuntu)

```bash
less /etc/passwd | grep messagebus "restrict user with namw"
```

##### [How to Install and Enable SSH with Password Authentication on Ubuntu](https://medium.com/@ravidevops2470/how-to-enable-ssh-with-password-authentication-on-ubuntu-22-04-a7cbdf476d8b)
**Note:** can use other method [[UbuntuLinux]] key word (<mark style="background: #FFF3A3A6;">How to fix ssh root user permission deny on ubuntu server 22.04</mark>)

```bash
sudo apt update  
sudo apt install openssh-server "Install OpenSSH Server"
sudo nano /etc/ssh/sshd_config "Edit SSH Configuration, reference image show below "
sudo service restart ssh "or use commad" sudo service ssh restart
sudo service start ssh "or use commad"  sudo service ssh start
passwd "change password login ubuntu container"
ssh username@your_server_ip "check remote login thought ssh with syntax"
```
![[Pasted image 20241014221457.png]]

##### [How to remove file, folder with 

terminal on macos/linux.](https://docs.oracle.com/cd/E19253-01/806-7612/files-20/index.html)

```bash
rmdir folder_name 'remove and empty directory'
rm -r folder_name 'remove directory and all its contents'
rm -rf folder_name 'remove directory and all its contents, not use if unsure'
```

Open List tree directory  and show all infomation.

```bash 
ls -1 -la "you can show ls --help for many"
```

##### **[Install wget service on terminal with brew.](https://www.cyberciti.biz/faq/howto-install-wget-om-mac-os-x-mountain-lion-mavericks-snow-leopard/)**
```bash
brew install wget
```

##### **Open file and applications in terminal.**

```bash
open -a Google\ chrome "open app chrome"
open -a firefox "open app firefox"
cd Downloads
open -a Google\ Chrome phan\ tan-phong-cv.pdf "open file pdf with google chrome"
```

##### **Delete apps on terminal.**
```bash
cd /
cd Applications
sudo rm -r Google\ Docs.app/ "remove google doc on applications, use sudo equal root account"
```

##### **[Install nvim editor.](https://github.com/neovim/neovim/blob/master/INSTALL.md)**
```bash
brew install neovim
```

##### **Shutdown macos with terminal command.**
```bash
sudo shutdown -r now
```

##### **Close apps with terminal.**
```bash
killall firefox "close app firefox"
killall Google\ Chrome "close app chrome"
killall Activity\ Monitor "close service Monitor on macos"
```

##### **Show all process with terminal on mac.**
```bash
top -o cpu "show all services running on you macbook, you can type top --help show more about infomation "
```

##### **[How to change multiple files name with terminal on macos.](https://iboysoft.com/tips/rename-file-macos-terminal.html)**
```bash
for file in *; do                                                                                                
  mv "$file" "${file%.txt}.md"

done
```

##### **[How to delete folder & file error <mark style="background: #FFF3A3A6;">not permission</mark> at terminal:](https://unix.stackexchange.com/questions/555493/permission-denied-from-sudo-rm-r-filedirectory-file-on-macos)**

1. Reboot your Mac to enter recovery mode - see [Mac startup key combinations](https://support.apple.com/HT201255) to find out how to do this on your Mac
2. When in recovery mode, select _Terminal_ from the _Utilities_ menu
3. Enter `csrutil disable` and confirm the warning, then wait until the command has finished
4. Enter `shutdown -r now` to reboot into normal operating mode
5. When in normal mode, again open _Terminal_ and remove the file(s) using `sudo rm` - this time it should succeed
6. Reboot again to enter recovery mode
7. When in recovery mode, again select _Terminal_ from the Utilities menu
8. Enter `csrutil enable` and confirm, then wait until the command has finished
9. Enter `shutdown -r now` to reboot into normal operating mode

When back in normal mode, you may verify that SIP protection has been successfully re-enabled by entering `csrutil status` in _Terminal_.

Please note that is absolutely recommended to immediately re-enable SIP. You should not run your Mac without SIP unless you know exactly what you are doing.

##### **[How to change DNS with terminal on macos](https://cleanbrowsing.org/help/docs/manually-change-dns-on-a-mac-terminal/)**
- Change for wifi card ```bash
```bash
sudo networksetup -setdnsservers Wi-Fi 127.0.0.1
```
- Check dns active
```bash
networksetup -getdnsservers Wi-Fi
nslookup cnn.com
```
- Reset DNS to default form DHCP server
```bash
networksetup -setdnsservers Wi-Fi
```

##### **How to open path in macos:** <mark style="background: #FFF3A3A6;">command + shift + g</mark>
##### [How to set time schedule auto turn on &  turn off](https://support.apple.com/en-vn/guide/mac-help/mchl40376151/13.0/mac/13.0) 
```bash 
sudo pmset repeat wakeorpoweron MTWRFSU 7:00:00 sleep MTWRFSU 23:30:00
```
##### **[ Brew Uninstall Package](https://www.warp.dev/terminus/brew-uninstall-package)**

##### [**How to istall unar with homebrew**](https://superuser.com/questions/52124/how-can-i-extract-rar-files-in-macos)
```bash
brew install unrar # install Unrar on macos #
unrar x <fiel unrar> # unrar file expect ex: unrar x 4U.rar #
```