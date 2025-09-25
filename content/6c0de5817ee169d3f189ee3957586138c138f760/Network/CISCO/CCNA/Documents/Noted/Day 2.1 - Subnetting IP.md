---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
Layer 3: Network (Continue)
---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
Quy hoạch IP cho không gian mạng (chia IP):
TH1: quy hoạch về subnet (số Host cho mỗi Subnet bằng nhau)	 2^n -> trái sang phải (mượn bit)
TH2: quy hoạch về Host (số Host cho mỗi Subnet khác nhau)	2^m - 2 -> phải sang trái (cần số bit)

---------------------------------------------------------------------------------------------------------------------------------------
Trường hợp 1: Quy hoạch về Subnet
VD1: 192.168.1.0/24 chia thành 3 subnet, lấy subnet thứ 2
Xác định số bit mượn, bước nhảy, SubnetMaks, IP Net, IP Broadcast, IP Hosts.
Số bit cần mượn (n) để quy hoạch thành 3 subnet
	2^n >= 3
-> n = 2 (mượn 2 bit -> 3 subnet)
/24 thuộc Octet thứ 4 nên sẽ phân tích Octet này thành nhị phân
	192.168.1.0/24 -> 192.168.1.|xxxx xxxx
Mượn 2 bit làm subnet -> đặt 2 bit này là ab
	192.168.1.ab|xx xxxx/26
ab (2bits ~ 4 giá trị)
00 -> subnet thứ 1
01 -> subnet thứ 2
10 -> subnet thứ 3
11

Lấy subnet thứ 2 -> ab = 01
	192.168.1.01|xx xxxx
Magic number là bit sau cùng của số bit mượn (b là bit sau cùng)
b mà MAGIC NUMBER -> bước nhảy = 64
128	64	32	16	8	4	2	1
 a	b
 1      1 = 128 + 64 = 192
IP tổng quát: 	192.168.1.01|xx xxxx/26
		192.168.1.64/26
	  SM:   255.255.255.192
128	64	32	16	8	4	2	1
 1      1 = 128 + 64 = 192

IP Net (x=0)	 : 192.168.1.64
IP Broadcast(x=1): 192.168.1.127
IP Hosts	 : 192.168.1.65 - 192.168.1.126

-> 
   
Lớp mạng đầu tiên:     	  192.168.1.00|xx xxxxx/26
			  192.168.1.0/26

Lớp mạng thứ 2:		  192.168.1.01|xx xxxxx/26
			  192.168.1.64/26  

Lớp mạng thứ 3		: 192.168.1.10|xx xxxxx/26
			  192.168.1.128/26 (127+1 = Broadcast của lớp mạng trước + 1)

---------------------------------------------------------------------------------------------------------------------------------------			    			     
VD2: 172.16.224.0/20 được chia thành 6 subnet - lấy subnet thứ 4
Xác định số bit mượn, bước nhảy, SubnetMaks, IP Net, IP Broadcast, IP Hosts.
B1: xác định số bits cần mượn -> 2^n >= 6
-> n = 3
/20 thuộc Octet thứ 3 nên sẽ phân tích Octet này thành nhị phân
	172.16.1110|xxxx.xxxx xxxx/20
Mượn 3 bit làm subnet -> đặt 3 bit này là abc
	172.16.1110 abc|x.xxxx xxxx/23
abc (3 bits ~ 8 giá trị)
000
001
010
011 -> subnet thứ 4 -> abc = 011

100
101
110
111
Magic number là bit sau cùng của số bit mượn (c là bit sau cùng)
c là Magic number -> bước nhảy = 2
128	64	32	16	8	4	2	1
 1	1	1	0	a	b	c
IP tổng quát(với abc = 011): 	172.16.1110 011x.xxxx xxxx/23
				172.16.230.0/23
Subnet Mask		   :    255.255.254.0

IP Net (x=0)	 : 172.16.230.0
IP Broadcast(x=1): 172.16.231.255
IP Hosts	 : 172.16.230.1 - 192.168.231.254
Note: Khi chia subnet thì chiều từ trái sang phải.

231.255 + 1 = 232.0
1110 0111.1111 1111
		  1
___________________
1110 1000.0000 0000
   232   .    0

---------------------------------------------------------------------------------------------------------------------------------------
VD3: 172.16.0.0/22 
 chia thành 7 lớp, lấy lớp mạng thứ 4
Xác định số bit mượn, bước nhảy, SubnetMaks, IP Net, IP Broadcast, IP Hosts.
- Số bit mượn: n=3
- Bước nhảy = 32
- Subnet Mask: 
- IP Net
- IP Broadcast
- IP Hosts

chia thành 7 lớp mạng -> 2^n >= 7
-> n = 3 (abc)
 	172.16.0000 00|xx.xxxx xxxx/22 (/22 ở Octet thứ 3)
	172.16.0000 00ab.c|xxx xxxx/25
c là Magic number -> bước nhảy = 128
128	64	32	16	8	4	2	1. 	128	64	32	16	8	4	2	1
						a	b.	c
Lấy lớp mạng thứ 4 -> abc = 011

IP tổng quát: 	172.16.0000 0001.1|xxx xxxx/25
		172.16.1.128/25
Subnet Mask:	255.255.255.128
IP Net: 172.16.1.128
IP Broadcast: 172.16.1.255
IP Hosts: 172.16.1.129 - 172.16.1.254

---------------------------------------------------------------------------------------------------------------------------------------
VD4: 10.128.0.0/14 được chia thành 9 lớp mạng, lấy lớp mạng thứ 5
B1: xác định số bit mượn
Số bit cần mượn để chia thành 9 lớp mạng
	2^n >= 9
-> n = 4 (mượn bit để chia cho 9 lớp mạng)

B2: đi tìm bước nhảy (phụ thuộc vào số bit cần mượn)
mượn 4 bit -> abcd
/14 thuộc Octet thứ 2 -> phân tích IP ở Octet thứ 2
128	64	32	16	8	4	2	1
 1       0	0	0	0	0	0	0 = 128
	10.1000 0000.0.0
	10.1000 00|xx.xxxx xxxx.xxxx xxxx/14 (14bit đầu tiên giữ nguyên, các bit còn lại phía sau là sự thay đổi - ký hiệu là x)
/14 của lớp mạng ban đầu có số Hosts = 2^(32-14) - 2 = 2^18 - 2 ~ 260 000 Hosts

Số bit mượn là 4bit = 2^4 = 16 
Lớp mạng ban đầu /14 sau khi xác định số bit mượn (4bit = 16) sẽ chia thành 16 phần bằng nhau, 
trong đó chỉ lấy 9 phần, phần được lấy ở thứ tự thứ 5.
-> Đem số bit mượn vào để xác định lại lớp mạng cần chia (đem 4 bit abcd vào sau dấu |)
	10.1000 00ab.cd|xx xxxx.xxxx xxxx/18
d là Magic number
128	64	32	16	8	4	2	1.	128	64	32	16	8	4	2	1
 1	0	0	0	0	0	a	b	c	d
-> bước nhảy = 64

B3: IP tổng quát và Subnet Mask 
Lấy lớp mạng thứ 5: abcd = 0100
0000
0001
0010
0011
0100 -> lớp mạng thứ 5
0101 -> lớp mạng thứ 6
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

IP tổng quát: 		0000 1010.1000 00ab.cd|xx xxxx.xxxx xxxx/18
với abcd=0100		0000 1010.1000 0001.00|xx xxxx.xxxx xxxx/18
			
			   10    .   129   .     0    .    0    /18
					10.129.0.0/18
Subnet Mask:		0000 1010.1000 0001.00|xx xxxx.xxxx xxxx/18	
			1111 1111.1111 1111.11 00 0000.0000 0000
			   255   .    255  .    192   .    0
(/18 -> 18 bit bằng 1 từ trái sang phải)

B4: Xác định IP Net, IP Broadcast, IP Hosts
IP Net (x=0)	 : 	10.129.0.0
IP Broadcast(x=1):	10.129.63.255
IP Hosts 	 :	10.129.0.1 - 10.129.63.254

- Lớp mạng thứ 6
B3: IP tổng quát và Subnet Mask 
Lấy lớp mạng thứ 6: abcd = 0101

IP tổng quát: 		0000 1010.1000 00ab.cd|xx xxxx.xxxx xxxx/18
với abcd=0101		0000 1010.1000 0001.01|xx xxxx.xxxx xxxx/18
			    10   .   129   .     64   .    0    /18
Subnet Mask:		0000 1010.1000 0001.00|xx xxxx.xxxx xxxx/18	
			1111 1111.1111 1111.11 00 0000.0000 0000
			   255   .    255  .    192   .    0
B4: Xác định IP Net, IP Broadcast, IP Hosts
IP Net (x=0)	 : 	10.129.64.0
IP Broadcast(x=1):	10.129.127.255
IP Hosts 	 :	10.129.64.1 - 10.129.127.254

---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
Trường hợp 2: Quy hoạch số lượng Host dựa vào Subnet đã có
---------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------
Sau khi nhận được lớp mạng thứ 5 này ở Site - Hồ Chí Minh 10.129.0.0/18
Phòng A có 20 nhân viên
Phòng B có 100 nhân viên
Phòng C có 50 nhân viên

B1: Sắp xếp các phòng theo thứ tự từ nhiều đến ít: 100 - 50 - 20 ~ B - C - A
B2: Xác định IP tổng quát & Subnet Mask (SM) của các phòng
Phòng B:
Xác định số bit CẦN dùng cho 100 nhân viên ---> 2^m - 2 >= 100
-> m = 7
CẦN 7 bit để quy hoạch cho phòng B có 100 nhân viên (tính từ phải sang trái lấy 7 bits)
		10.129.0.0|xxx xxxx/25 (32 - 7(số bit cần) = 25)

IP tổng quát: 10.129.0.0/25
Subnet Mask:  255.255.255.128

B3: XÁc định IP Net, IP Broadcast, IP Hosts.
IP Net: 	10.129.0.0
IP Broadcast:  	10.129.0.127
IP Hosts:	10.129.0.1 - 10.129.0.126

Đi lớp mạng tiếp theo thì lấy Broadcast + 1
		10.129.0.1|xxx xxxx

---------------------------------------------------------------------------------------------------------------------------------------
Phòng C: 
Xác định số bit CẦN dùng cho 50 nhân viên ---> 2^m - 2 >= 50
-> m = 6
CẦN 6 bits để quy hoạch cho phòng C có 50 nhân viên (tính từ phải sang trái lấy 6 bits)
		10.129.0.10xx xxxx/26 (30 - 6(số bit cần) = 26)

IP tổng quát: 10.129.0.128/26
Subnet Mask:  255.255.255.192

Xác định IP Net, IP Broadcast, IP Hosts.
IP Net:		10.129.0.128
IP Broadcast:	10.129.0.191
IP Hosts:	10.129.0.129 - 10.129.0.190

Đi lớp mạng tiếp theo thì lấy Broadcast + 1
		10.129.0.11xx xxxx

---------------------------------------------------------------------------------------------------------------------------------------
Phòng A:
Xác định số bit CẦN dùng cho 20 nhân viên ---> 2^m - 2 >= 20
-> m = 5
Cần 5 bits để quy hoạch cho phòng A có 20 nhân viên (tính từ phải sang trái lấy 5 bits)
		10.129.0.110x xxxx/27 (32 - 5(số bit cần) = 27)

IP tổng quát: 10.129.0.192/27
Subnet Mask: 255.255.255.224 
IP Net: 	10.129.0.192
IP Broadcast:	10.129.0.223
IP Hosts:	10.129.0.193 - 10.129.0.222

Bài tập: 172.16.128.0/20 chia thành 7 lớp mạng, lấy lớp mạng thứ 3
Sau khi có IP tổng quát của lớp mạng thứ 3, sẽ quy hoạch cho các phòng ban như sau.
Phòng A: 10
Phòng B: 200
Phòng C: 100
Phòng D: 50

 
