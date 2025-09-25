Routing - Static Route

Routing: kỹ thuật định tuyến để các lớp mạng có thể tương tác được với nhau.
(mang tính chất như là người dẫn đường và Gateway sẽ đóng vai trò là người chỉ dẫn).
- Nếu các thiết bị cùng 1 LAN (cùng 1 subnet) thì sẽ tương tác trực tiếp với nhau không cần thông qua Gateway.
- Nếu các thiết bị khác LAN (khác subnet) khi muốn tương tác sẽ gửi lên Gateway để dẫn đường.
Gateway sẽ dựa vào thông tin bảng định tuyến để dẫn đường cho Traffic (Bảng định tuyến trên Router được gọi là
Routing Table).

Các thông số trong bảng định tuyến.
Chỉ số AD (Administrative Distance): là chỉ số khoảng cách định tuyến -> càng nhỏ sẽ càng được ưu tiên.
- Kết nối trực tiếp - Connected Direct(các thiết bị được kết nối cáp vật lý trực tiếp với nhau) AD = 0 (tốt nhất)
- Định tuyến tĩnh - Static Route: là kỹ thuật định tuyến được dẫn dắt bởi người quản trị.

Static Route:
	Ưu điểm:
- Dễ cấu hình
- Dễ quản lý
- Không tốn tài nguyên của thiết bị (do không dùng thuật toán để tính toán).
	Nhược điểm:
- Không phù hợp cho các hạ tầng mạng quá nhiều thiết bị.
- Khó quản lý thông tin cấu hình khi quá nhiều thiết bị tham gia vào mạng.
- Bị loop khi không quản lý tốt về cấu hình.

Note: HOP là Router ( đi qua 2 HOP là đi qua 2 Router)
Chỉ số AD của định tuyến càng nhỏ càng được ưu tiên.

Static Route - configuration gồm 2 phần: subnet-destination và IP của next-hop

Khi cấu hình xong, muốn xóa cấu hình thì thêm chữ NO phía trước câu lệnh cấu hình.
Đối với interface - muốn xóa toàn bộ cấu hình trong interface thì dùng câu lệnh.
Router(config)#default interface e0/0


Dự phòng cho Static khi có nhiều Next-hop đến cùng 1 destination:
--> Nâng chỉ số AD lên >1 trong câu lệnh cấu hình static route (Static route có AD default = 1)

Router(config)#ip route 192.168.2.0 255.255.255.0 192.168.12.2 5

Default Route: dùng để truy cập đên 1 host bất kỳ, thường là định tuyến ra môi trường internet.
Router(config)#ip route 0.0.0.0  0.0.0.0 <ip next-hop>

10.0.0.0 255.0.0.0 /8 -> (2^24 -2) host
128.0.0.0 128.0.0.0 /1 -> (2^31 -2) host


1000 0000.0.0.0 	Subnet mask của /1: 1000 0000.0.0.0

(2^32 - 2) host -> Subnet Mask: 0.0.0.0 (any - bất kỳ)







