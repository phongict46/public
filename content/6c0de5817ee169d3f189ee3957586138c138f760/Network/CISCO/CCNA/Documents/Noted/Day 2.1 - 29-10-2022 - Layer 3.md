Mô hình OSI có 7 lớp:
7. Application: tương tác với người dùng (thấy được, sử dụng được
nhưng không chạm được).
6. Presentation: định dạng lại các thông tin từ Application gần với
mã máy nhất (Certificate, hash, nén, file định dạng .doc .xmlx
Assembly II)
5. Session: giữ phiên khi đã tạo kết nối thành công (SQL, Oracle, MySQL, MariaDB, ...)
4. Transport (Segment): tạo kết nối và truyền dịch vụ (Multi Services)
- mỗi service là 1 port
- 2^16 Services: 1 - 1024 là port Standard
VD: 22 SSH, 23 Telnet, 20-21 FTP, 53 DNS, ...
3. Network (packet): định tuyến và quy hoạch.
2. Datalink (frame): định nghĩa môi trường và chuyển mạch trong quá trình định tuyến.
- Có dây (Wire) - cáp đồng, cáp quang (802.3)
- Không dây (Wireless) - Wifi (802.11), WiMax, 4G, 5G, vệ tinh, ...
FCS (Frame Check Sequence)
1. Physical: hướng đến tiêu chuẩn kết nối

--------------------------------------------------------------------------------------
Layer 3: Network (IP)
--------------------------------------------------------------------------------------
IP: là 1 chuỗi 32bit, được chia thành 4 phần, mỗi phần 8 bit (Octet)
2^8 = 256 giá trị, 0 -> 255

127: 0111 1111
128: 1000 0000

64 : 0100 0000
193: 1100 0001
Lưu ý:
>128 thì bit đầu tiên = 1
<128 thì bit đầu tiên = 0

Chẵn thì bit cuối cùng = 0
Lẻ thì bit cuối cùng   = 1

128 	64	32	16	8	4	2	1
128	192	224	240	248	252	254	255

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
1010	A
1011	B
1100	C
1101	D
1110	E
1111	F

192.168.1.0/32
-
192.168.1.3/32
00
01
10
11

xx

x = 0 là địa chỉ net
x = 1 là địa chỉ broadcast

192.168.1.0/30

192.168.1.0/24
192.168.1.0000 0000
-
192.168.1.1111 1111

CIDR

-------------------------------------------------------------------------------
Chia subnet (quy hoạch lớp mạng)
-------------------------------------------------------------------------------
VD1: 192.168.1.0/24 được chia thành 4 phần
lấy lớp mạng thứ 2

1. Xác định số bit cần MƯỢN để chia subnet (n)
Được chia thành 4 phần -> 2^n >= 4
--> n = 2
đặt 2 bit mượn là: ab
2. Xác định Octet cần phân tích
192.168.1.0/24 (24 bit trở về trước giữ nguyên -> thuộc Octet thứ 4)
--> phân tích Octet thứ 4

192.168.1.0000 0000/24
3. Đặt số bit cần mượn vào Octet đã phân tích
mượn 2 bit ab để chia subnet
192.168.1.ab|00 0000
4. XÁc định magic number để xác định bước nhảy của lớp mạng
ab thì b là magic number 
128	64	32	16	8	4	2	1
a	b	
bước nhảy = 64
5. Xác định lớp mạng thứ 2: ab = 01
192.168.1.01xx xxxx/26 (26 = 24 + 2 bit mượn)
6. Xác định các thông số trong subnet:
- Địa chỉ tổng quát: 192.168.1.64/26
	192.168.1.01xx xxxx/26
- Subnet Mask: 255.255.255.192
- địa chỉ net 		(x=0): 192.168.1.64
- địa chỉ broadcast 	(x=1): 192.168.1.127
- địa chỉ host		     : 192.168.1.65 - 192.168.1.126

Lớp mạng thứ 3: 192.168.1.10xx xxxx/26
192.168.1.192 (4)
192.168.1.128 (3)
192.168.1.64  (2)
192.168.1.0   (1)

VD2: Subnet 172.16.128.0/22 chia thành 14 lớp mạng
lấy lớp mạng thứ 11

B1: xác định số bit MƯỢN (n)
2^n >= 14 -> n = 4
đặt 4 bit mượn là: abcd

B2: xác định Octet cần phân tích
/22 thuộc Octet thứ 3 -> phân tích Octet thứ 3

172.16.1000 00|00.0/22

B3: đặt số bit cần mượn vào octet đã phân tích
172.16.1000 00|ab.cd00 0000/22

B4: xác định magic number để xác định bước nhảy:
d là magic number

128	64	32	16	8	4	2	1.	128	64	32	
						a	b	c	d
-> bước nhảy = 64

B5: xác định lớp mạng thứ 11
abcd = 1010 (thứ tự 11)

172.16.1000 00|ab.cd|xx xxxx/26
172.16.1000 00|10.10|xx xxxx/26

B6: xác định các giá trị trong subnet thứ 11
- Địa chỉ tổng quát: 172.16.130.128/26
- Subnet mask: 255.255.255.192
- Địa chỉ net 		(x=0): 172.16.130.128
- Địa chỉ broadcast 	(x=1): 172.16.130.191
- Địa chỉ host		     : 172.16.130.129 - 172.16.130.190

-------------------------------------------------------------------------------
Chia số host theo số lượng cho trước 
-------------------------------------------------------------------------------
VD: Subnet 192.168.1.0/24 được chia theo số lượng như sau
Phòng A: 5 người
Phòng B: 50 người
Phòng C: 20 người

B1: sắp xếp giá trị các phòng theo thứ tự từ lớn đến nhỏ: B -> C -> A

B2: xác định Octet cần phân tích
/24 thuộc Octet thứ 4 -> phân tích Octet thứ 4
192.168.1.0000 0000/24

B3: xác định số bit CẦN để chứa đủ số lượng host của phòng (m)

1. Phòng B: 50 người
2^m - 2 >= 50 -> m = 6

CẦN 6 bit làm host cho phòng B -> cho 6 bit = x từ phải sang trái vào Octet đã phân tích
192.168.1.00|xx xxxx/26
       
Địa chỉ tổng quát: 192.168.1.0/26
Subnet mask: 255.255.255.192
Địa chỉ net (x=0): 192.168.1.0
Địa chỉ Broadcast (x=1): 192.168.1.63
Địa chỉ host: 192.168.1.1 - 192.168.1.62
-> subnet cho lớp mạng tiếp theo (chưa xác định số bit cần làm host):
			192.168.1.00 	
		       +	   1
			_____________
			192.168.1.01
2. Phòng C: 20 người
2^m - 2 >= 20 -> m = 5
CẦN 5 bit làm host cho phòng C -> cho 5 bit = x từ phải sang trái vào Octet đã phân tích với giá trị 
của phòng vừa phân tích + 1 (broadcast + 1)
192.168.1.01|0|x xxxx/27
00 + 1 = 01
hay là broadcast 63 =0011 1111 
		 +1  0000 0001
		 64 =0100 0000
		
			192.168.1.00 	
		       +	   1
			_____________
			192.168.1.01
192.168.1.010x xxxx/27
- địa chỉ tổng quát: 192.168.1.64/27
- subnet mask: 255.255.255.224
- Địa chỉ net (x=0): 192.168.1.64
- địa chỉ broadcast (x=1): 192.168.1.95 (64 + 31(5bit))
- Địa chỉ host: 192.168.1.65 - 192.168.1.94

-> subnet cho lớp mạng tiếp theo (chưa xác định số bit cần làm host):
			192.168.1.010 	
		       +	    1
			_____________
			192.168.1.011

Phòng A: 5 người
Xác định số bit CẦN để làm host cho phòng A:
2^m - 2 >= 5 --> m = 3
CẦN 3 bit làm host cho phòng A của subnet 192.168.1.011|0 0000/27
cho 3 bit từ phải sang trái = x
		192.168.1.011|0 0|xxx/29
Địa chỉ tổng quát: 192.168.1.96/29
Subnbet mask: 255.255.255.248
Địa chỉ net (x=0): 192.168.1.96
Địa chỉ broadcast (x=1): 192.168.1.103
Địa chỉ host : 192.168.1.97 - 192.168.1.102

-------------------------------------------------------------------------------
Địa chỉ không gian mạng 
-------------------------------------------------------------------------------
Nhóm A: 0-126
127 loopback -> không dùng
127.0.0.1
Nhóm B: 127 - 191
Nhóm C: 192 - 223
Nhóm D: 224 - 239
Nhóm E: 240 - 255

Private: 
Nhóm A: 10.0.0.0/8
Nhóm B: 172.16.0.0/16
	-
	172.31.0.0/16
172.16.0.0/12
Nhóm C: 192.168.0.0/16

Public: là các địa chỉ còn lại trừ Private

----------------------------------------------------------------------------
Summarization Network
----------------------------------------------------------------------------	
172.16.0.0/16
...
172.31.0.0/16

172.0001 0000.0.0/16
...
172.0001 1111.0.0/16
_____________________
172.0001 |xxxx|.0.0/16

172.16.0.0/12


192.168.1.0/32
...
192.168.1.3/32
---------------------------------------------------------
0 - 3/32
00
...
11
-> 0-3
192.168.1.0/30
---------------------------------------------------------
1 - 3
01
10
11
-> 1, 2-3
192.168.1.1/32
192.168.1.2/31

---------------------------------------------------------
1-2 -> không sum được
01 -> thuộc dãy 1 bit (0,1)
10 -> thuộc dãy 2 bit (00,01,10,11)

192.168.1.1/32
192.168.1.2/32
---------------------------------------------------------
1 - 4
001
010
011
100
--> 1, 2-3, 4
---------------------------------------------------------
0-7
000
...
111
--> 0-7
192.168.1.0/29

1-7
1
2-3
4-7

001 192.168.1.1/32

010 192.168.1.2/31
011

100 192.168.1.4/30
101
110
111
---------------------------------------------------------
15-32
15 192.168.1.15/32

16 192.168.1.16/28
...
31

32 192.168.1.32/32
---------------------------------------------------------
25 - 92
0001 1001
...
0001 1111

0100 0000
0101 1100


25
26-7, 28-31
0001 1010
     1011

     1100
     ...	
0001 1111

64	0100 0000 4 bit
77	0100 1111

78      0101 0000 3 bit
85      0101 0111

86	0101 1000 2 bit
89 	0101 1011

90	0101 1100 1 bits
91	0101 1101

92	0101 1110
---------------------------------------------------------
64-93  0100 0000

       0101 1111
