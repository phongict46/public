<<<<<<< HEAD
				Redundant Networking (dự phòng)
#########################################################################################################################################
1. Dự phòng Layer 1 (cable): Dự phòng Layer1 - dự phòng về cáp (gôm nhiều dây Physic lại thành 1 dây Logic)
ETHERCHANNEL (Cisco)/Port Group (Standard) là 1 tính năng bó/gôm nhiều dây Physical thành 1 dây Logical
- Tăng tính dự phòng về cáp
- Tăng băng thông
VD: gôm 4 dây trunk Physical lại thành 1 dây trunk Logical
4 dây đều 1Gb -> sau khi gôm lại thành 1 dây Logical sẽ có BW = 4Gb
--> Cisco hỗ trợ gôm tối đa 8 port Physical -> 1 port Logical

---------------------------------------------------------------------------------------------------------------------------------------
Etherchannel là 1 tính năng dự phòng cho kết nối dây cáp của Cisco (protocol là PaGP)
	- Desirable 
	- Auto
Port Group là 1 tính năng dự phòng cho kết nối dây cáp của chuẩn Standard (protocol là LaCP)
	- Active
	- Passive

---------------------------------------------------------------------------------------------------------------------------------------
So sánh về tính chất của port Physical và port Channel/ port Group
1 port 10Gb gôm 8 port 1Gb ~ 10Gb -> không thể truyền được nhiều đối với các dữ liệu có dung lượng lớn do BW mỗi dây chỉ 1Gb nhưng có tính dự phòng cao.
1 port Physical 10Gb và 1 port Etherchannel 10Gb -> truyền được nhiều đối với các dữ liệu có dung lượng lớn do BW trên dây là 10Gb  nhưng không có tính dự phòng.
-> Về tính chất thì 1 port Physical sẽ không bị lỗi dù cho kết nối trên bất kỳ thiết bị hãng nào đi nữa nhưng không có chức năng dự phòng.
-> Port Channel/ Port Group thì được tính dự phòng nhưng vẫn sẽ bị lỗi Bug trên OS do bản chất port channel chỉ là port logical được quy định bởi OS của hãng build ra.

---------------------------------------------------------------------------------------------------------------------------------------
Configuration:
	SW1 (e0/0, e0/1) ==================== (e0/0, e0/1) SW2 

1. LaCP
SW1(config)# interface range e0/0, e0/1
SW1(config-if-range)# channel-protocol lacp
SW1(config-if-range)# channel-group 1 mode active
LACP:
SW2(config)# interface range e0/0, e0/1
SW2(config-if-range)# channel-protocol lacp
SW2(config-if-range)# channel-group 2 mode passive
--> Cấu hình mang tính minh họa, về thực tế nên cấu hình 2 đầu Active.

2. PaGP
SW1(config)# interface range e0/0, e0/1
SW1(config-if-range)# channel-protocol pagp
SW1(config-if-range)# channel-group 1 mode desirable
LACP:
SW2(config)# interface range e0/0, e0/1
SW2(config-if-range)# channel-protocol pagp
SW2(config-if-range)# channel-group 2 mode auto
--> Cấu hình mang tính minh họa, về thực tế nên cấu hình 2 đầu Desirable.

#########################################################################################################################################
Dự phòng Layer2: dự phòng về Switch (gôm nhiều Switch vật lý thành 1 Switch luận lý)
StackWise: gôm nhiều SW vật lý (Physical)-> 1 SW luận lý (Logical)
-> dùng để dự phòng về thiết bị SW
SW hỗ trợ StackWise là Switch Layer 3, hoặc các dòng SWitch Layer 2 có Module mở rộng gắn Module Stackwise
Hỗ trợ Stackwise tối đa 8 Switch 

----------------------------------------------------------------------------------------------------------------------------------------
Thực tế StackWise chia làm 2 dạng:
TH1: mở rộng port
Các SW không nhất thiết phải cùng số port và cùng cấu hình.
--> thường dành cho Switch thuộc lớp Access
TH2: dự phòng thường chỉ có 2 SW 
SW1 cấu hình như thế nào thì SW2 cấu hình ih chang
Các SW phải cùng số port và cùng cấu hình.
--> thường dành cho Switch thuộc lớp Distribution và Core

----------------------------------------------------------------------------------------------------------------------------------------
Quá trình bầu cử Active Switch
Các stack member đều có thể là active switch hoặc standby switch. Nếu như active switch bị lỗi, standby switch sẽ được đưa lên là active switch. 
Quá trình bầu chọn stack switch master dựa vào các tiêu chí sau và theo thứ tự ưu tiên từ trên xuống:
	- Switch đang là switch Master trong stack
	- Switch member với chỉ số priority cao nhất (Chỉ số priority này có giá trị từ 1 đến 15. Giá trị priority mặc định là 1)
	- Switch có thời gian khởi động ngắn nhất
	- Switch có địa chỉ MAC thấp nhất

Quá trình bầu chọn lại xảy ra khi trong hệ thống có 1 trong các nguyên nhân sau:
	- Stack switch được reset lại.
	- Active switch lỗi, reset, tắt nguồn hoặc được tháo ra khỏi stack
	- Thêm member vào stack

----------------------------------------------------------------------------------------------------------------------------------------
Configuration:
1. Thiết lập chỉ số priority bằng lệnh: switch <stack-member-number> priority <new-priority-number> 
Switch# switch 1 priority 15

----------------------------------------------------------------------------------------------------------------------------------------
2. Xóa bỏ thông tin switch vừa tháo ra khỏi stack. Câu lệnh:  no switch <stack-member-number> provision
Switch(config)# no switch 3 provision
----------------------------------------------------------------------------------------------------------------------------------------

3. Disable và enable 1 stack port: switch <stack-member-number> stack port <port-number> disable
Switch# switch 2 stack port 1 disable
----------------------------------------------------------------------------------------------------------------------------------------

4. Enable lại stack port: switch <stack-member-number> stack port <port-number> enable
Switch# switch 2 stack port 1 enable
----------------------------------------------------------------------------------------------------------------------------------------

5. Thay đổi number mới cho 1 switch bằng lệnh: switch <current-stack-member-number> renumber <new-stack-member-number>
Switch# switch 2 renumber 5
=======
				Redundant Networking (dự phòng)
#########################################################################################################################################
1. Dự phòng Layer 1 (cable): Dự phòng Layer1 - dự phòng về cáp (gôm nhiều dây Physic lại thành 1 dây Logic)
ETHERCHANNEL (Cisco)/Port Group (Standard) là 1 tính năng bó/gôm nhiều dây Physical thành 1 dây Logical
- Tăng tính dự phòng về cáp
- Tăng băng thông
VD: gôm 4 dây trunk Physical lại thành 1 dây trunk Logical
4 dây đều 1Gb -> sau khi gôm lại thành 1 dây Logical sẽ có BW = 4Gb
--> Cisco hỗ trợ gôm tối đa 8 port Physical -> 1 port Logical

---------------------------------------------------------------------------------------------------------------------------------------
Etherchannel là 1 tính năng dự phòng cho kết nối dây cáp của Cisco (protocol là PaGP)
	- Desirable 
	- Auto
Port Group là 1 tính năng dự phòng cho kết nối dây cáp của chuẩn Standard (protocol là LaCP)
	- Active
	- Passive

---------------------------------------------------------------------------------------------------------------------------------------
So sánh về tính chất của port Physical và port Channel/ port Group
1 port 10Gb gôm 8 port 1Gb ~ 10Gb -> không thể truyền được nhiều đối với các dữ liệu có dung lượng lớn do BW mỗi dây chỉ 1Gb nhưng có tính dự phòng cao.
1 port Physical 10Gb và 1 port Etherchannel 10Gb -> truyền được nhiều đối với các dữ liệu có dung lượng lớn do BW trên dây là 10Gb  nhưng không có tính dự phòng.
-> Về tính chất thì 1 port Physical sẽ không bị lỗi dù cho kết nối trên bất kỳ thiết bị hãng nào đi nữa nhưng không có chức năng dự phòng.
-> Port Channel/ Port Group thì được tính dự phòng nhưng vẫn sẽ bị lỗi Bug trên OS do bản chất port channel chỉ là port logical được quy định bởi OS của hãng build ra.

---------------------------------------------------------------------------------------------------------------------------------------
Configuration:
	SW1 (e0/0, e0/1) ==================== (e0/0, e0/1) SW2 

1. LaCP
SW1(config)# interface range e0/0, e0/1
SW1(config-if-range)# channel-protocol lacp
SW1(config-if-range)# channel-group 1 mode active
LACP:
SW2(config)# interface range e0/0, e0/1
SW2(config-if-range)# channel-protocol lacp
SW2(config-if-range)# channel-group 2 mode passive
--> Cấu hình mang tính minh họa, về thực tế nên cấu hình 2 đầu Active.

2. PaGP
SW1(config)# interface range e0/0, e0/1
SW1(config-if-range)# channel-protocol pagp
SW1(config-if-range)# channel-group 1 mode desirable
LACP:
SW2(config)# interface range e0/0, e0/1
SW2(config-if-range)# channel-protocol pagp
SW2(config-if-range)# channel-group 2 mode auto
--> Cấu hình mang tính minh họa, về thực tế nên cấu hình 2 đầu Desirable.

#########################################################################################################################################
Dự phòng Layer2: dự phòng về Switch (gôm nhiều Switch vật lý thành 1 Switch luận lý)
StackWise: gôm nhiều SW vật lý (Physical)-> 1 SW luận lý (Logical)
-> dùng để dự phòng về thiết bị SW
SW hỗ trợ StackWise là Switch Layer 3, hoặc các dòng SWitch Layer 2 có Module mở rộng gắn Module Stackwise
Hỗ trợ Stackwise tối đa 8 Switch 

----------------------------------------------------------------------------------------------------------------------------------------
Thực tế StackWise chia làm 2 dạng:
TH1: mở rộng port
Các SW không nhất thiết phải cùng số port và cùng cấu hình.
--> thường dành cho Switch thuộc lớp Access
TH2: dự phòng thường chỉ có 2 SW 
SW1 cấu hình như thế nào thì SW2 cấu hình ih chang
Các SW phải cùng số port và cùng cấu hình.
--> thường dành cho Switch thuộc lớp Distribution và Core

----------------------------------------------------------------------------------------------------------------------------------------
Quá trình bầu cử Active Switch
Các stack member đều có thể là active switch hoặc standby switch. Nếu như active switch bị lỗi, standby switch sẽ được đưa lên là active switch. 
Quá trình bầu chọn stack switch master dựa vào các tiêu chí sau và theo thứ tự ưu tiên từ trên xuống:
	- Switch đang là switch Master trong stack
	- Switch member với chỉ số priority cao nhất (Chỉ số priority này có giá trị từ 1 đến 15. Giá trị priority mặc định là 1)
	- Switch có thời gian khởi động ngắn nhất
	- Switch có địa chỉ MAC thấp nhất

Quá trình bầu chọn lại xảy ra khi trong hệ thống có 1 trong các nguyên nhân sau:
	- Stack switch được reset lại.
	- Active switch lỗi, reset, tắt nguồn hoặc được tháo ra khỏi stack
	- Thêm member vào stack

----------------------------------------------------------------------------------------------------------------------------------------
Configuration:
1. Thiết lập chỉ số priority bằng lệnh: switch <stack-member-number> priority <new-priority-number> 
Switch# switch 1 priority 15

----------------------------------------------------------------------------------------------------------------------------------------
2. Xóa bỏ thông tin switch vừa tháo ra khỏi stack. Câu lệnh:  no switch <stack-member-number> provision
Switch(config)# no switch 3 provision
----------------------------------------------------------------------------------------------------------------------------------------

3. Disable và enable 1 stack port: switch <stack-member-number> stack port <port-number> disable
Switch# switch 2 stack port 1 disable
----------------------------------------------------------------------------------------------------------------------------------------

4. Enable lại stack port: switch <stack-member-number> stack port <port-number> enable
Switch# switch 2 stack port 1 enable
----------------------------------------------------------------------------------------------------------------------------------------

5. Thay đổi number mới cho 1 switch bằng lệnh: switch <current-stack-member-number> renumber <new-stack-member-number>
Switch# switch 2 renumber 5
>>>>>>> origin/v4
