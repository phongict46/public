<<<<<<< HEAD
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
					Spanning Tree
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
Spanning Tree (chống Loop): các thông số đều thấp ưu tiên hơn
1. Bầu chọn Root-Switch: Bridge-ID gồm 2 thành phần (ưu tiên từ trên xuống):
a. Lowest Priority, giá mặc định Priority Switch Cisco - 32768 +/- (n x 4096) 
-> thay đổi để chọn Root Switch 1 cách chủ động
--> Priority thấp nhất = 0 (thông số điều chỉnh được -> cấu hình được)
b. Lowest MAC
-> (thông số không điều chỉnh được -> không cấu hình được)

---> Toàn bộ port của Root Switch sẽ là Designated Port (DP)

--------------------------------------------------------------------------------------------------------------------
2. Bầu chọn Root Port -> Lowest Root Path Cost (Cost tính từ Root Switch có giá trị thấp nhất)
- Cost
Cost được tính từ Root Switch đến các port của các Switch còn lại.
Port nào có Cost thấp nhất là Root Port
(cách tính Cost: đi ra không tính, đi vào mới tính)
VD: Root Switch F0/0 -------- G0/0 Switch
		19		4		
	Cost = 4 (đi ra từ port F0/0 - không tính, đi vào là Port G0/0 - tính Cost)

---------------------------------------------------------------------------------------------------------------------
3. Bầu chọn Designated Port (DP) trên mỗi phân đoạn mạng:
a. Root Path Cost
b. Sender-BridgeID(Priority, MAC)
c. Port-ID
Port-ID: 
TH1: Từ port của Sender-BridgeID, port nào có thứ tự thấp hơn sẽ ưu tiên hơn 
TH2: Ở giữa phân đoạn có Hub thì sẽ tính trên port-ID của chính nó (port nào có thứ tự thấp hơn sẽ ưu tiên hơn)

---------------------------------------------------------------------------------------------------------------------
4. Xác định Block Port
Trên mỗi phân đoạn mạng đều phải có 1 Designated Port.
- Cost
- Priority
- MAC
--> Port nào có giá trị cao hơn sẽ bị Block.

#######################################################################################################################
Spanning Tree chia làm 2 trường hợp để thành port Forwarding:
TH1: User (người dùng) kết nối vào Switch (port Access)
có 3 trạng thái: Listening - Learning - Forwarding
Listening -> Learning (15s)
Learning -> Forwarding (15s)
Tổng thời gian người dùng cắm cáp để sử dụng: 30s (Access)

---------------------------------------------------------------------------------------------------------------------
TH2: Uplink kết nối đến Root Switch bị Fail (giữa Switch với Switch -> Trunking)
có 4 trạng thái: Blocking - Listening - Learning - Forwarding
Blocking -> Listening (20s)
Listening -> Learning (15s)
Learning -> Forwarding (15s)
Tổng thời gian từ Block port sang Foward port: 50s (Trunking)

---------------------------------------------------------------------------------------------------------------------
Tăng tốc Spanning Tree:
TH1: User (người dùng) kết nối vào Switch (khoảng 6s là chuyển đèn từ Cam - Xanh lá)
-> Spanning Tree hỗ trợ cơ chế port-fast (chỉ tác dụng cho port MODE ACCESS)
Cách 1: vào từng interface hoặc interface range
Switch(config)#interface e0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#spanning-tree portfast (config thêm portfast để tăng tốc hội tụ Spanning Tree)

Cách 2: cấu hình port-fast tại mode Global config (chỉ tác động lên port Access, không tác động lên port Trunk).
Switch(config)#spanning-tree portfast default

TH2: Uplink kết nối đến Root Switch bị Fail
-> Spanning Tree hỗ trợ cơ chế Uplink-Fast, Backbone-Fast, ... (CCNP Switching)

PVST+: định nghĩa mỗi vlan sẽ có 1 spanning-tree khác nhau
--> để tận dụng tất cả các port đã block

---------------------------------------------------------------------------------------------------------------------
		DHCP
---------------------------------------------------------------------------------------------------------------------
Cấp IP 1 cách tự động
Tạo DHCP là định nghĩa 1 Pool
- Subnet cần để cấp
- Default-Gateway của subnet cần để cấp
- Option: DNS 

DHCP gồm 4 gói:
- DHCP Discovery (Broadcast): thiết bị mạng của người dùng tìm DHCP Server
- DHCP Offer (Unicast): từ DHCP Server gửi thông tin của DHCP Server về cho người dùng.
- DHCP Request (Unicast): request xin IP đến DHCP Server.
- DHCP Acknownledge (Unicast): cấp IP cho người dùng.

D O R A
=======
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
					Spanning Tree
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
Spanning Tree (chống Loop): các thông số đều thấp ưu tiên hơn
1. Bầu chọn Root-Switch: Bridge-ID gồm 2 thành phần (ưu tiên từ trên xuống):
a. Lowest Priority, giá mặc định Priority Switch Cisco - 32768 +/- (n x 4096) 
-> thay đổi để chọn Root Switch 1 cách chủ động
--> Priority thấp nhất = 0 (thông số điều chỉnh được -> cấu hình được)
b. Lowest MAC
-> (thông số không điều chỉnh được -> không cấu hình được)

---> Toàn bộ port của Root Switch sẽ là Designated Port (DP)

--------------------------------------------------------------------------------------------------------------------
2. Bầu chọn Root Port -> Lowest Root Path Cost (Cost tính từ Root Switch có giá trị thấp nhất)
- Cost
Cost được tính từ Root Switch đến các port của các Switch còn lại.
Port nào có Cost thấp nhất là Root Port
(cách tính Cost: đi ra không tính, đi vào mới tính)
VD: Root Switch F0/0 -------- G0/0 Switch
		19		4		
	Cost = 4 (đi ra từ port F0/0 - không tính, đi vào là Port G0/0 - tính Cost)

---------------------------------------------------------------------------------------------------------------------
3. Bầu chọn Designated Port (DP) trên mỗi phân đoạn mạng:
a. Root Path Cost
b. Sender-BridgeID(Priority, MAC)
c. Port-ID
Port-ID: 
TH1: Từ port của Sender-BridgeID, port nào có thứ tự thấp hơn sẽ ưu tiên hơn 
TH2: Ở giữa phân đoạn có Hub thì sẽ tính trên port-ID của chính nó (port nào có thứ tự thấp hơn sẽ ưu tiên hơn)

---------------------------------------------------------------------------------------------------------------------
4. Xác định Block Port
Trên mỗi phân đoạn mạng đều phải có 1 Designated Port.
- Cost
- Priority
- MAC
--> Port nào có giá trị cao hơn sẽ bị Block.

#######################################################################################################################
Spanning Tree chia làm 2 trường hợp để thành port Forwarding:
TH1: User (người dùng) kết nối vào Switch (port Access)
có 3 trạng thái: Listening - Learning - Forwarding
Listening -> Learning (15s)
Learning -> Forwarding (15s)
Tổng thời gian người dùng cắm cáp để sử dụng: 30s (Access)

---------------------------------------------------------------------------------------------------------------------
TH2: Uplink kết nối đến Root Switch bị Fail (giữa Switch với Switch -> Trunking)
có 4 trạng thái: Blocking - Listening - Learning - Forwarding
Blocking -> Listening (20s)
Listening -> Learning (15s)
Learning -> Forwarding (15s)
Tổng thời gian từ Block port sang Foward port: 50s (Trunking)

---------------------------------------------------------------------------------------------------------------------
Tăng tốc Spanning Tree:
TH1: User (người dùng) kết nối vào Switch (khoảng 6s là chuyển đèn từ Cam - Xanh lá)
-> Spanning Tree hỗ trợ cơ chế port-fast (chỉ tác dụng cho port MODE ACCESS)
Cách 1: vào từng interface hoặc interface range
Switch(config)#interface e0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#spanning-tree portfast (config thêm portfast để tăng tốc hội tụ Spanning Tree)

Cách 2: cấu hình port-fast tại mode Global config (chỉ tác động lên port Access, không tác động lên port Trunk).
Switch(config)#spanning-tree portfast default

TH2: Uplink kết nối đến Root Switch bị Fail
-> Spanning Tree hỗ trợ cơ chế Uplink-Fast, Backbone-Fast, ... (CCNP Switching)

PVST+: định nghĩa mỗi vlan sẽ có 1 spanning-tree khác nhau
--> để tận dụng tất cả các port đã block

---------------------------------------------------------------------------------------------------------------------
		DHCP
---------------------------------------------------------------------------------------------------------------------
Cấp IP 1 cách tự động
Tạo DHCP là định nghĩa 1 Pool
- Subnet cần để cấp
- Default-Gateway của subnet cần để cấp
- Option: DNS 

DHCP gồm 4 gói:
- DHCP Discovery (Broadcast): thiết bị mạng của người dùng tìm DHCP Server
- DHCP Offer (Unicast): từ DHCP Server gửi thông tin của DHCP Server về cho người dùng.
- DHCP Request (Unicast): request xin IP đến DHCP Server.
- DHCP Acknownledge (Unicast): cấp IP cho người dùng.

D O R A
>>>>>>> origin/v4
