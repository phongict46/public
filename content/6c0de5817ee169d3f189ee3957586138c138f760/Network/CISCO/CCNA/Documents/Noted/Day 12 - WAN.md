WAN: là sự kết nối giữa các Site với nhau (trong 1 công ty)
Internet: là sự kết nối vĩ mô hơn của WAN là kết nối giữa các công ty với nhau
- Đối với các công ty đủ năng lực về mặt hạ tầng cơ sở (1 hệ thống tự trị) sẽ tự định tuyến 
ra ngoài internet một cách trực tiếp (họ sử dụng IP Public 1 cách trực tiếp) để trở thành 1 AS
-> đủ năng lực về thiết bị, quy hoạch, nhân lực và tự quản lý Routing của mình trong Internet toàn cầu.

- Đối với các công ty chưa đủ về mặt năng lực, quy hoạch, nhân lực và sự quản lý thì thường thuê lại
hạ tầng mạng của các công ty đã trở thành AS.

Các dịch vụ Internet/WAN ISP cung cấp:
Internet: 3 loại dịch vụ 
- FTTH: internet băng thông rộng (dùng cho hộ gia đình, các công ty nhỏ).
VD: Mua gói cước FTTH 330k/tháng 40Mb(BW Trong nước, BW quốc tế = ? thường là nhỏ hơn 2Mb)

- ILL (Internet Lead Line): kênh thuê riêng, BW trong nước & BW quốc tế được đảm bảo (dù cho có đứt cáp biển
vẫn đảm bảo đúng BW) -> các bạn có thể liên hệ trực tiếp với Sale cung cấp để hỗ trợ khi có vấn đề xảy ra.
VD: Mua ILL-FPT giá 18tr/tháng (200Mb BW trong nước - 10Mb BW quốc tế).
	Truy cập bị chậm, apple.com truy cập 220ms -> yêu cầu để tối ưu cho nhanh hơn
	Sau khi tối ưu truy cập apple.com còn 100ms
- BGP connected: chỉ xài khi công ty của bạn là AS.

WAN: 2 loại dịch vụ
- Metronet-Layer2/MPLS-Layer2/L2VPN: thì 2 đầu ở 2 site cùng Subnet
- Metronet-Layer3/MPLS-Layer3/L3VPN: thì 2 đầu ở 2 site khác Subnet


1 con SW 1U thường tối đa 48 port - không thể đấu nối cho 48 khách hàng với BW 40Mb/KH
Đối với đường truyền Internet thì sẽ thông qua bộ chia.

Gói FTTH 2010 giá 220k-8Mb: port 1GB đi qua bộ chia 128, sẽ được 128 KH với BW 8Mb 
-> giá thành: 220 x 128 = 28.160.000/tháng/1 port
	SW48port: 28.160.000 x 48 = 1.351.680.000/tháng/SW-48port
Các bộ chia nằm ở tập điểm (tủ bên ngoài đóng khung sắt như cái trụ)

thay SW-48port 1GB thành SW-48port 2.5Gb thì BW tăng lên từ 8Mb -> 20Mb

1 SW-48port 10GB qua bộ chia 256 
1 port 256 KH
48 port ~ 256 x 46 ~ 12.000 KH
-> 1 SW sẽ có 2 port gắn lên 2 port Router 48port
-> 1 Router sẽ có = 12.000 x 24 = 288.000
Gói FTTH 2010 giá 150k-4Mb: port 1GB đi qua bộ chia 2 sau đó qua bộ chia 128, sẽ được 256 KH với BW 4Mb

#########################################################################################################################
PPP có 2 kiểu xác thực:
- PAP: không có mã hoá
- CHAP: có mã hoá bằng thuật toán MD5 (băm uesrname và password bằng thuật toán MD5
sau đó so sánh chuỗi MD5 ở 2 đầu)

Giao thức xác thực cho tài khoản FTTH - PPPoE sẽ khai báo trên cổng ảo interface dialer
- Do interface của Router là Layer 3 (còn giao thức PPPoE là Layer 2 nên không thể
cấu hình trực tiếp trên interface của Router)

Interface Dialer 0 (chứa thông tin của giao thức PPPoE) -> 
là interface Layer 2 sử dụng Protocol PPP để chạy cho giao thức xác thực internet là PPPoE

- Đối với giao thức PPPoE khi NAT thì sẽ vào interface dialer để gán ip nat outside


VPN: đem đến giải pháp kết nối mạng Private trên hạ tầng Internet
- Metronet (L2VPN/L3VPN) rất đắc đỏ để kết nối Private giữa 2 nơi xa.
Ưu điểm của Metronet: độ tin cậy cao, không rớt gói và như dây cáp kết nối 2 đầu 2 site ở xa.

- VPN (chạy trên hạ tầng Internet):
Đáp ứng: 2 đầu phải có IP tĩnh.
2 đầu có IP Public và ping được tới nhau -> khi đó tạo tunnel mới kết nối được giữa 2 sites.
Ưu điểm: không phát sinh thêm chi phí về kết nối Private giữa 2 sites.

VPN trong chương trình CCNA là VPN Site-to-Site dùng giao thức GRE Tunnel.

VPN: có 2 dạng chính
- Site với Site:
	Site-to-Site (1-to-1)
	Site-to-MultiSite (1-to-n)
- Remote VPN:
	dùng phần mềm VPN để kết nối về Router/FW
Cisco (anyconnect)
Palo Alto (Global Protect)

