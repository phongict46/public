Cáp:
1. Cáp đồng
2. Cáp quang

Cáp đồng: cáp đồng trục, cáp xoắn Ethernet (UTP, STP)
Cáp quang: 1 lõi, 2 lõi - Multimode, SingleMode.

- Cáp đồng trục
Camera, truyền hình cáp - Layer 1 

- Cáp Ethernet:
Chéo:   Router - Router
	Router - PC
	PC     - PC
	Switch - Switch
	Hub    - Hub

Thẳng:  Router - Switch
	Router - Hub
	PC     - Switch
	PC     - Hub

Layer 3 - Layer 3: chéo (Router, PC)
Layer 2 - Layer 2: chéo
Layer 1 - Layer 1: Chéo
Layer 2 - Layer 1: chéo
-> Layer2 & Layer1 được tính chung là 1 loại.

Layer3 - Layer2/Layer1: thẳng


Router Cisco là 1 Server chạy hệ điều hành Linux của Cisco (không dùng hệ điều hành khác được).
Physical: Mainboard, RAM, CPU, Power, các port mạng để kết nối.
Dây console - Layer1 

PC/Server khởi động:
- Check phần cứng: nguồn, các thông số trong mạch (BIOS)
- Ổ đĩa lấy hệ điều hành.

Router khởi động:
- Check phần cứng: nguồn, các thông số trong mạch (ROM)
- Ổ đĩa lấy hệ điều hành (Flash) - IOS Cisco
- Boost thông số cấu hình (NVRAM).


Router> mode user
- enable -> đi tiếp tục vào router
- exit -> thoát

Router# mode priviledge
- Show thông số cấu hình của thiết bị 
- Show thông tin trạng thái cổng
-> Show được tất cả các thông số, thông tin, trạng thái, ... của thiết bị.
- chỉ được cấu hình (config) duy nhất là Network Time Protocol tại mode priviledge này.

Ethernet - 10Mb
FastEthernet - 100Mb
GigaEthernet - 1Gb
TenGigaEthernet - 10Gb

show running-config: xem toàn bộ thông số cấu hình của Router
show ip interface brief: xem trạng thái interface 1 cách ngắn gọn.

Router#configure terminal
Router(config)# mode global config
cấu hình (config) cho thiết bị.


Khi cấu hình thì không nhớ thì ? để xem các tập lệnh có thể có

Để từ mode Global config -> proiviledge
Router(config)#end
Router#

Có thể cấu hình vắn tắt/đầy đủ. Cấu hình vắn tắt nhưng show 1 cách đầy đủ và xem có chính xác hay không 
thì dùng TAB.


