---
Status: true
New field: 2025-06-22T21:54
Stage: Source
---

**Description:** 
- CSDL: data
- Server: virtual server on customer environment
- Network: all web servers, databases, apps... run on the same network 192.168.21.0/24 and are assigned static IPs.
- Operating system running the database: Oracle Linux
- Database: PostgreSQL
- Old database server: 192.168.21.56
- New database server: 192.168.21.71
- Estimated time: 12 AM - 5 AM April 16, 2025
- Completion time: 1:50 AM April 16, 2025
- Preparation time: 11 PM April 15, 2025
- Start time: 12 AM April 16, 2025
  
![[Pasted image 20250416103034.png | 200x200]]
![[Pasted image 20250416103115.png | 200x200]]

**Content:** 
- Implement the migration of the database system from the old server to the new server (DBIZ).
	- Turn off all running services on PostgreSQL (execution time 10').
	- Turn off all services running on the app server (execution time 10').
	- Data transfer using the copy method with a total capacity of 300GB (execution time 1 hour).
- Perform the IP conversion 192.168.21.56 from the old database server to the new database server (CLIENT IT).
    - Disable the virtual network card on the old database server VM by manual disabling method.
    - Add static IP 192.168.21.56 on the new database server + adjust the /etc/host file from 192.168.21.71 to 192.168.21.56.
- Testing (DBIZ & IT CUSTOMER).
	- Ensure network communication between the servers after the IP change.
		- Turn off the firewall on the servers
		- Check the blocking policy on the firewall device (rule deny icmp protocol).
	- Ensure that the servers and services are turned on.
	- Ensure the application works and generates successful transactions.
	- Ensure the implementation of the scheduling tasks for data replication for the database server.

**Result:**
- All steps to transfer the database from the old server to the new one were carried out without any unexpected issues.
- Switching the IP from the old server to the new server did not result in any unforeseen issues.
- The app server 192.168.21.37 is blocking pings from the other servers on the same network (the issue has been occurring since before the database server migration).
	- Command to disable the firewall feature on Oracle Linux.
	  ```bash
	  sudo firewall-cmd --permanent --add-icmp-block-inversion
	  sudo firewall-cmd --permanent --remove-icmp-block=echo-request
	  sudo firewall-cmd --reload
	  sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT # use for iptable firewall#
	  sudo iptables-save > /etc/sysconfig/iptables # use for iptable firewall#
	
	```

**Question & Answer :**
- When a transaction has occurred, is it not possible to rollback to the old database server? (Mr.Dan sharing).
- Ping success between the app server and the other servers can information be exchanged? (Mr.Minh shared).