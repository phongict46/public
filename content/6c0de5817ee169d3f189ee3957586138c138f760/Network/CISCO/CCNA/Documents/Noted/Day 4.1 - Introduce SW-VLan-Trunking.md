Module 2: Switching

Layer4: Segment (phiên)
Layer3: Packet (gói)
Layer2: Frame (khung) -> Switching
Layer1: Bit (giá trị 0 và 1)

-------------------------------------------------------------------------------------------------------------------------
3 cơ chế chuyển mạch của Switch: hoạt động theo song công (Full Duplex), học MAC trên interface
1. Cut-Through: khi nhận Frame sẽ lập tức chuyển đi ngay (tương tự với cơ chế của UDP trong Layer4-Transport)
2. Store-and-Forward: khi nhận Frame sẽ kiểm tra, nếu Frame được nhận đầy đủ và không lỗi thì mới chuyển Frame (tương tự với cơ chế của TCP trong Layer4-Transport)
3. Fragment-Free: 64Bytes đầu tiên sẽ kiểm tra như Store-&-Forward và từ Byte thứ 65 sẽ theo cơ chế Cut-Through.

-------------------------------------------------------------------------------------------------------------------------
Switch học MAC bằng giao thức: ARP (IP -> MAC)
Có MAC của thiết bị, muốn gắn MAC với 1 IP cố định nào đó: Reverse ARP (R-ARP) --> Server
MAC được lưu ở RAM, nên khi khởi động lại (Reload/Reset/Reboot) sẽ mất bảng MAC và sẽ học lại MAC khi Switch hoạt động.

Switch dùng để mở rộng hạ tầng thiết bị mạng (đóng vai trò kết nối các thiết bị mạng lại với nhau: Camera, PC, Server, Printer, ...)

-------------------------------------------------------------------------------------------------------------------------
Ethernet LAN: Switch dùng để kết nối các thiết bị trong mạng LAN theo chuẩn Ethernet-802.3 (Đồng/Quang)
Cáp RJ45(đồng)/Single-Multi-Mode(Quang)

-------------------------------------------------------------------------------------------------------------------------
PoE: Power over Ethernet
Port của Switch sẽ cung cấp nguồn cho thiết bị mạng khi kết nối vào Switch (thường dùng Camera, Access Point).

-------------------------------------------------------------------------------------------------------------------------
Đối với 1 Switch vật lý (Switch không có cấu hình, hoặc không cấu hình cho Switch) 
thì 1 Switch chỉ kết nối được thiết bị của 1 LAN.

Phương án để giải quyết vấn đề khi có sự thay đổi vị trí giữa các LAN với nhau.
VD: IT có thể ngồi ở bất cứ phòng ban nào, nhưng khi gắn dây mạng vẫn sử dụng được lớp mạng của IT.

Để giải quyết vấn đề thay đổi vị trí giữa các LAN, Switch vật lý sẽ phân chia nó thành n Switch ảo trong 1 thiết bị vật lý (VLAN).
VD: 1 Switch chỉ chứa được 1 LAN, vậy 10 LAN sẽ cần 10 SW --> về mặt vật lý của Switch
    1 Switch có thể chia được VLAN (thành n Switch ảo trong SW vật lý) mỗi port của Switch có thể là 1 VLAN (Virtual LAN).
    -> 1 Switch 48 port có thể chứa 48 VLAN khác nhau (48 phòng ban khác nhau trên 1 Switch) và 48 VLAN nào sẽ không tương tác được với nhau (48 VLAN tương đương 48 subnet).

n = 2^12 - 2 = 4094 VLAN (1 - 4094, bỏ VLAN0 và VLAN4095)

Chia nhỏ vùng Broadcast Domain: giải quyết được vấn đề Băng Thông và rủi ro truy cập trong mạng.

-------------------------------------------------------------------------------------------------------------------------
Switch Cisco có VLAN1 là VLAN Default - tất cả các port của Switch ban đầu đều cùng VLAN1.
-------------------------------------------------------------------------------------------------------------------------
VLAN Native là VLAN không bị tag trên đường TRUNK, nên có thể truy cập đến tất cả các Switch trong mạng.
Nếu thay đổi VLAN Native thì trên đường trunk ở đầu Switch phải cấu hình VLAN Native giống nhau.
Nều 2 đầu Switch cấu hình VLAN Native khác nhau thì vẫn đưa về VLAN1 làm VLAN Native.
Giá trị chuyển VLAN Native chỉ có tác dụng trên đoạn trunking mà nó cấu hình.

-------------------------------------------------------------------------------------------------------------------------
Trunk (nhiều)
Access (ít - chỉ 1 VLAN)
Auto (sao cũng được)
Desirable (nhiều hoặc ít đều được)

Switch - Switch  (Trunking)
Switch - Router  (Trunking/Access)
Switch - Layer 3 khác (Access)


-------------------------------------------------------------------------------------------------------------------------
VTP: dùng để đồng bộ VLAN trên hạ tầng Switch
VTP có 3 mode: Server, Client và Transparent.
	Server: tạo VLAN, xóa VLAN và đồng bộ VLAN.
	Client: không tạo VLAN, không xóa VLAN, chỉ đồng bộ VLAN.
	Transparent: tạo VLAN, xóa VLAN nhưng không đồng bộ VLAN.

Khi Xóa hoặc tạo VLAN thì Revision Number trên VTP của Switch Server sẽ tăng lên 1.

-------------------------------------------------------------------------------------------------------------------------
Switch khi clear cấu hình không mất VLAN.
muốn xóa cả VLAN thì phải xóa file vlan.dat

Switch#erase startup-config
Switch#delete vlan.dat

Lưu ý: dù xóa VLAN thì VTP domain vẫn còn, nên trước khi đem SWitch gắn vào mạng thì kiểm tra số Revision Number (Configuration Revision)
xem có nhỏ hơn con SWitch VTP Server hay không.
Nếu Switch nào có số Revision lớn hơn sẽ đồng bộ theo Switch đó.

-------------------------------------------------------------------------------------------------------------------------
Configuration:
Tổng quan về Switch:
1 Switch vật lý thì chỉ dẫn 1 VLAN (1LAN) trên all port.
Khi sử dụng VLAN để cấu hình trên Switch thì có thể có n VLAN trên n port.
- VLAN: chia nhỏ Broadcast Domain, giải quyết vấn đề kết nối giữa các LAN với nhau (Access trên port).
B1: tạo VLAN: tạo 3 VLAN
Switch(config)#vlan 10
Switch(config-vlan)#name ICT
Switch(config)#vlan 20
Switch(config-vlan)#name KT
Switch(config)#vlan 30
Switch(config-vlan)#name MKT

B2: Access VLAN vào port
Switch(config)#interface e0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan <vlan-id>

B3: kiểm tra port đã access vlan.
Switch#show vlan brief
Switch#show vlan

-------------------------------------------------------------------------------------------------------------------------
- Trunking: giải quyết vấn đề dẫn VLAN giữa các Switch, hoặc giữa Switch với Router.
(1 dây trunking chứa 4094 VLAN).
Lưu ý: nên cấu hình 2 đầu trunking giống nhau để dễ xử lý sự cố.

B1: encapsulation chuẩn Trunking vào interface (thường sử dụng là Dot1q)
Switch(config)#interface e0/1
Switch(config-if)#switchport trunk encapsulation dot1q

B2: cấu hình Mode trunk vào port
Switch(config-if)#switchport mode trunk

B3: kiểm tra port trunk
Switch#show interface trunk

-------------------------------------------------------------------------------------------------------------------------
- VTP: đồng bộ VLAN giữa các Switch (là giao thức chỉ hỗ trợ trên thiết bị Switch của Cisco)
có 3 mode: Server, Client và Transparent.
	Server: tạo VLAN, xóa VLAN và đồng bộ VLAN.
	Client: không tạo VLAN, không xóa VLAN, chỉ đồng bộ VLAN.
	Transparent: tạo VLAN, xóa VLAN nhưng không đồng bộ VLAN.

B1: cấu hình domain VTP
Switch(config)#vtp domain ccna.waren

B2: cấu hình mode VTP cho Switch (Server/Client)
Switch(config)#vtp mode server/client

B3: kiểm tra thông số VTP
Switch#show vtp status




