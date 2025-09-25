			VPN - Virtual Private Network
############################################################################################################################################
- Kết nối giữa 2 đầu xa (2 nơi khác nhau), mang tính bảo mật hơn.

- Dùng để vào mạng nội bộ (LAN) của công ty (tài liệu, server).

VPN dùng để kết nối đến công ty thực hiện các công việc tại nhà, 1 nơi nào đó bất kỳ trên thế giới (cafe, nước ngoài,...)
vẫn có thể làm việc được như ở công ty.
- VPN: Site to Site là dùng để kết nối 2 nơi nhưng cùng thuộc 1 mạng LAN.
Công ty A có 2 site ở HCM và HN, 2 nơi này cùng 1 mạng LAN để truy cập qua lại lẫn nhau như cùng 1 văn phòng.

- VPN: Client to Site - dùng 1 phần mềm của thiết bị mạng để người dùng truy cập vào mạng công ty ở bất kỳ nơi nào.

Khi bật VPN và kết nối thì sẽ biến mạng tại nhà/cafe thành mạng công ty (đi nơi nào cũng có mạng công ty để làm việc).
-> chỉ cần bật phần mềm VPN lên là sẽ biến 1 mạng bất kỳ nào cũng thành mạng công ty.

---------------------------------------------------------------------------------------------------------------------------------------------
VD: Quy hoạch mạng của công A
Wifi: 192.168.1.0/24 
LAN: 192.168.2.0/24
Server: 192.168.3.0/24
VPN (Client to Site): 192.168.4.0/24

- Nếu người dùng Laptop ở tại công ty thì sẽ nhận IP - 192.168.1.0/24
- Nếu người dùng xài PC ở tại công ty thì sẽ nhận IP - 192.168.2.0/24
- Nếu người dùng sử dụng đt, tablet, Laptop, PC bật phần mềm VPN để truy cập đến mạng công ty
thì sẽ được nhận IP - 192.168.4.0/24
--> khi bật VPN thì người dùng sẽ truy cập dc các Server/File thuộc 192.168.3.0/24

Trước khi ra đời VPN thì sự kết nối giữa các Site để trao đổi thông tin đều thông qua dịch vụ MetroNet của ISP
--> Chưa có công nghệ kết nối từ người dùng đến công ty

Do chi phí duy trì MetroNet quá cao nên VPN ra đời để giải quyết vấn đề kết nối giữa các Site với nhau
(Site-to-Site, Site-to-Multisites) (nhiều users kết nối đến công ty)
và có các công nghệ bổ trợ cho việc kết nối từ người dùng đến công ty (Client-to-Site) (1 người kết nối đến công ty).

--> VPN sẽ dựa vào kết nối thông qua Internet (đòi hỏi phải có IP Public tĩnh).

---------------------------------------------------------------------------------------------------------------------------------------------
Configuration
	    172.16.1.0/24							 172.16.2.0/24
		R1 (G0/0)--------------------Internet-------------------------------(G0/1) R2
	(IP vật lý: G0/0 - 100.0.0.1/30)				(IP vật lý: G0/1 -200.0.0.1/30)
	(IP Tunnel: Tunnel12 - 192.168.12.1/24)				(IP Tunnel: Tunnel12 - 192.168.12.2/24)

R1(config)#interface tunnel 12						R2(config)#interface tunnel 12
R1(config-if)#ip address 192.168.12.1 255.255.255.2			R2(config-if)#ip address 192.168.12.2 255.255.255.2		
R1(config-if)#tunnel source G0/0					R1(config-if)#tunnel source G0/1
R1(config-if)#tunnel destination 200.0.0.1				R1(config-if)#tunnel destination 100.0.0.1
R1(config)#ip route 172.16.2.0 255.255.255.0 tunnel 12			R1(config)#ip route 172.16.1.0 255.255.255.0 tunnel 12

---------------------------------------------------------------------------------------------------------------------------------------------
Khóa học tham khảo
SVPN - CCNP Security

Cách Routing đối với Tunnel: nên sử dụng Outbound interface
1. Static Route (IP Next-hop): ip route <destination-subnet> <subnet-mask destination-subnet> <"IP Next-hop">
--> nên cấu hình IP Next-hop đối với interface vật lý (VD: interface g0/0, interface f0/0, interface e0/0) 
2. Static Route (Outbound-Interface): ip route <destination-subnet> <subnet-mask destination-subnet> <outbound-interface>
--> nên cấu hình Outbound Interface đối với tunnel VPN (VD: interface tunnel 1, interface tunnel 12, ...)
############################################################################################################################################


						IPv6
############################################################################################################################################
IPv4 là 1 chuỗi 32 bit. Số IP có thể có 2^32 ~ 4.3 tỷ IP
- IP Private dùng để kết nối trong mạng Local.
- NAT dùng để tiết kiệm IPv4 (nhiều IP sử dụng trên 1 IP).
- 1 chuỗi 32bits
- chia thành 4 octets (4 phần)
- mỗi phần cách nhau bằng dấu "."
- Mỗi phần chứa 8 bits.
- tính theo hệ thập phân từ 0-255 (cách tính nhị phân là dùng được cho tất cả)
- Subnetmask biểu diễn bằng số thập phân như địa chỉ IP

---------------------------------------------------------------------------------------------------------------------------------------------
IPv6 là 1 chuỗi 128 bit. Số IPv6 có thể có 2^128
Dân số thế giới là 8 tỷ 2^34. Trong 100 năm nữa thế giới tăng gấp 10 lần 80 tỷ người 2^40
---> Số IPv6 của 1 người có thể có là 2^128/2^40 ~ 2^88
IPv6 ra đời giải quyết vấn đề thiếu hụt IP.

Do không gian của IPv6 rất lớn nên không thể Broadcast ra toàn bộ được.
--> IPv6 không có Broadcast.
Thay vào đó IPv6 có 1 cơ chế commucation mới là: Anycast (giao tiếp với Destination nào gần nó nhất)
--> dựa vào thông tin định tuyến để biết được Destination nào là gần nhất.

- IPv6 là 1 chuỗi 128bits
- chia thành 8 phần
- mỗi phần cách nhau bằng dấu ":"
- Mỗi phần chứa 16bits (thường viết 4bits biểu diễn 1 chữ số)
- tính theo hệ thập lục phân(0-F)
- Subnetmask được ghi bằng /<số prefix>

---------------------------------------------------------------------------------------------------------------------------------------------
Quy tắc viết IPv6:
IPv6 là 1 chuỗi 128 bit được chia thành 8 phần, mỗi phần 16 bit (xxxx xxxx xxxx xxxx)
Số 0 đằng trước được quyền rút gọn, số 0 phía sau không được quyền rút gọn
VD: 2003:078F::1 ~ 2003:78F::1
    2003:78F0::1 ~ 2003:78F0::1 (không rút gọn số 0 phía sau)
Trong 1 IPv6 chỉ duy nhất được 1 lần viết tắt :: (đại diện cho nhiều số 0)

Địa chỉ Link-Local: địa chỉ IPv6 này chỉ có tác dụng trong 1 link kết nối giữa 2 Router
Địa chỉ Link-Local không định tuyến được và chỉ có tác dụng để kiểm tra đấu nối IPv6 và thường để dễ nhận diện Router.

IPv4 thường quy hoạch là /24
IPv6 thường quy hoạch là /64
IPv6 Private FC00::/7 thì sẽ có bao nhiêu subnet /64 (2^(64-7) = 2^57 subnet /64) ~ 2^57 phòng ban, với mỗi phòng ban /64

::/0 là default route IPv6
0.0.0.0 0.0.0.0 là default route của IPv4

---------------------------------------------------------------------------------------------------------------------------------------------
VD: phân tích nhị phân của địa chỉ IPv6 - 2031:0000:130F:0000:0000:09C0:876A:130B/64
2301 = 0010 0000 0011 0001 	
130F = 0001 0011 0000 1111
0000 = 0000 0000 0000 0000
0000 = 0000 0000 0000 0000
09C0 = 0000 1001 1100 0000
876A = 1000 0111 0110 1010
130B = 0001 0011 0000 1011

VD: Viết lại địa chỉ IPv6 - 2031:0000:130F:0000:0000:09C0:876A:130B theo cách tối giản nhất: 
Quy tắc:
- số 0 phía trước có thể loại bỏ.
- Nhiều số nhất trong đoạn IPv6 được biểu diễn bằng "::" 
-> TRONG 1 IPv6 chỉ có duy nhất 1 lần "::"

2031:0:130F:0:0:9C0:876A:130B
2031:0:130F::9C0:876A:130B

0.0.0.0 0.0.0.0 default route của IPv4

0:0:0:0:0:0:0:0 default route của IPv6
::/0 -> default route của IPv6

0:0:0:0:0:0:0:1/96
::1/96

2^32 IPv4 khoảng 4.5 tỷ IP

2^35 khoảng 40 tỷ thiết bị

2^128 IPv6

số lượng IPv6 trên 1 người có thể sử dụng
dân số khoảng 10 tỷ ~ 2^33

2^128 / 2^33 = 2^(128-33) = 2^95

-> không có Broadcast
mà sẽ tương tác theo 1 loại community mới là Anycast (tương tác với địa chỉ IPv6 nào gần nhất theo định tuyến)

---------------------------------------------------------------------------------------------------------------------------------------------
Các kiểu community của IPv6:
- Unicast
- Multicast
- Anycast (Không có broadcast)

IPv6 thường sử dụng Auto config bằng cách 
- sử dụng địa chỉ MAC (48bits) của thiết bị 
- kết hợp với 1 dãy 16bit FF-FE 
- và nghịch đảo bit số 7 trong chuỗi MAC

---------------------------------------------------------------------------------------------------------------------------------------------
Trên 1 interface luôn tồn tại 2 loại IPv6
- Địa chỉ IPv6 Link-Local FE80::/12 (tự động sinh ra khi bật IPv6 trên router/Switch
- Địa chỉ IPv6 của interface (do con người đặt, hoặc cấu hình theo chủ ý "manual"/"auto" theo range IP của người quản trị)

Link-Local chỉ có giá trị trên 1 link (phân đoạn) -> không dùng để định tuyến được (mục tiêu là để xác định Router)
Router có 4 port thì chỉ cần 1 địa chỉ Link-local

Khi ping để kiểm tra phân đoạn đó bằng địa chỉ Link-Local đã kết nối hay chưa thì Ping IPv6 destination + Outbound interface

---------------------------------------------------------------------------------------------------------------------------------------------
Bật IPv6 trên router:
Router(config)#ipv6 unicast-routing

Show:
ipv4: Router#show ip int brief

ipv6: Router#show ipv6 int brief

1. Static Route
ipv6 unicast-routing (start routing IPv6)
ipv6 route <destination-IPv6 subnet> <IPv6-next hop>


Dynamic Route: vẫn nhận IPv4 làm Router-ID, chỉ hỗ trợ cấu hình định tuyến IPv6 Dynamic Routing trên interface.
Không cấu hình được trên mode router.
Router(config)#ipv6 router ospf 1
Router(config-router)#router-id 1.1.1.1

---------------------------------------------------------------------------------------------------------------------------------------------
Cách IPv6 cấu hình tự động - EUI-64
MAC của thiết bị: 05-36-E8-BB-14-CB (MAC là 1 chuỗi 48 bit)
B1: tách địa chỉ MAC làm 2 phần bằng nhau
05-36-E8
BB-14-CB

B2: chèn giữa 2 phần FF-FE (chèn thêm 16bit)
05-36-E8-	FF-FE	-BB-14-CB
05-36-E8-FF-FE-BB-14-CB

B3: sẽ lấy nghịch đảo bit thứ 7 của chuỗi đã chèn
05 = 0000 0101
lấy nghịch đảo bit thứ 7
     0000 0111

B4: địa chỉ IPv6 được cấp tự động
FE80::736:E8FF:FEBB:14CB/64

---------------------------------------------------------------------------------------------------------------------------------------------
Configuration IPv6:

IPv6 Routing: đầu tiên phải bật tính năng ipv6 routing
Router(config)#ipv6 unicast routing

mỗi interface sẽ có:
1 IPv4
1 IPv6 (Public/Private)
1 IPv6 (Link-Local)
Do chạy được IPv4 lẫn IPv6 nên được gọi
hỗ trợ Dual Stack

I. Manual: tự quy hoạch IPv6 trên interface
	int G0/0
	  ipv6 add 2001::1/64

II. Automatic: EUI-64 (tự động 64 bit cuối)
dùng MAC chèn giữa MAC bằng FF-FE
MAC (48 bits) + FF-FE (16 bits) = 64
--> Sau đó lấy nghịch đảo bit thứ 7

1. Static Route
IPv4: 
Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.12.1
IPv6: 
Router(config)#ipv6 route 2003::1:0/64 2003::12:1

2. Dynamic Route:
IPv4: có 2 cách cấu hình:
	- cấu hình mode router
Router(config)#router ospf 1
Router(config-router)#router-id 1.1.1.1
Router(config-router)#network 192.168.1.1 0.0.0.0 area 0
192.168.1.1 là của interface e0/0 

	- vào interface enable ospf (không khuyến nghị cấu hình OSPF)
Router(config)#interface e0/0
Router(config-if)#ip ospf 1 area 0
Router(config)#interface e0/1
Router(config-if)#ip ospf 1 area 0

IPv6: sẽ vào interface để enable Routing OSPF
Router(config)#ipv6 router ospf 1
Router(config-router)#router-id 1.1.1.1
Router(config)#interface e0/0
Router(config-if)#ipv6 ospf 1 area 0
Router(config)#interface e0/1
Router(config-if)#ipv6 ospf 1 area 0

A = 1010
AA = 1010 1010
     1010 1000
