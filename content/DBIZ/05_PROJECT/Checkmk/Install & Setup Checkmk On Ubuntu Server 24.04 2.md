---
Status: true
New field: 2025-06-22T21:54
Stage: Source
---

##### - **Email notification via email gmail with nullmailer**
![[Pasted image 20250504163352.png]]
- install nullmailer on host install checkmk
	- file remotes 
	  ```bash
	  smtp.gmail.com smtp port=465 ssl auth-login user="phongict46@gmail.com" pass="tyqt rgjv xvrf bwhc"
		```
	- file default domain 
	  ```bash
	  gmail.com
		```
	- file mailname
	  ```bash
	  smtp.gmail.com
		``` 
	- file pausetime
	  ```bash
	  180
		```
	- file sendtimeout
	  ```bash
	  3600
		```


![[Pasted image 20250504164521.png]]



