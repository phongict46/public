[How to create and configure basic on aws free tier](https://www.youtube.com/watch?v=CjKhQoYeR4Q)

Link login:

- [Console Home](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
- [Billing and Cost Management home](Billing and Cost Management home)
- [Amazon Web Services Sign-In](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
- [Create VM Instances AWS](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:)
- [Dashboard IAM Global](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/home)

##### **How to create ubuntu on aws ec2**

- login web manage aws ec2 [link](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:) , on left panel choose **Instances** -> Launch Instances.
![[Pasted image 20250109210218.png]]

- Choose OS expect (ex: Ubuntu free tier), with OS free tier you can use free cost limit time each OS, take care avoid lot money for Services/OS unexpect. 
![[Pasted image 20250109211041.png]]

- Move down on this page you can choose type for Instance with Free tier, and create **key pair** use for remote **SSH to vm Ubuntu running on Instance**, after create key pair will automatic download save to **Download directory on computer** [[How to ssh to Ubuntu linux with Key-Pair]] -> you can choose many options or advance options for new vm you expect, after that  Choose **Launch Instance** for finish create vm.
![[Pasted image 20250109211330.png]]

- After finish you can see new vm create and running -> choose Connect go to page show information serves login vm.
![[Pasted image 20250109213413.png]]

- Implement tep by tep in image for check and connect vm via ssh (in terminal mac/linux or vs-code ) [[How to ssh to Ubuntu linux with Key-Pair]].
![[Pasted image 20250109213628.png]]

##### **How to install and connect Docker/Portainer on ubuntu aws ec2**

- Install docker/portainer on ubuntu vm you can implement step-by-step  [[How to install Docker and Portainer on Proxmox VM]].
![[Pasted image 20250110103409.png]]

- Connect portainer on vm aws ec2 will different with access local vm, you need open port 9443 on vm remote follow step below same in image.
![[Pasted image 20250110104730.png]]

- You need type https:// for login, the first time login portainer system need restart service/portainer use command `sudo docker <container id> stop/start`
![[Pasted image 20250110104901.png]]

- Login web application manage portainer success after stop/start container in step above.
![[Pasted image 20250110105645.png]]

- Fill information initial for manage portainer.
![[Pasted image 20250110110343.png]]