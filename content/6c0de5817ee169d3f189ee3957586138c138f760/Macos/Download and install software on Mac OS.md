Refer: [Link](https://anjikeesari.com/developertools/software/mac/)
## Install homebrew

This is the first software you may need to install before installing anything on a Mac. For more information, refer to this link: [https://brew.sh/](https://brew.sh/)

Homebrew is similar to Chocolatey in the Windows environment.

homebrew (for Mac users) = cocho (for Windows users)

To use Homebrew, you will need to have a terminal window open and install Homebrew on your system. To install Homebrew, you can copy and paste the following command into the terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

upgrade brew
```bash
brew update
brew --version
```
Install telnet on macos
```bash
brew install telnet
```
syntax connect with telnet
```bash
telnet [hostname/ipaddress] [port number]
```

[How to remove java on macos](https://www.java.com/en/download/help/mac_uninstall_java.html)

 **Uninstall Oracle Java using the Terminal**

**Note:** To uninstall Java, you must have Administrator privileges and execute the remove command either as root or by using the `sudo` tool.

Remove one directory and one file (a symlink), as follows:

1. Click on the **Finder** icon located in your dock
2. Click on the **Utilities** folder
3. Double-click on the **Terminal** icon
4. In the Terminal window **Copy and Paste** the commands below:  
    `sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin`  
    `sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefPane`  
    `sudo rm -fr ~/Library/Application\ Support/Oracle/Java`

Do not attempt to uninstall Java by removing the Java tools from `/usr/bin`. This directory is part of the system software and any changes will be reset by Apple the next time you perform an update of the OS.  
  
Note: After successfully uninstalling Java, you may remove Java Deployment cache using these [instructions](https://www.java.com/deploymentcache/).

**How do I remove the Java Deployment cache after Java is uninstalled?**

```bash
cd Library/Application\ Support/
rm -r Oracle
```

##### [How to enable SSH Server on macos](https://osxdaily.com/2022/07/08/turn-on-ssh-mac/)
Turning on the macOS SSH server is done through system preferences in earlier versions of MacOS:

1. Pull down the  Apple menu and go to “System Preferences”
2. Open the “Sharing” preference panel”
3. Check the box for “Remote Login” to turn on the SSH server on the Mac
4. Optionally but recommended for anyone wanting to create a full shell experience, check the box for “Allow full disk access for remote users”  
    ![Enable SSH Server on Mac with Remote Login](https://cdn.osxdaily.com/wp-content/uploads/2022/04/enable-ssh-mac-remote-login-610x465.jpg)
    
5. The SSH server has started, you’re free to connect to the Mac using any SSH client

You can connect to the Mac with any SSH client from any operating system, whether it’s another Mac with Terminal, Windows PC with PuTTY, Linux with a terminal, iPhone or Android with an SSH app, Android with an SSH app, or anything else with an SSH client.

