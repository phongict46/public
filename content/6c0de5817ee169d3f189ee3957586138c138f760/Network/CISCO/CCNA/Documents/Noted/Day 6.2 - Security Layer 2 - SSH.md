########################################################################################################################
Tấn công trong Hạ tầng Layer 2: 
1. Tấn công tràn bảng MAC -> biến SW thành Hub
2. VLAN Hopping -> biến port đấu nối thành port trunk và tận dụng VLAN Native để truy cập trong mạng.
3. Tấn công STP -> làm thay đổi Root Switch trong mạng bằng cách SW của người tấn công là Root Switch.
4. Giả mạo địa chỉ MAC và giả mạo DHCP

########################################################################################################################
1. Tấn công tràn bảng MAC (bảng CAM): biến Switch thành Hub
--> Giới hạn tốc độ của port Access
--> Port Security cho port Access

Các thành phần của port Security
a. Học MAC:
- Static (Switch reset không bị mất thông tin MAC trên port)
- Dynamic (default) -> mất thông tin MAC trên port khi Switch reset
- Sticky (Switch reset không bị mất thông tin MAC trên port)

b. Tác động khi bị vi phạm (violation):
- Protect:
- Restrict: gửi thông tin nhưng không Block port(err-disable)
- Shutdown: gửi thông tin, Block port (err-disable)


c. Chế độ mặc định (default) bật port security:
- MAC maximun = 1
- Học Mac = dynamic
- Violation = Shutdown
Để bật lại port khi (err-disable) thì SHUTDOWN trước rồi NO SHUTDOWN sau
-------------------------------------------------------------------------------------------------------------------------
Switch(config)#int e0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan vlan-id
Switch(config-if)#spanning-tree portfast

-------------------------------------------------------------------------------------------------------------------------
switchport port-security
--> Optional for port security:
switchport port-security violation [protect/restrict/shutdown(default)]
switchport port-security mac-address AAAA.BBBB.CCCC (Static)
switchport port-security mac-address sticky (học xong và saved lại)
switchport port-security mac-address dynamic (default)
switchport port-security maximum 5 (số MAC tối đa được phép trên 1 port Access)

########################################################################################################################
2. VLAN Hopping
VLAN Native là VLAN không bị tag trên đường trunk
--> có thể đi được qua tất cả các Switch
Vlan Native là VLAN 1 (đối với Switch Cisco)

Note: mục tiêu là để scan quét trong mạng để tìm server, dịch vụ của công ty để tấn công
Dùng VLAN Native để đi được đến tất cả VLAN.
--> Chuyển đổi port mạng của hacker sang port Trunk

Trunk - Auto = Trunk (luôn allow VLAN Native đi qua nó mà không cần tag)
Giải quyết:
B1. Chuyển VLAN Native trên các port Trunk (tất cả port trunk trong mạng)
Switch(config-if)#switchport trunk allow native 555

Switch1 (VLAN Native 555) -----trunk----- Switch2 (VLAN Native 444)
-> missmatch VLAN native thì sẽ giữ VLAN 1 làm VLAN Native
B2. Chuyển tất cả các port kết nối end user thành port Access
Access - Trunk = N/A
B3. Nên shutdown các port không có kết nối tới end user

########################################################################################################################
3. Chống tấn công cây STP: spanning-tree bpduguard enable (cấm gửi BPDU từ port Access, chỉ nhận BPDU từ Root Switch)
Spanning Tree manipulation: làm thay đổi cây Spanning Tree
- Gắn 1 Switch vào trong port của người dùng và thay đổi nó thành Root Switch
--> thay đổi cây Spanning Tree để all traffic đổ dồn về con Root Switch giả (capture thông tin trong mạng)
Giải quyết:
- Bật BPDU Gaurd (Root Switch là Switch gửi BPDU và các Switch khác chỉ nhận BPDU từ Root Switch)

Swich(config-if)#switchport mode access
Swich(config-if)#switchport access vlan vlan-id
Swich(config-if)#spanning-tree portfast
Swich(config-if)#spanning-tree bpduguard enable

########################################################################################################################
4. Chống tấn công giả mạo DHCP Server: cấm đặt IP tĩnh trên port Access
- Trusted port Trunk
- Untrusted port Access
Swich(config-if)#ip dhcp snooping trusted --> trunk
Swich(config-if)#ip dhcp snooping untrusted --> access

-------------------------------------------------------------------------------------------------------------------------
Note: tất cả các port vi phạm chính sách trên sẽ rơi vào trạng thái Err-Disabled.
-------------------------------------------------------------------------------------------------------------------------


########################################################################################################################
						SSH 
########################################################################################################################
Telnet thì phiên remote sẽ không có mã hoá (capture được thì sẽ thấy toàn bộ quá trình thao tác).
SSH thì sẽ có mã hoá trên phiên remote (sử dụng key)
Telnet khác SSH ở Key để mã hoá
Local Authentication: tạo username/password trên thiết bị SWitch hoặc Router.

-------------------------------------------------------------------------------------------------------------------------
Router(config)#ip domain-name waren.vn
Router(config)#ip ssh version 2
Router(config)#username admin password cisco
Router(config)#crypto key generate rsa 
How many bits in the modulus [512]: 1024  <-- nhập 1024 trở lên để sử dụng ssh version 2

Router(config)#line vty 0 4
Router(config-line)#login local
Router(config-line)#transport input ssh


