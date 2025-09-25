**Forwarded from [Phan Phong](https://t.me/PhanTanPhong)**

no ip dhcp conflict logging
ip dhcp excluded-address 192.168.1.10 192.168.1.20
!
ip dhcp pool 1
 utilization mark high 80 log
 utilization mark low 60 log
 network 192.168.1.0 255.255.255.0
 domain-name cisco.com
 dns-server 8.8.8.8 1.1.1.1
 netbios-name-server 192.168.1.1
 default-router 192.168.1.1
 lease 5
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