<<<<<<< HEAD
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
					HSRP
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
Dự phòng Layer 1: dự phòng về cáp Ethernet
Etherchannel (Cisco)
PortGroup (Standard)
Dự phòng tối đa 8 port trong 1 channel.
--> sẽ tác động lên các port (vào interface để cấu hình)
--> các port có cùng cấu hình (nên config bằng interface range)
PAgP (Cisco): Desirable/Auto 
LAcP (Standard): Active/Passive
2 đầu thiết bị không nhất thiết phải cùng số của Channel
Channel 1 -------- Channel 2
Lưu ý: nên quy hoạch Channel trên thiết bị khi có nhiều kết nối Etherchannel 
trên 1 thiết bị.
--> Nên join các port vào Channel trước rồi sau đó vào interface port-channel để cấu hình
#######################################################################################################################
Dự phòng Layer 2: dự phòng về Switch
- Mở rộng port: các Switch không nhất thiết phải giống nhau về số port và giống nhau về mặt thiết bị
(miễn là các thiết bị Switch đó có hỗ trợ Stack) - tối đa 8 Switch
- Backup (thường 2 thiết bị Stack lại với nhau để Backup) - yêu cầu phải giống nhau về số port, giống nhau về mặt thiết bị
giống nhau về config.
--> Khi đấu nối vào Switch Stack để Backup thì phải đấu cáp vào cả 2 Switch
(thường cho Switch-Core - Hybrid, từ Switch-Distribution và Switch-Core cho hạ tầng 3 lớp).

#######################################################################################################################
Dự phòng layer 3:
- Phải có 1 địa chỉ GW đại diện cho các Router tham gia vào dự phòng.
- Phải có Router Main/Active, Router Backup/Standby
- Cơ chế giựt quyền khi thiết bị Router Main được phục hồi (chỉ cấu hình trên Router Main/Active)

---------------------------------------------------------------------------------------------------------------------
FHRP nói chung cho tất cả thiết bị dự phòng Layer3 (trong đó có 3 giao thức cho FHRP)
1. HSRP (Cisco): chỉ được 2 thiết bị tham gia vào Main - Standby (chỉ 1 thiết bị Main hoạt động)
2. VRRP (Standard): 1 Main - n Standby (tối đa n = 16) (chỉ 1 thiết bị Main hoạt động)
3. GLBP (Cisco - chỉ dành cho thiết bị có hỗ trợ VSS): cả 2 thiết bị đều chạy đồng thời để share tải

---------------------------------------------------------------------------------------------------------------------
Khi Router ACtive bị Down thì Router Standby sẽ thay thế 
-> vô tình bị problem Gateway bị thay đổi trên DHCP
default-router 192.168.1.1 (Router Active)
Khi Router ACtive down phải vào DHCP thay đổi lại defaul-router sang Router Standby 192.168.1.2
FHRP là 1 cơ chế High Available (HA) thì khi bật tính năng này, 2 Router vật lý sẽ được đại diện
bằng 1 Router luận lý (Logical) với 1 IP đại diện làm GW cho toàn bộ mạng.
-> Default-router trên DHCP sẽ trỏ về IP đại diện của 2 Router tham gia vào quá trình HA.
KHi cấu hình chỉ nên cấu hình dựa trên ngữ cảnh.
- Phân biệt được Active & Standby (Priority Router nào lớn hơn sẽ ưu tiên hơn)
- Đặt IP đại diện cho Router HA
- Cơ chế giựt quyền Active (Pre-empt)

=======
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
					HSRP
---------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------
Dự phòng Layer 1: dự phòng về cáp Ethernet
Etherchannel (Cisco)
PortGroup (Standard)
Dự phòng tối đa 8 port trong 1 channel.
--> sẽ tác động lên các port (vào interface để cấu hình)
--> các port có cùng cấu hình (nên config bằng interface range)
PAgP (Cisco): Desirable/Auto 
LAcP (Standard): Active/Passive
2 đầu thiết bị không nhất thiết phải cùng số của Channel
Channel 1 -------- Channel 2
Lưu ý: nên quy hoạch Channel trên thiết bị khi có nhiều kết nối Etherchannel 
trên 1 thiết bị.
--> Nên join các port vào Channel trước rồi sau đó vào interface port-channel để cấu hình
#######################################################################################################################
Dự phòng Layer 2: dự phòng về Switch
- Mở rộng port: các Switch không nhất thiết phải giống nhau về số port và giống nhau về mặt thiết bị
(miễn là các thiết bị Switch đó có hỗ trợ Stack) - tối đa 8 Switch
- Backup (thường 2 thiết bị Stack lại với nhau để Backup) - yêu cầu phải giống nhau về số port, giống nhau về mặt thiết bị
giống nhau về config.
--> Khi đấu nối vào Switch Stack để Backup thì phải đấu cáp vào cả 2 Switch
(thường cho Switch-Core - Hybrid, từ Switch-Distribution và Switch-Core cho hạ tầng 3 lớp).

#######################################################################################################################
Dự phòng layer 3:
- Phải có 1 địa chỉ GW đại diện cho các Router tham gia vào dự phòng.
- Phải có Router Main/Active, Router Backup/Standby
- Cơ chế giựt quyền khi thiết bị Router Main được phục hồi (chỉ cấu hình trên Router Main/Active)

---------------------------------------------------------------------------------------------------------------------
FHRP nói chung cho tất cả thiết bị dự phòng Layer3 (trong đó có 3 giao thức cho FHRP)
1. HSRP (Cisco): chỉ được 2 thiết bị tham gia vào Main - Standby (chỉ 1 thiết bị Main hoạt động)
2. VRRP (Standard): 1 Main - n Standby (tối đa n = 16) (chỉ 1 thiết bị Main hoạt động)
3. GLBP (Cisco - chỉ dành cho thiết bị có hỗ trợ VSS): cả 2 thiết bị đều chạy đồng thời để share tải

---------------------------------------------------------------------------------------------------------------------
Khi Router ACtive bị Down thì Router Standby sẽ thay thế 
-> vô tình bị problem Gateway bị thay đổi trên DHCP
default-router 192.168.1.1 (Router Active)
Khi Router ACtive down phải vào DHCP thay đổi lại defaul-router sang Router Standby 192.168.1.2
FHRP là 1 cơ chế High Available (HA) thì khi bật tính năng này, 2 Router vật lý sẽ được đại diện
bằng 1 Router luận lý (Logical) với 1 IP đại diện làm GW cho toàn bộ mạng.
-> Default-router trên DHCP sẽ trỏ về IP đại diện của 2 Router tham gia vào quá trình HA.
KHi cấu hình chỉ nên cấu hình dựa trên ngữ cảnh.
- Phân biệt được Active & Standby (Priority Router nào lớn hơn sẽ ưu tiên hơn)
- Đặt IP đại diện cho Router HA
- Cơ chế giựt quyền Active (Pre-empt)

>>>>>>> origin/v4
