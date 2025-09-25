**<mark style="background: #BBFABBA6;">Module 1: Network Basic (Foundation)</mark>**
- **Model TCP/IP - OSI:**
	+ Isolate the incident or evaluate the incident to come up with the correct treatment plan.
	+ Overall perspective of all IT fields.

**OSI 7 layer model vs Tcp/ip 4 layer model:**  

![[osi vs tcp ip.png|300]]

____________________________________
- Hub n port    = 1 Broadcast Domain, 1 Collision Domain 
- Switch n port = 1 Broadcast Domain, n Collision Domain
- Router n port = n Broadcast Domain, n Collision Domain
- ARP resolves IP to MAC: throughout the entire process, the IP remains unchanged, only the MAC changes in each segment.

**<mark style="background: #BBFABBA6;">Module 2: Switching</mark>**
1. **<mark style="background: #D2B3FFA6;">VLAN, Trunking:</mark>**
	- a. VLAN	
		- The switch can only be plugged in and used, it cannot be configured and only has 1 VLAN
		- Switches with VLAN configuration have 4096 - 2 = 4094 VLANs (switchport access vlan vlan-id)
		```bash
		Switch(config)#switchport mode access
		Switch(config)#switchport access vlan vlan-id
		```

	- b. Trunking
		- To conduct a VLAN, the two switches must access 1 VLAN at both ends (10 VLANs require 10 cables to conduct 10 VLANs).
		- Trunking was born to conduct 4094 VLANs on 1 wire (only costs 1 cable for 4094 VLANs).
		- Trunking: ISL (Cisco), Dot1q (Standard)
		```bash
		Switch(config)#switchport trunk encapsulation dot1q
		Switch(config)#switchport mode trunk
		```
	
	- c. VTP: 
		- VLAN synchronization on Cisco switches
	  ```bash
		Switch(config)#vtp domain abc.com
		Switch(config)#vtp mode server/client 
		```

		- VTP has 3 modes: Server (Create VLAN, Sync VLAN), Transparent (Create VLAN, Not Sync VLAN), Client (Not Create VLAN, Sync VLAN).
	
	- d. Sub-Interface on Router (Router communicate with Switch port Trunk):
		 - assign vlan (layer 2) to router port (layer 3)
		```bash
		Router(config)#interface f0/0.10
		Router(config)#encapsulation dot1q 10
		Router(config)#ip address 172.16.10.1 255.255.255.0
		```

2. **<mark style="background: #D2B3FFA6;">CDP - LLDP:</mark>** Used to view device information
	```bash
	   Router(config)#cdp run
	   Router(config)#lldp run
	   Router(config)#show cdp neighbor
	```

3. **<mark style="background: #D2B3FFA6;">DHCP:</mark>** Automatic IP allocation feature for a certain VLAN.
	- --> The first condition for DHCP to work is that Routing is completed.
	- --> For DHCP Relay Agent, go to the correct Gateway (where the DHCP Server configures the Gateway parameters) to point to the DHCP Server. (ip helpder-address "IP-DHCP-Server")

4. **<mark style="background: #D2B3FFA6;">Interface VLAN</mark>**: Usually acts as a Gateway for the entire network.
    - 2-layer model: Interface-vlan is configured at Switch-Core
	- 3-layer model: Interface-vlan is usually configured at Switch-Distribution (can be configured on Switch-Core)

5. **<mark style="background: #D2B3FFA6;">STP</mark>:** Anti-loop in Layer 2 environment.
	- When the Switch is connected in a closed loop, a loop phenomenon will occur. _Solution:_ Temporarily block any port so that the connection is no longer a closed loop.

	Step 1: 
	-  Vote Root Switch
	-  Priority: lowest is best (n +/- 4096 apart, default priority Switch = 32768)
	-  MAC: lowest is best. --> All ports of Root Switch are DP (Designated Port)
	
	Step 2:
	-  Vote Root Port
	- Calculated by Cost: Cost is calculated from the Root Switch to the remaining ports, the lowest Cost is the best (Root Port - RP).
	
	Step 3: Block port (worst port) - Block all VLANs on this segment.
	- Cost: lowest is best
	- Priority of Sender-ID: lowest is best
	- MAC: lowest is best.

	Port status:
	1. From normal port to port Forwarding: 30s (Listening ->(15s) Learning ->(15s) Forwarding)
	2. From port Block to port Forwarding: 50s (Blocking ->(20s) Listening ->(15s) Learning ->(15s) Forwarding)
	
	- STP portfast: Used to make the Access port converge faster (only affects the Access port, does not affect the trunk port)
		- Go to each port access interface to configure portfast
		```bash
		Switch(config-if)#spanning-tree portfast
		```
	
		- Configure portfast in config mode (apply on all access ports, except trunk port)
		```bash
		Switch(config)#spanning-tree portfast default.
		```
		- Per-Vlan-STP (just Cisco support): PVSTP+ Each VLAN will have its own STP

6. **<mark style="background: #D2B3FFA6;">Increase redundancy (High Redundancy):</mark>**
	- a. Cable: Etherchannel - port channel
		- Cisco: PAgP (desirable - auto)
		- Standard: LaCP (active - passive)
		- no protocol: on - off
	
	- b. Switch: stackwise
		- Expansion: Switches do not necessarily have to be the same in terms of equipment, configuration, and port number
		- Backup: Switches must be the same in terms of device, configuration and port number.
	
	- c. Preventive Layer3 (Switch Layer3 / Router) - FHRP:
		- Cisco: HSRP Only supports 2 devices ( 1 Active - 1 Standby)
		- Standard: VRRP Supports up to about 16 devices (1 Active - the rest are Passive)
		- _Solution_: Generate 1 virtual GW representing 2 Routers (of which only Active Router is active).

7. **<mark style="background: #D2B3FFA6;">Security Layer2</mark>** (originates right from the company)
	- a. MAC table attack: overflow the Switch's MAC table (When the Switch overflows the MAC, it will become a Hub)
		Target:
		- Convert Switch into Hub to capture information.
		- Causes the Switch to be Down (destructive attack).
		- _Solution_: Limit the port speed, and use port Security to limit the number of MACs on a port.

		 Port Security:
		1. MAC learning: default 1 MAC when port Security is enabled
		    - Static: Assign the MAC address directly to the port
		    - Dynamic (default): Learn MAC, when reset, learn again -> lose MAC address when Switch resets or clears MAC table.
		    - Sticky: After learning the MAC, it will be saved forever (except when Admin deletes the MAC), combining learning MAC Dynamically and saving MAC Static.
		
		2. Impact: Protect, Restrict, Shutdown
			- Protect: Does not send any information to the administrator, does not affect the port.
			- Restrict: Does not affect the port, but does send a warning to the administrator.
			- Shutdown: impact the port (err-disable: if you want to enable it again, shut down the port first and no shutdown again), and send a warning to the administrator.
			
			- Default of port Security:
			  ```bash
			  Switch(config)#switchport port-security
			  ```
			- Maximum number of MACs: 1
			- Learn MAC: Dynamic
			- Action: Shutdown

	- b. VLAN Hopping:
	  -  Transform into Trunk port and use VLAN Native to detect user/Server information in other VLANs. Trunk ------ Access = N/A. _Solution:_ configure all unused ports as Access ports and shutdown unused ports.
	- c. Change STP: 
	  - Attach a SW with the lowest MAC and Priority to the network port (will become the Root Switch), making all traffic in the network go through this Switch (to capture user information). -> Affects network access and slows down the network. _Solutoin:_ Bật BPDU Guard trên port Access (Không nhận BPDU từ port ACcess)
	
	- d. DHCP snooping - ARP spoofing: 
	  - impersonate DHCP or a server to capture user information. --> _Solution:_ Enable dhcp snooping on Access port to prevent DHCP spoofing (prohibit setting static IP on dhcp snooping configuration ports - violation will result in Err-disable). --> Trusted port trunk to receive DHCP from uplink, not receive DHCP from downlink.

**<mark style="background: #BBFABBA6;">Module3: Routing</mark>**

1. **<mark style="background: #D2B3FFA6;">Static Route:</mark>** 
   - Routing is completely proactive at the discretion of the administrator.
	- --> Easy to configure for small and medium sized models 
	- --> Easy to handle problems (due to complete initiative according to the administrator's wishes)
	
	- --> Difficult configuration for large network models.
	- --> Static Route is trashed (there are Routings that are no longer available but have not been deleted) but have not dared to delete them because I don't know if it will affect other parts or not.
	- Fix:- Always re-mark the static route statement.
	
2. **<mark style="background: #D2B3FFA6;">Dynamic Route: OSPF</mark>**
	- a. Router-id: 
	  - IP Represents the Router when establishing neighbors between Routers participating in OSPF (selects the largest IP among the interfaces to be the router-id when not configuring the router-id).
	- b. DR-BDR: 
	  - In the Broadcast MultiAccess segment, there will be DR and BDR elections
		- DR: undertakes to receive all LSA from the DR-OTHER (224.0.0.5) and return routing information to the DR-OTHER (224.0.0.6)
		- BDR: only receive LSA from the DR-OTHER (224.0.0.5) and only respond when DR suffers Down.
		- DR Only valid in 1 network segment, so configuring DR and BDR is to go to the interface to adjust Priority.
		- Priority Default = 1
		- Priority = 0 (cannot participate in DR voting)
		- Priority = 255 (greatest value)
		- Router(config-if)#ip ospf priority 255 (definitely DR)
		- Router(config-if)#ip ospf priority 0  (cannot participate in the election DR)
	
	- c. Point-to-Point: 
	  - In a peer-to-peer environment, there is no DR/BDR election. In this environment, Routers send LSAs to each other and calculate their own routes and then send each other routing information.
	    ```bash
	    Router(config-if)#ip ospf network point-to-point
	    ```
	
	- d. Cost của OSPF: 
		```bash
			reference BW (10^8)
		Cost = _____________________
			Interface BW
		```
			
		Refernce BW can be changed in Router mode
		
		```bash
		Router(config)#router ospf 1
		Router(config-router)#ospf auto-cost reference-bandwidth 10000 (10GB)
		```
		To change Cost to optimize routing along a certain route, go to the interface to configure.
		```bash
		Router(config-if)#ip ospf cost 10 
		```
3. **<mark style="background: #D2B3FFA6;">Access Control List (ACL):</mark>**
	- Used to filter traffic (Filter) or classify traffic (Classification) in the network according to the administrator's wishes.
	- a. Impact of ACL: Deny and Permit
	- b. Form of ACL: 
		- Standard: Can only define a single source IP (default destination IP is ANY) --> often used for subnet classification, VLAN classification (Classification)
		- Extended: Definition includes: protocol (ip/icmp/tcp/udp), source IP, destination IP, port-ID/ping/time-exceeded/... --> traffic (Filter)
		
		For example: write ACL to do the following:
		- VLAN10: can access the web and ping (the rest cannot be accessed).
		- VLAN20: have access to all services.
		- VLAN30: prohibit web access and traceroute.
		172.16.VLAN-ID.0/24
		```bash
		Router(config)#ip access-list extended Local_LAN
		Router(config-acl)#permit tcp   172.16.10.0 0.0.0.255 any eq www
		Router(config-acl)#permit icmp 172.16.10.0  0.0.0.255 any 
		Router(config-acl)#deny   ip   172.16.10.0  0.0.0.255 any
		Router(config-acl)#permit ip   172.16.20.0 0.0.0.255  any
		Router(config-acl)#deny   tcp  172.16.30.0 0.0.0.255  any eq www
		Router(config-acl)#deny   icmp 172.16.30.0 0.0.0.255  any traceroute
		Router(config-acl)#permit ip   172.16.30.0 0.0.0.255  any
		Router(config-acl)#deny ip any any (implicit deny - HIDDEN)
		```
4.  **<mark style="background: #D2B3FFA6;">Network Address Translation (NAT):</mark>** 
   - convert IP/port-A to IP/port-B
	- Source NAT: if >1 then define = ACL
	- Destination NAT: if >1 then define = POOL
		- a. Static NAT: 1 -> 1
			- Source:       n IP-A = 1 -> not defined ACL
			- Destination:  n IP-B = 1 -> not defined Pool
		```bash
		Router(config)#ip nat inside source static 172.16.1.1 100.0.0.1
		```
		- b. Dynamic NAT: n IP-A -> n IP-B
			- Source:      n IP-A > 1 -> define ACL
			- Destination: n IP-B > 1 -> define Pool
			172.16.1.1 - 172.16.1.12 to IP 100.0.0.2 - 100.0.0.13
				```bash
				Router(config)#ip access-list standard NAT-1.1-1.12
				Router(config-acl)#permit 172.16.1.0 0.0.0.15
				Router(config-acl)#ip nat pool MY_POOL 100.0.0.2 100.0.0.13 subnet-mask 255.255.255.240
				Router(config-acl)#ip nat pool MY_POOL 100.0.0.2 100.0.0.13 prefix-length 28
				Router(config)#ip nat inside source list NAT-1.1-1.12 MY_POOL 
				```
		- c. NAT overload: n IP-A -> 1 IP-B
			- Source:       n IP-A > 1 -> define ACL
			- Destination:  n IP-B = 1 -> not defined Pool
	
				- 172.16.1.0/24 NAT go out with interface g0/0 connecting to the network
				- 172.16.2.0/24 NAT go out with the remaining Public IP 100.0.0.14
		```bash
		Router(config)#ip access-list standard Internet_1.0
		Router(config-acl)#permit 172.16.1.0 0.0.0.255
		Router(config)#ip nat inside source list Internet_1.0 interface g0/0 overload
		Router(config)#ip access-list standard Internet_2.0
		Router(config-acl)#permit 172.16.1.0 0.0.0.255
		Router(config)#ip nat inside source list Internet_2.0 100.0.0.14 
		```
5. **<mark style="background: #D2B3FFA6;">IPv6:</mark>** 
	- On the interface there will be 2 IPv6
	- 1 IPv6 is the Link-Local address automatically configured by Link-Local EUI-64EUI-64 is to split the MAC address, inserting between FF-FE and inverse bit number 7 of the MAC address --> Link-Local only has Only affects 1 Link, so when ping test, it will include Outbound-interface --> 1 Router can use 1 Link-Local address for the entire Interface.
	- The goal of the Link-Local address is to identify Routers with each other.
	- 1 IPv6 configured on the Interface (this IPv6 is used for routing) -> each Interface must have a different IPv6 address.
	
	- Bật IPv6: 
	  ```bash
	  Router(config)#ipv6 unicast routing
	  ```
	After typing this command, the Router's interfaces will generate a Link-Local EUI-64 address
	
	- Static Route:
		```bash
		Router(config)#ipv6 route <destination IPv6> /subnet "ipv6-next-hop"
		```
	
	- Dynamic Route:
		```bash
		R1(config)#ipv6 router ospf 1
		R1(config-router)#router-id 1.1.1.1
	
		R1(config)#int g0/0
		R1(config-if)#ipv6 ospf 1 area 0
	
		R2(config)#ipv6 router ospf 1
		R2(config-router)#router-id 2.2.2.2
	
		R2(config)#int g0/0
		R2(config-if)#ipv6 ospf 1 area 0
		```
6. **<mark style="background: #D2B3FFA6;">VPN (GRE Tunnel):</mark>**
	- Connect the Private environment between sites through the internet environment by creating a Tunnel 1 interface tunnel including:
		- Tunnel Source
		- Tunnel Destination
		- IP của Tunnel --> Different from a normal interface in that it defines tunnel-source and tunnel-destination.
```bash
R1(config)#int tunnel 12
R1(config-if)#tunnel source 100.0.0.1 (or G0/1)
R1(config-if)#tunnel destination 200.0.0.1
R1(config-if)#tunnel mode gre ip (default) -> not to type configuration
R1(config-if)#ip address 192.168.12.1 255.255.255.0
			
R2(config)#int tunnel 12
R2(config-if)#tunnel source 200.0.0.1 (or g0/1)
R2(config-if)#tunnel destination 100.0.0.1
R2(config-if)#tunnel mode gre ip (default) -> not to type configuration
R2(config-if)#ip address 192.168.12.2 255.255.255.0
```
