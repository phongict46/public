#####  **Backup config:**

``` bash
	no ip dhcp conflict logging "*intercept create conflict logging*"
	ip dhcp excluded-address 192.168.1.10 192.168.1.20 "exclude range ip address not assign for dhcp client automatic"
	!
	ip dhcp pool 1 "Create new pool for dhcp server name '1' "
	utilization mark high 80 log *"monitor pool dhcp server, if utilization percentage threshold 80 will create log notification show for administration"*
	utilization mark low 60 log *"monitor pool dhcp server, if utilization percentage below 80 will create log notification show for administration"*
	network 192.168.1.0 255.255.255.0
	domain-name cisco.com
	dns-server 8.8.8.8 1.1.1.1
	netbios-name-server 192.168.1.1
	default-router 192.168.1.1
	lease 5 *"time for lease ip address will request again before 50 percentage of 5 day(note cisco guide)*
	!
	ip dhcp pool 2
	network 192.168.0.0 255.255.255.0
	domain-name cisco.vn
	dns-server 8.8.4.4
	default-router 192.168.0.1
	!
	!
	ip dhcp snooping vlan 1-2
	no ip dhcp snooping information option
	ip dhcp snooping database tftp://192.168.1.2/databasebinding
	ip dhcp snooping
	interface GigabitEthernet0/0
	switchport trunk allowed vlan 1,2
	switchport trunk encapsulation dot1q
	switchport mode trunk
	negotiation auto
	ip dhcp snooping trust
	!
	interface GigabitEthernet0/1
	switchport access vlan 2
	negotiation auto
	ip dhcp snooping trust
	interface GigabitEthernet1/2
	switchport access vlan 2
	negotiation auto
	ip dhcp snooping trust
	interface Vlan1
	ip address 192.168.1.1 255.255.255.0
	!
	interface Vlan2
	ip address 192.168.0.1 255.255.255.0
	 
```


