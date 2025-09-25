172.16.0.0/16
...
172.31.0.0/16
-------------
172.16.0.0/12

IP Address:  192.168.1.20
Subnet Mask: 255.255.255.0
Default Gate:192.168.1.1 

Data Link: cách nhìn về chuyển mạch
Mở rộng hạ tầng mạng

Thiết bị Hub ra đời dùng để giải quyết vấn đề về kết nối trong hạ tầng mạng.
Hub chỉ thuần là thiết bị kết nối (không có khả năng chuyển mạch).
Tất cả các thiết bị khi kết nối vào Hub sẽ Broadcast ra toàn mạng khi gửi thông tin.
Ethernet đầu tiên ra đời chỉ có BW-10Mb

Nhược điểm:
- Gửi thông tin dạng Broadcast nên tiêu tốn rất nhiều BW.
- Bị xung đột khi có từ 2 user trở lên gửi thông tin cùng 1 lúc (đơn công - HalfDuplex)
--> Drop toàn bộ thông tin.

Hub 4 port (layer 1):
- 1 vùng Broadcast Domain
- 1 vùng Collision Domain (vùng xung đột)

Giải quyết vấn đề xung đột cho HUB thì có cơ chế CSMA/CD: dùng timer để tạo thông tin gửi dữ liệu
--> để check xem đường truyền có rãnh không (theo luật FIFO - First IN First OUT)

Switch: dùng để giải quyết vấn đề về xung đột.
1. Học MAC:
- Mỗi port của Switch có 1 địa chỉ MAC (dựa trên tính năng học địa chỉ MAC trên port).
- Mỗi port của Switch là 1 vùng Collision Domain.
Sau khi học xong MAC thì việc chuyển mạch theo cơ chế Unicast (1 - to - 1)

2. Switch là 1 thiết bị song công (Full Duplex) nên có thể truyền và nhận cùng 1 lúc được
-> không có xung đột trên đường truyền.

Switch khi chưa học MAC thì sẽ cơ chế giống HUB -> Broadcast ra toàn mạng để học MAC vào port.

ARP: dùng để học MAC từ địa chỉ IP (chuyển đổi IP -> MAC)
Khi chưa học MAC thì Switch sẽ gửi MAC là FFFF-FFFF-FFFF ra toàn bộ mạng để tìm thông tin MAC.
-> Unknown Unicast (Flood toàn bộ ra tất cả port - trừ port chính nó).

Switch 4 port (layer 2):
- 1 vùng Broadcast Domain
- 4 vùng Collision Domain (vùng xung đột)

RARP: Reverse ARP (chuyển đổi MAC -> IP)

Router: mỗi port của Router là 1 lớp mạng
- Mỗi port của Router sẽ là 1 Broadcast Domain và 1 Collision Domain.

Router 4 port (Layer 3):
- 4 vùng Broadcast Domain.
- 4 vùng Collision Domain.

Trong toàn bộ tiến trình ARP thì chỉ có MAC thay đổi, còn IP thì không đổi.











