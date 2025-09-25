# [Smart usage of Obsidian (iPhone, Mac, Git, & as a team)](https://github.com/minguking/Smart_usage_of_obsidian)

### Before start,

[](https://github.com/minguking/Smart_usage_of_obsidian#before-start)

1. you have to download `Obsidian Git` for your Mac Obsidian app from Community plugins. [For more info](https://github.com/denolehov/obsidian-git)
    - Create a git `repository` to upload your local Obsidian works.
    - Now, your local Obsidian works will automatically commit and push as you set.
2. you have to download Obsidian iOS app for your apple device and if you've never used this app, please follow below

> open the app and create a vault **enabling Store iCloud** so that it can automatically create an `Obsidian` folder inside iCloud.

  

Your Obsidian is now syncing with Git. **But you want to sync it with your iPhone as well.**

### Let's start

[](https://github.com/minguking/Smart_usage_of_obsidian#lets-start)

Now, what we're going to do is to clone `Obsidian repository` that you've created.  
We're not just cloning into your local Mac directory but into your iCloud -> Obsidian folder

Using terminal, direct to iCloud folder -> Obsidian isn't pretty easy.  
So, to make it easier, open terminal and type in terminal command below:

```bash
$ cd ~
$ ln -s Library/Mobile\ Documents/iCloud~md~obsidian/Documents/ Obsidian
$ cd Obsidian
```

This command creates the links for the files/folders

now, we can direct to iCloud -> Obsidian folder very easily by just typing `cd Obsidian` only.  
  
You can now git clone into the obsidian folder and enjoy git - iCloud synchronous environment.  
Once cloning is completed, open your iOS Obsidian app, find cloned folder/files and check beautifully synced between devices.  
  

### More

[](https://github.com/minguking/Smart_usage_of_obsidian#more)

If you set your Obsidian Git plugin `Pull updates on startup` turned on, your iPhone will show you an error that is failed to pull/push once you open the app. That's because you didn't login to your Git on your iOS Obsidian app.  

Inside of your Obsidian app -> Settings -> Obsidian Git -> Authentication/Commit Author enter your username on GitHub, type in your password. Restart your Obsidian, it will work perfectly.

### Fix:

- **Option 1:** Can use "<mark style="background: #FFF3A3A6;">[Access Token](https://github.com/settings/tokens)</mark>" setup in **github** for authenticator in plugin <mark style="background: #FFF3A3A6;">Git</mark> on **Obsidian**.
![[Pasted image 20241018150115.png]]
- **Option 2:** [[$ Rcovery Data Source]].  ( *unadvantage*: lost of time for sync **Vault** with **Icloud** )
