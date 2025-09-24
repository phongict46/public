---
Status: true
New field: 2025-06-22T21:54
Stage: Source
---


# **Case Study:** 
- Check storage using with oracle linux os command. 
```bash
df -h
```
![[Pasted image 20250623111041.png]]

- How to fix error "Failed to set locale, defaulting to C.UTF-8"
  ![[Pasted image 20250708164416.png]]
	- Use command **nano ~/.bashrc** and update some code below.
	
	  ```bash
	## US English ##
	export LANG=en_US.UTF-8
	export LANGUAGE=en_US.UTF-8
	export LC_COLLATE=C
	export LC_CTYPE=en_US.UTF-8  
	  ```

	- Now you can use [yum command](https://www.cyberciti.biz/faq/rhel-centos-fedora-linux-yum-command-howto/ "How to use the yum command on Linux (CentOS/RHEL)") without any issues:
	```bash
	sudo yum update  
	sudo yum search nload  
	sudo install nload
	```

- How to delete history command.
  ```bash
  history -c "clear all command in file ~.bash_history"
  history -w "save after clear"
  history "test again, sure any command already delete"
	```

- How to find some command in file ~.bash_history.
  ```bash
  history | grep openvpn "find command with name openvpn"
  history -d 123 "delete line 123 include name openvpn in file ~.bash_history"
	```