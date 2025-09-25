Ôn tập:
3 cơ chế chuyển mạch của Switch: Cut Through, Store & Forward, Fragment-Free (64Bytes)
Switch PoE (Power over Ethernet): Switch có tích hợp nguồn trong kết nối cáp dây Ethernet (RJ45)
thành phần phần cứng của Switch tương tự như Router: Board mạch, Nguồn, RAM, CPU, Flash (chứa OS), NVRAM (chứa Config).
Giao diện: 
Switch>enable (mode user)
Switch# (priviledge)
Xóa cấu hình trên Switch thì phải xóa luôn file vlan.dat (Switch thật)
Switch#erase startup-config (NVRAM)
Switch#delete vlan.dat

Backup và Restore thì cũng như Router:
- Copy config ra file bên ngoài (Backup)
- Paste lại vào trong Switch để Restore lại cấu hình.

Save config trên Switch cũng giống trên Router
Switch#copy running-config startup-config
Switch#write memory
Switch#wr

-------------------------------------------------------------------------------------------------------------------------
Layer2: VLAN - Trunking
1 Switch không có cấu hình (gắn nguồn lên để các thiết bị khác kết nối) chỉ cùng 1 LAN
VLAN: dùng để giảm Broadcast Domain, giải quyết về kết nối khi có sự thay đổi vị trí (sử dụng tối ưu nhất các Port của Switch)
--> 1 Switch có thể có nhiều LAN 
1 Switch vật lý chỉ có 1 LAN sẽ phân thành n Switch ảo với n VLAN.

n = 2^12 - 2 = 4094 VLAN (1-4094, bỏ VLAN0 và VLAN4095).

Khi kết nối giữa các Switch với nhau thì phải có đường dẫn VLAN
- Chưa có công nghệ Trunking thì 1 VLAN phải có 1 dây dẫn cho VLAN đó.
- Công nghệ Trunking dùng để giải quyết kết nối giữa Switch và Switch (1 dây Trunking chứa được 4094 VLAN).
Dây Trunking nhận diện được VLAN thông qua cơ chế TAG VLAN.
Tag: Trunking port
& Untag: Access port

Trunking có 2 công nghệ: ISL (Cisco) - 28 Bytes, Dot1q - 4 Bytes (Standard-IEEE)

Config của VLAN và Trunking
Tạo VLAN:
Switch(config)#vlan <vlan-id>
Switch(config)#vlan 10
Switch(config-vlan)#name IT
Gán VLAN vào interface:
Switch(config)#interface e0/1
Switch(config)#description "Connect-to-PC-PhuongLam-IT"
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10

Trunking:
Switch(config)#interface e0/0
Switch(config)#description "Trunking-Connect-to-SWCore"
Switch(config-if)#switchport trunk encapsulation dot1q/isl
Switch(config-if)#switchport mode trunk

-------------------------------------------------------------------------------------------------------------------------
Dynamic Trunking Protocol (DTP): thỏa thuận Trunk/Access
Switch khi port chưa cấu hình ở mode AUTO (sao cũng được)
Trunk: nhiều VLAN
Access: 1 VLAN
Desirable: Nhiều VLAN/ 1 VLAN
Auto: sao cũng được

Trunk - Trunk 		= Trunk
Trunk - Auto 		= Trunk
Trunk - Desirable	= Trunk
Trunk - Access		= N/A

Auto - Auto		= Access

Switch Cisco có VLAN 1 là VLAN Default, tất cả các port khi chưa cấu hình đều là AUTO
Switch (Auto)----- Switch (Auto) = Access VLAN 1

Switch (Trunk) -----Switch (Trunk) = Trunk (dẫn nhiều VLAN nên chứa cả VLAN1)

Lưu ý: tất cả các Switch trong 1 công ty phải có đủ VLAN để đảm bảo vấn đề tương tác với nhau.
(VLAN End to End)

SW1(10,20,30)------------SW2(10,20,30)----------------SW3(10,20)-------------SW4(10,20,30)-------------SW5(10,20,30)
PC(VLAN30)------------------PC(VLAN30)
-> Các port thuộc VLAN30 bắt đầu từ SW4 bị rơi vào trạng thái lơ lửng. 
-> Khắc phục: tạo thêm VLAN30 trên SW3 thì các port của VLAN30 từ SW4 trở lại bình thường.

-------------------------------------------------------------------------------------------------------------------------
VTP dùng bộ VLAN: có 3 mode
- Server: tạo VLAN, xóa VLAN, đồng bộ VLAN
- Client: không tạo VLAN, không xóa VLAN, đồng bộ VLAN
- Transparent: tạo VLAN, xóa VLAN, không đồng bộ VLAN (phải tạo VLAN manual)

-------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------
Tương quan Layer2 và Layer 3:
-------------------------------------------------------------------------------------------------------------------------
TH1: Routing InterVLAN with Router (Router đóng vai trò làm Gateway) - Router on a Stick
Router là Layer 3
Switch là Layer 2
Để Router có thể nhận diện được VLAN trên port Router thì Router phải 
dùng đến công nghệ để tương tác được với Trunking của Switch.
--> Router on a Stick sẽ Stick Vlan trên port ảo của Router (Sub-interface)
Tất cả các VLAN trong mạng sẽ tương tác được với nhau.
-------------------------------------------------------------------------------------------------------------------------
TH2: Routing InterVLAN with Interface VLAN (Interface VLAN đóng vai trò làm Gateway)
- Interface VLAN ra đời để giải quyết vấn đề quản trị thiết bị (SSH/Telnet)
- Interface VLAN để giảm tải cho Router --> làm GW cho toàn bộ VLAN trong mạng (nên vừa làm GW vừa làm DHCP Relay Agent)
- Interface VLAN được tạo ra và UP thành công, phải đủ 2 điều kiện.
VD: Interface vlan 10 muốn UP thì phải thỏa mãn 2 điều kiện
- VLAN 10
- Ít nhất 1 port access VLAN10 hoặc có TRUNKING
-------------------------------------------------------------------------------------------------------------------------
TH3: Switch - Layer 3 (Switch giờ đóng vai trò như Router, sự tương đồng tương tác giữa các VLAN trên Switch (interface vlan) và trên Router (sub-interface)
- Port kết nối từ Switch lên Router đươc thay từ Layer 2 (trunking) thành port Layer 3 (IP) 
-> ngăn hoàn toàn Broadcast lên Router (BW trên Router được bảo toàn trọn vẹn)
--> phân đoạn này thành phân đoạn Layer 3 hoàn toàn nên việc định tuyến tương tự như Router với Router (BW trên phân đoạn này được giữ nguyên trọn vẹn)

- Interface VLAN trên Switch để giảm tải cho Router --> làm GW cho toàn bộ VLAN trong mạng (nên vừa làm GW vừa làm DHCP Relay Agent)
-------------------------------------------------------------------------------------------------------------------------
Vậy 1 SWitch sẽ có 4094 interface VLAN
và Interface VLAN là interface Layer 3 ảo của Switch (layer 2)
Interface VLAN làm Gateway để giảm tải cho Router
Lưu ý: VLAN Management là 1 VLAN không Access cho bất kỳ thiết bị nào trong mạng.
và VLAN Management phải là VLAN khác quy hoạch cho VLAN của các phòng ban.

-------------------------------------------------------------------------------------------------------------------------
VD: định tuyến giữa các VLAN dùng interface VLAN (Number) là interface được tạo trên SW 
SW1(10,30)-------------------SW2(30)----------------SW3(10,20)
		            PC(VLAN30)--------------PC(VLAN20)

Chiều đi:
SW2(config)#ip route 172.16.20.0 255.255.255.0 172.16.30.1

SW1(config)#ip route 172.16.20.0 255.255.255.0 172.16.10.3

Chiều về:
SW3(config)#ip route 172.16.30.0 255.255.255.0 172.16.10.1


------------------------------------------------------------------------------------------------------
Những lưu ý trong bài lab số 11
------------------------------------------------------------------------------------------------------
TH1:
Với SW-Core là Port Layer 3: cần định tuyến để Router thấy các subnet 10,20,30 
với next-hop int e0/2 (port layer 3 - IP: 192.168.100.2)

Core(config)#interface e0/2
Core(config-if)#no switchport
Core(config-if)#ip add 192.168.100.2 255.255.255.0

Router(config)#interface e0/0
Router(config-if)#no shut
Router(config-if)#ip add 192.168.100.1 255.255.255.0

------------------------------------------------------------------------------------------------------
TH2: cần định tuyến để Router thấy các subnet 10,20,30 
với next-hop int vlan 100 (192.168.100.2)

Port Layer 2: Access
Core(config)#interface e0/2
Core(config-if)#switchport
Core(config-if)#switchport mode accesss
Core(config-if)#switchport access vlan 100
Core(config)#int vlan 100
Core(config-if)#no shut
Core(config-if)#ip add 192.168.100.2 255.255.255.0

Router(config)#interface e0/0
Router(config-if)#no shut
Router(config-if)#ip add 192.168.100.1 255.255.255.0

------------------------------------------------------------------------------------------------------
TH3: không cần định tuyến do router chứa sub-interface 
-> bị broadcast lên Router
Port Layer 2: Trunk
Core(config)#interface e0/2
Core(config-if)#switchport
Core(config-if)#switchport trunk encapsulation dot1q
Core(config-if)#switchport mode trunk
Core(config)#int vlan 100
Core(config-if)#no shut
Core(config-if)#ip add 192.168.100.2 255.255.255.0

Router(config)#interface e0/0
Router(config-if)#no shut
Router(config)#interface e0/0.100
Router(config-if)#encapsulation dot1q 100
Router(config-if)#ip add 192.168.100.1 255.255.255.0

#########################################################################################################################################
				ARCHITECTURE NETWORKING
#########################################################################################################################################
SW Layer 3 sẽ đưa ra mô hình phân cấp hạ tầng SW để giải quyết về vấn đề phân chia VLAN, Trunking, GW và định tuyến
Mô hình của Cisco phân ra các cấp như sau
1. Mô hình 3 cấp (mô hình chuẩn): >= 1000 user/ những công ty đa quốc gia/ công ty có nhiều Site chi nhánh
2. Mô hình Hybrid: phân hoá dạng nhiều site nhưng cùng 1 khu vực (dùng để tiết kiệm chi phí)

khi số lượng SW Access quá nhiều thì cần 1 lớp trung gian giảm đi sự kết nối Access (Distribution sẽ đảm nhiệm)
--> Do Switch không thể mở rộng port được nữa --> chuyển hoá từ Hybrid sang 3 lớp

Interface VLAN trong mô hình thực tế sẽ ở Switch Distribution (3 lớp)/ Switch Core (Hybrid)

SW Core và SW Distribution sẽ là SWitch Layer 3.
--> Switch Distribution sẽ trỏ toàn bộ định tuyến về Switch Core (ip route 0.0.0.0 0.0.0.0 về next-hop của Core)
Trong mô hình thực tế:
Quy hoạch về mặt IP đấu nối.
Subnet riêng dành cho Distribution và Core
Subnet riêng dành cho Core và FW
Subnet riêng dành cho FW và Load Balancing
-> có thể gom vào cùng 1 subnet đấu nối

Switch Access trong mô hình 3 lớp/Hybrid thì Downlink là Access & Uplink là Trunk. Interface VLAN sẽ đóng vai trò quản lý thiết bị
Switch Distribution trong mô hình 3 lớp thì Downlink là Trunk & Uplink là IP (Layer 3). Interface VLAN sẽ được đóng vai trò làm GW và quản lý thiết bị
Switch Core 
- Trong mô hình 3 lớp dùng 100% port là IP (port layer3). Chỉ tạo duy nhất 1 Interface VLAN cho quản lý thiết bị
- Trong mô hình Hybrid (giống Distribution) downlink là Trunk và Uplink là IP. Interface VLAN sẽ được đóng vai trò làm GW và quản lý thiết bị

