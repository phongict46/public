##### **Login via terminal macos:**
- Move to directory include file Key-pair already download when create Key-pair in Instance of aws/ec2  
```bash
cd /User/Paul/Downloads
```
- Copy and paste command include information (name key-pair, account, info vm) When choose connect to vm on Instance web page.
```bash
ssh -i "keyubuntu.pem" ubuntu@ec2-18-209-168-180.compute-1.amazonaws.com
```
![[Pasted image 20250109213628.png]]
![[Pasted image 20250109214522.png]]
- Use command `exit` for terminal login to vm.
##### **Login via VS Code:**
- Navigate to extension **Remote Tunnels** -> choose **SSH Setting** -> open file config ssh.
![[Pasted image 20250109220735.png]]

- Fill information necessary for each vm difference .
	- **Host:** fill any thing you expect (recommend use information same with HostName Instance).
	- **HostName:** this is name provide by Instance delegate for each vm .
	- **IdentityFile:** this is patch save file Key-Pair.
	- **User:** user delegate login to vm.
- After fill all information press **Ctrl+s** for save.
![[Pasted image 20250109220820.png]]

- After save file config with step upper you can see new ssh show in option SSH left hand side -> choose icon `->` for log in  to vm on aws ec2. If everything information correct you can see processing  success same with image show below.
![[Pasted image 20250109221059.png]]
