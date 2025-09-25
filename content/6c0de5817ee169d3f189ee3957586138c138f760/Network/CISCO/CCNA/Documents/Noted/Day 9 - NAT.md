NAT - Network Address Translation
NAT có 3 loại: 
1. Static NAT (Mapping từ 1 IP - 1 IP):
Source: 1 IP		-> không cần định nghĩa Source bằng ACL
Destination: 1 IP	-> không cần định nghĩa Destination bằng Pool
ip nat inside source static <IP-Source> <IP-Destination>
################################################################################################
VD: nat IP 192.168.1.1  khi ra ngoài internet dùng IP 200.0.0.96
B1. Inside: 1 IP	-> không cần định nghĩa Source bằng ACL
B2. Destination: 1 IP	-> không cần định nghĩa Destination bằng Pool
B3: cấu hình NAT static
Router(config)ip nat inside source static 192.168.1.1 200.0.0.96


----------------------------------------------------------------------------------------------------------------
2. Dynamic NAT (Mapping từ n IP - n IP) với n >= 2:
Source: n >= 2 	-> định nghĩa Source bằng ACL
Destination: n >= 1IP -> định nghĩa Destination bằng Pool
B1: tạo ACL cho Source
B2: tạo Pool cho Destination
B3: ip nat inside source list <name-ACL> pool <name-Pool>
################################################################################################
VD: nat IP từ 192.168.1.1 - 4 khi ra ngoài internet dùng IP 200.0.0.96-200.0.0.99
B1. Inside (>=2 IP) -> cấu hình ACL
Router(config)#ip access-list standard MY_LAN 
Router(config-std-acl)#permit 192.168.1.0 0.0.0.7 
B2. Outside (>=1 IP) -> cấu hình Pool
Router(config)#ip nat pool MY_POOL 200.0.0.96 200.0.0.99 prefix-length 29
B3. cấu hình NAT Dynamic
Router(config)#ip nat inside source list MY_LAN pool MY_POOL


----------------------------------------------------------------------------------------------------------------
3. NAT/PAT overload 
Inside: nhiều IP -> định nghĩa IP-inside bằng ACL
Outside: ít IP hơn Inside

B1: tạo ACL cho IP-inside
B2: tạo Pool cho IP-outside nếu dùng IP tĩnh, không tạo Pool cho IP-outside nếu dùng interface outside
B3: ip nat inside source list <name-ACL> pool <name-Pool>
    ip nat inside source list <name-ACL> interface <port-number>
################################################################################################
VD: 
TH1: nat subnet 192.168.1.0/24 truy cập IP cổng ngoài G0/0 của router kết nối tới nhà mạng (ISP)
TH2: nat subnet 192.168.1.0/24 ra 1 IP Public mua từ nhà mạng (ISP) 200.0.0.99
################################################################################################
TH1: dùng outbound interface
B1. Inside (>=2 IP) -> cấu hình ACL
Router(config)#ip access-list standard MY_LAN 
Router(config-std-acl)#permit 192.168.1.0 0.0.0.255 
B2. Outside là interface cổng ngoài (outbound inteface) -> không cần định nghĩa Pool
B3. cấu hình NAT
Router(config)#ip nat inside source list MY_LAN interface G0/0
################################################################################################
TH2: dùng IP Public mua từ nhà mạng
B1. Inside (>=2 IP) -> cấu hình ACL
Router(config)#ip access-list standard MY_LAN 
Router(config-std-acl)#permit 192.168.1.0 0.0.0.255 

B2. Outside (>=1 IP) -> cấu hình Pool
Router(config)#ip nat pool MY_POOL 200.0.0.99 200.0.0.99 prefix-length 30 (prefix-length hoặc mask yêu cầu phải lớn hơn /30, nên cứ định nghĩa ít hơn 4IP thì dùng /30)
hoặc dùng mask
Router(config)#ip nat pool MY_POOL 200.0.0.99 200.0.0.99 mask 255.255.255.252
B3. cấu hình NAT
Router(config)#ip nat inside source list MY_LAN pool MY_POOL




----------------------------------------------------------------------------------------------------------------
PAT (Port Address Translation): chuyển đổi port
1. Static PAT (Mapping từ 1 port - 1  port)
	ip nat inside source static protocol <IP-inside> <port-inside> <IP-outside> <port-outside>
				--> protocol: tcp/udp
VD: NAT Web server (tcp port 80) có IP 192.168.1.1 sang IP Public với port là 8080
Router(config)#ip nat inside source static tcp 192.168.1.1 80 200.0.0.97 8080

2. Overload PAT giống overload NAT(Source IP nhiều - Destination IP ít)
Inside: nhiều IP -> định nghĩa Source bằng ACL
Outside: ít IP 
TH1: (>=1) -> định nghĩa Destination bằng Pool
TH2: dùng IP ở interface outside (cổng WAN của ISP cung cấp)
ip nat inside source list <name-ACL> interface <port-number> ""không tạo Pool nếu dùng địa chỉ trên Interface""

VD: 
TH1: nat subnet 192.168.1.0/24 truy cập IP cổng ngoài G0/0 của router kết nối tới nhà mạng (ISP)
TH2: nat subnet 192.168.1.0/24 ra 1 IP Public mua từ nhà mạng (ISP) 200.0.0.99
################################################################################################
TH1: dùng outbound interface
B1. Inside (>=2 IP) -> cấu hình ACL
Router(config)#ip access-list standard MY_LAN 
Router(config-std-acl)#permit 192.168.1.0 0.0.0.255 
B2. Outside là interface cổng ngoài (outbound inteface) -> không cần định nghĩa Pool
B3. cấu hình NAT
Router(config)#ip nat inside source list MY_LAN interface G0/0
################################################################################################
TH2: dùng IP Public mua từ nhà mạng
B1. Inside (>=2 IP) -> cấu hình ACL
Router(config)#ip access-list standard MY_LAN 
Router(config-std-acl)#permit 192.168.1.0 0.0.0.255 
B2. Outside (>=1 IP) -> cấu hình Pool
Router(config)#ip nat pool MY_POOL 200.0.0.99 200.0.0.99 prefix-length 30 (prefix-length hoặc mask yêu cầu phải lớn hơn /30, nên cứ định nghĩa ít hơn 4IP thì dùng /30)
hoặc dùng mask
Router(config)#ip nat pool MY_POOL 200.0.0.99 200.0.0.99 mask 255.255.255.252
B3. cấu hình NAT
Router(config)#ip nat inside source list MY_LAN pool MY_POOL

----------------------------------------------------------------------------------------------------------------
Quy tắc cấu hình NAT/PAT 
B1: tạo ACL cho IP-inside (nếu >= 2 IP), không tạo ACL nếu IP-inside = 1
B2: tạo Pool cho IP-outside, không tạo Pool nếu dùng địa chỉ trên Interface
B3: cấu hình NAT
TH1: - ip nat inside source 	static <IP-inside> <IP-outside>
				list <name-ACL> pool <name-Pool>
TH2: - ip nat inside source list <name-ACL> interface <port-number> ""không tạo Pool nếu dùng địa chỉ trên Interface""


