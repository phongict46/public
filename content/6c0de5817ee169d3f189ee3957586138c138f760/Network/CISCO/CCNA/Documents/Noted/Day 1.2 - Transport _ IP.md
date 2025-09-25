#####################################################################################################################################	
				MÔ HÌNH OSI/TCP-IP
#####################################################################################################################################
Application: sự tương tác của người dùng với ứng dụng.

-----------------------------------------------------------------------------------------------------------------------------------
Transport: định nghĩa dịch vụ cho tầng ứng dụng.
Transport ở Layer 4 trong mô hình TCP-IP/OSI
Mỗi ứng dụng sẽ tương ứng với port trong lớp Transport
HTTP: port 80
HTTPs: port 443
DNS: port 53

---------------------------------------------------------------------------------------------------------------------------------------
Transport sẽ đảm bảo về mặt kết nối thông tin giữa các dịch vụ.
Dùng ứng dụng Outlook trên Desktop thì sẽ thuần sử dụng port 103 cho POP3 để nhận email
Dụng Web để check mail thì đầu tiên sẽ chạy port của ứng dụng HTTP/HTTPS để truy cập vào mail, sau đó mới nhận email (POP3)
Port dùng cho các ứng dụng quốc tế sẽ có Port trong khoảng: 1 - 1023
Port dùng cho các ứng dụng từ nhà sản xuất tự định danh: 1024 - 7999

---------------------------------------------------------------------------------------------------------------------------------------
Transport dùng để truyền tải thông tin từ Source -> Destination (tuỳ theo ứng dụng mà có thể dùng loại Transport
có đáp ứng về mặt toàn liệu dữ liệu hay không)

Trên 1 IP sẽ có 65535 port tương đương 2^16 port dành cho dịch vụ.
8.8.8.8:443 -> Server có IP 8.8.8.8 đang chạy dịch vụ HTTPs
8.8.8.8:103 -> Server có IP 8.8.8.8 đang chạy dịch vụ POP3 (get email)
8.8.8.8:22  -> Server có IP 8.8.8.8 đang chạy dịch vụ remote bằng SSH để cấu hình từ xa.
8.8.8.8:53  -> Server có IP 8.8.8.8 đang chạy dịch vụ DNS (dùng để phân giải tên miền Web).

---------------------------------------------------------------------------------------------------------------------------------------
Transport có thể truyền tải Multi Service trên cùng 1 IP
Transport có 2 dạng truyền tải dữ liệu cho đầu cuối:
- UDP: truyền tải dữ liệu không cần xác nhận ở đầu xa có nhận được dữ liệu hay không (nhận xong  -> truyền và không cần xác nhận đã nhận hay không)
--> Phù hợp cho các ứng dụng mang tính Real-Time (Streaming, Meeting, Video, Voice-IP, Gaming, ...)
--> Phù hợp cho Service khi tìm kiếm Server.
VD: Khi PC muốn truy cập google.com thì sẽ gửi gói UDP-53 (DNS-port 53/UDP) để tìm DNS Server.

---------------------------------------------------------------------------------------------------------------------------------------
- TCP: truyền tải dữ liệu cần đáp ứng độ tin cậy (nhận xong -> truyền cần có sự xác nhận từ đầu xa).
TCP đáp ứng độ tin cậy nên khi xảy ra lỗi sẽ truyền lại từ chỗ bị lỗi.
--> Phù hợp cho các ứng dụng đảm bảo độ toàn vẹn của dữ liệu (Truyền File/Download File, Email, HTTPS/HTTP, ...)
VD: PC muốn truy cập google.com sau khi Server đã nhận được DNS-port 53/UDP từ PC thì sẽ gửi lại thông tin Web & IP-Web bằng TCP-port 53
Và sau đó sẽ dùng TCP-port 443 để truy cập trang https://google.com
TCP sẽ dùng cơ chế bắt tay 3 bước (Three-way Shakehand) để đảm bảo việc truyền dữ liệu được toàn ven.

---------------------------------------------------------------------------------------------------------------------------------------
Ôn tập các hệ số
Thập phân: là hệ 10 nên sẽ có 0-9 -> 9 + 1 = 10
Bát phân: là hệ 8 nên sẽ có 0-7 -> 7 + 1 = 10
Thập lục phân: là hệ 16 sẽ có 0-F -> F(15) + 1 = 10
10 (Thập phân) = A (Thập lục phân)
11 = B
12 = C
13 = D
14 = E
15 = F
Nhị phân: là hệ 2 sẽ có 0-1 -> 1 + 1 = 10
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 0 (nhớ 1) = 10

---------------------------------------------------------------------------------------------------------------------------------------
Nhị phân là hệ số có 2 giá trị 0 và 1 (1 bit) hay còn gọi là 1 bit (0 và 1)
0 bit ~ 0 hoặc 1 (chỉ 1 giá trị) ~ 2^0 = 1 giá trị
2^1 = 2 giá trị
2^2 = 4 giá trị
...
Máy tính và công nghệ thông tin sẽ dùng số nhị phân để diễn đạt.

Chuỗi 8 bit: tổng giá trị của chuỗi 8 bit theo góc nhìn thập phân (0 - 255)
128	64	32	16	8	4	2	1
2^7	2^6	2^5	2^4	2^3	2^2	2^1	2^0

0000	0
0001	1
0010	2
0011	3
0100	4
0101	5
0110	6
0111	7

1000	8
1001	9	
1010	10 = A
1011	11 = B
1100	12 = C
1101	13 = D
1110	14 = E
1111	15 = F
Thập lục = 16 = 2^4 ~ 4 bits
---------------------------------------------------------------------------------------------------------------------------------------
Lưu ý:
---------------------------------------------------------------------------------------------------------------------------------------
Nhị phân khi tròn BIT sẽ có đủ giá trị theo hàm 2 mũ 
0 bit: 1 giá trị
0 hoặc 1

1 bit: 2 giá trị
0
1

2 bits: 4 giá trị
00
01
10
11

3 bits: 8 giá trị
000
001
010
011

100
101
110
111

4 bits: 16 giá trị (0 - 15)
0000
0001
0010
0011
0100
0101
0110
0111

1000
1001
1010
1011
1100
1101
1110
1111

1-15 (không đủ thành 4 bits)
---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
Layer 3: Network-OSI/Internet-TCP/IP
---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
- IP là 1 chuỗi 32bits được chia thành 4 phần: 
. Mỗi phần là 1 Octet, 
. Mỗi Octet có 8 bit
. Giá trị tổng 8 bit = 128 + 64 + 32 +16 + 8 + 4 + 2 + 1 = 255
VD: xác định IP nào là đúng:
11.128.137.253 - Đúng
254.237.258.125 - Sai do Octet thứ 3 là 258 
8.8.8.8 - Đúng
192.168.1.1 - Đúng
172.16.10.0 - Đúng
10.9.10.9 - Đúng

---------------------------------------------------------------------------------------------------------------------------------------
Phân tích IP thành nhị phân: 18.253.1.1
18|2
 0|9|2
   1|4|2
	 0|2|2
	   0|1
18 = 10010

128		64		32		16		8		4		2		1
2^7		2^6		2^5		2^4		2^3		2^2		2^1		2^0
 0		0		0		1		0		0		1		0
18 = 00010010

253|2
  1|126|2
      0|63|2
         1|31|2
	    1|15|2
	       1|7|2
	         1|3|2
		   1|1
253 = 1111 1101

128		64		32		16		8		4		2		1
2^7		2^6		2^5		2^4		2^3		2^2		2^1		2^0
 1       	1       	1       	1      		1       	1       	0       	1

253 = 1111 1101

18.253.1.1  = 0001 0010.1111 1101.0000 0001.0000 0001

192.168.1.1 = 1100 0000.1010 1000.0000 0001.0000 0001

128 = 1000 0000
192 = 1100 0000
224 = 1110 0000
240 = 1111 0000

---------------------------------------------------------------------------------------------------------------------------------------
IP địa chia thành các lớp sau:
Lớp A: 1 - 126 
Lớp 127 là Loopback, localhost không sử dụng
Lớp B: 128 - 191
Lớp C: 192 - 223
Lớp D: 224 - 239
Lớp E: 240 - 255
--> Chương trình CCNA chỉ sử dụng 3 lớp A,B,C

Địa chỉ IP Private: chỉ giao tiếp được trong Local Area Network (LAN) tách biệt hoàn toàn với môi trường Internet.
Địa chỉ IP Public: được IANA quy hoạch và khi muốn đăng ký phải thông qua đơn vị này.
Tại Việt Nam, nếu công ty muốn mua IPv4 Public và đăng ký thành 1 hệ thống AS thì chỉ cần đăng ký với VNIC

Các thiết bị mạng thì cần phải có IP để giao tiếp, IP Private sẽ được tạo ra để kết nối các thiết bị mạng
trong hạ tầng mạng Local.
IANA quy hoạch IP Private gồm các dãy sau:
IP Private lớp A: 10.0.0.0/8
IP Private lớp B: 172.16.0.0/16 - 172.31.0.0/16 -> 172.16.0.0/12
IP Private lớp C: 192.168.0.0/16

---------------------------------------------------------------------------------------------------------------------------------------
Subnet Mask:
/16, /8, /12 dùng để diễn tả lớp mạng/ nhận diện được lớp mạng
Subnet Mask để biểu diễn cho phần lớp mạng.
VD: 
192.168.1.1/24 và 192.168.1.1/25 là khác nhau hoàn toàn do 2 IP này dù giống nhau nhưng khác lớp mạng
192.168.1.1/24 và 192.168.1.2/26 không giao tiếp được với nhau do khác Subnet
192.168.1.1/24 và 192.168.1.2/24 thì giao tiếp được với nhau do cùng Subnet

Quy tắc tính Subnet Mask từ trái -> phải của theo số /Subnet giá trị bit = 1
128	64	32	16	8	4	2	1
 1 	1	1	1	1	1	1	1
 128 + 64  + 32 + 16 + 8 + 4 + 2 + 1 = 255
VD1: 192.168.1.1/24 
Subnet Mask: /24 từ trái sang phải của chuỗi 32 bit IP = 1 (có 24 bit bằng 1)
Sum 8 bits = 255
255.255.255.0

IP address: 192.168.1.1 
Subnet Mask: 255.255.255.0

VD2: 172.16.10.127/29
IP address: 172.16.10.127
Subnet Mask: 255.255.255.248

VD3: 10.10.0.1/17
IP address: 10.10.0.1
Subnet Mask: 255.255.128.0

VD4: 192.168.0.0/16 (SM có 16 bit từ trái sang phải = 1)
192	 .168	   .0	     .0
1111 1111.1111 1111.0000 0000.0000 0000

Số host có được từ Subnet Mask:
/16 -> 32 - 16 = 16 bits làm host

VD5: 192.168.1.0/24 
Subnet 192.168.1.0 có 32 - 24 = 8 bits làm host
2^8 = 256 host có thể có
IP khả dụng để có thể tương tác được với nhau (bỏ IP đầu (IP của lớp mạng) và IP cuối (IP Broadcast))
IP Net: 192.168.1.0 (IP đại diện cho lớp mạng)
IP Broadcast: 192.168.1.255 (IP dùng để tìm kiếm)

---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
Communication giữa các host trong hạ tầng mạng:
Unicast = chỉ 1 host (Focus vào 1)
Multicast = tương tác >= 2 (>1) (Focus nhưng lớn hơn 1)
Broadcast = tất cả (Không có Focus vào đối tượng nào cả, mục đích truyền đạt đến tất cả).

---------------------------------------------------------------------------------------------------------------------------------------
IP Public là phần IP còn lại trừ IP PRivate ra
VD: xác định các IP sau là Private hay Public
IP Private lớp A: 10.0.0.0/8 = 10.xxxx xxxx.xxxx xxxx.xxxx xxxx/8
( sẽ có 2^(32-8) - 2 = 2^24 -2 hosts
Subnet Mask 	 : 255.0.0.0
IP Net(x=0)	 : 10.0.0.0
IP hosts	 : 10.0.0.1 - 10.255.255.254 = 2^24 -2 hosts
IP Broadcast(x=1): 10.255.255.255
	
IP Private lớp B : 172.16.0.0/16 		- 172.31.0.0/16 		-> 172.16.0.0/12
		   172.16.xxxx xxxx.xxxx xxxx   - 172.31.xxxx xxxx.xxxx xxxx 	-> 172.001 xxxx.xxxx xxxx.xxxx xxxx
Subnet Mask	 : 255.255.0.0   		- 255.255.0.0   		-> 255.240.0.0
IP Net(x=0)	 : 172.16.0.0
IP hosts	 : 172.16.0.1 - 172.31.255.254
IP Broadcast(x=1): 172.31.255.255
10000:16
...
11111:31

IP Private lớp C: 192.168.0.0/16 
		  192.168.xxxx xxxx.xxxx xxxx/16
Subnet Mask	: 255.255.0.0 
IP Net (x=0)	: 192.168.0.0
IP khả dụng	: 192.168.0.1 -> 192.168.255.254
IP Broacast(x=1): 192.168.255.255

192.168.1.1 	-> IP Private
172.17.1.2 	-> IP Private
193.168.1.4 	-> IP Public
126.3.0.253 	-> IP Public
11.10.10.10 	-> IP Public
172.31.16.249	-> IP Private
172.32.8.1	-> IP Public
172.14.2.3	-> IP Public

---------------------------------------------------------------------------------------------------------------------------------------
VD: Các IP sau đây thuộc lớp A,B,C?
Lớp A: 1 - 126 
Lớp 127 là Loopback, localhost không sử dụng
Lớp B: 128 - 191
Lớp C: 192 - 223
Lớp D: 224 - 239 -> Multicast (Truyền hình số, giao thức định tuyến động)
Lớp E: 240 - 255

110.254.255.147 -> Lớp A (110 thuộc đoạn 1-126)
152.17.10.2 	-> Lớp B (152 thuộc đoạn 128-191)
172.83.9.11	-> Lớp B (172 thuộc đoạn 128-191)
193.7.245.252	-> Lớp C (193 thuộc đoạn 192-223)

Các host trong mạng muốn tương tác được với nhau phải cùng Net và Subnet Mask
192.168.1.0/24
192.168.1.xxxx xxxx
SM: 255.255.255.0
IP Net (x=0)	 : 192.168.1.0
IP Host		 : 192.168.1.1 - 192.168.1.254
IP Broadcast(x=1): 192.168.1.255

192.168.1.0/25
192.168.1.0xxx xxxx
SM: 255.255.255.128
IP Net (x=0)	 : 192.168.1.0
IP Host		 : 192.168.1.1 - 192.168.1.126
IP Broadcast(x=1): 192.168.1.127







