Wireless
#############################################################################################################################
Physical:
- Wire (có dây): chuẩn cáp RJ45 (đồng - Ethernet-10M/ FastEthernet 100M/ 
GigaEthernet 1G/ TenGigaEthernet 10G), LC/SC/FC (quang)
Switch (cơ chế hoạt động là Full Duplex) 24 port thì chỉ có 24 kết nối. Kết nối thứ 25 là không thể
- Wireless (không dây): dưới dạng sóng (Wave), các dạng sóng khác nhau về tần số.
	+ Wifi (trong phạm vi hẹp)
	+ Wimax (Wifi phục vụ trong công cộng khi mà mình đang bên ngoài)
	+ 3G/4G/5G (sóng kết nối dựa trên sóng điện thoại).
	+ SateLite (sóng kết nối thông qua vệ tinh).
	+ Viba (dành cho những phạm vi không thể kéo cáp nhưng vẫn đảm bảo có internet cho khu vực đó).
Lưu ý: Satelite & Viba (2 chảo) là sóng dạng bắn tia không thuộc dạng sóng hình cầu.
 
------------------------------------------------------------------------------------------------------------------------------
Quyết định để kết nối được số lượng thiết bị thông qua sóng Wifi là số Session của thiết bị hỗ trợ.
(Số lượng Session của Wireless tương đương số lượng Port trên thiết bị Switch hoặc Hub).

- Wireless thì cơ chế hoạt động là đơn công (Half Duplex) - sẽ gồm 2 kênh truyền và nhận khác nhau.
Có công nghệ CSMA/CA - wireless để tránh xung đột.
- Sóng Wireless phạm vi phủ sóng (vùng phủ sóng) là hình cầu nên sẽ có bán kính phủ sóng.
Muốn kết nối được duy trì thì vùng phủ sóng giữa 2 thiết bị:
	+ Cùng NAME(SSID) và password
	+ Độ giao nhau giữa 2 vùng phủ sóng phải > 11% mới đảm bảo được kết nối là liên tục khi đi qua giữa các vùng sóng.
--> Đây là cơ chế Roaming.

Trong CCNA chỉ quan tâm của lớp Datalink (layer2) 802.11a/b/g/n/ac - MAC của sóng mang Wifi
Chuẩn 2.4G thì chỉ có 3 kênh (Channel) kết nối được: 1, 6 và 11.
--> 3 kênh này không được trùng lặp nhau (sẽ bị tình trạng loop sóng -> dẫn đến chết thiết bị).

Chuẩn 5G thì có khoảng 23 kênh (Channel) nên thường không bị trùng lặp kênh.

------------------------------------------------------------------------------------------------------------------------------
OSI (Physical - Datalink)
TCP/IP (Network Access bao gồm cả Physical - Datalink).

Bluetooth
HDMI không dây

SSID không sử dụng Preshare-key mà sử dụng xác thực thông qua tài khoản (local/Active Directory/Radius/Tacac+).
thì trên thiết bị Wifi phải bật 802.1x (đăng nhập bao gồm username/password)
Đối với Local thì được tạo tài khoản trên thiết bị.
Đối với Active Directory/Radius/Tacac+ thì phải kết nối LDAP thiết bị Wifi với AD thì mới có thông tin username/password của AD để xác thực vào mạng.

------------------------------------------------------------------------------------------------------------------------------
Wifi Mesh thì sẽ có 1 thiết bị Wifi Main đóng vai trò như 1 Wifi Core (Controller) kết nối không dây các thiết bị Wifi còn lại.
-> dùng cho phạm vi rộng và việc kết nối cáp cho thiết bị Wifi dường như không thể 
(phù hợp cho những nơi không gian và trần rộng nhưng vẫn đảm bảo được mỹ quan và kết nối internet).

Khi thao tác cấu hình trên thiết bị Wifi thì phải vào từng thiết bị để cấu hình (rất tốn thời gian và dễ phát sinh ra lỗi).
-> Controller ra đời giải quyết vấn đề kết nối vào nhiều thiết bị.

Các thiết bị Wifi sẽ kết nối vào Controller và chỉ cấu hình 1 lần trên Controller sẽ Apply toàn bộ lên các thiết bị Wifi.
(Controller nhìn vẻ ngoài giống Switch).
