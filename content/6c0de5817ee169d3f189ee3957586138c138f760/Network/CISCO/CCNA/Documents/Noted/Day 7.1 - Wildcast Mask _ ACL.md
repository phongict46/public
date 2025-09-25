--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
					Wildcast Mask
--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
- Internet hay nói là viết Subnet Mask sau đó lấy ngược lại là Wildcast Mask
-> Sai 
Vậy Wildcast Mask là địa chỉ dùng để Filter/Classification các subnet 1 cách chi tiết 
nó hiệu quả hơn rất nhiều so với dùng Subnet Mask.
--------------------------------------------------------------------------------------------
VD: 192.168.1.0/24
SM: 255.255.255.0
WM: 0  .0  .0  .255

############################################################################################
WM được định nghĩa như sau:
- Giống nhau liên tục tròn Bit thì = 0
- Khác nhau liên tục tròn Bit thì = 1
--------------------------------------------------------------------------------------------
VD1:
192.168.1.0 - 192.168.1.3
192.168.1.0000 0000
192.168.1.0000 0001
192.168.1.0000 0010
192.168.1.0000 0011 (tròn 2 bit)
0  .0  .0.0000 0011 -> WM: 0.0.0.3

VD2:
192.168.1.0/24
192.168.1.0000 0000
...
192.168.1.1111 1111 (tròn 8 bit)
0  .0  .0.1111 1111 -> WM: 0.0.0.255
			SM: 255.255.255.0

############################################################################################
WM dùng dể biểu diễn chính xác 1 dãy IP không liên tục về Bit, 
và phân biệt được chẵn và lẽ
--------------------------------------------------------------------------------------------
VD1: 172.16.0.1-8
Quy tắc là bắt đầu từ chẵn và kết thúc là lẻ thì sẽ không bị dư
Net (chẵn) -> Broadcast (Lẻ)

172.16.1.1 	-> WM: 0.0.0.0 (Host)

2-3		-> 172.16.1.0000 0010 -> WM: 0.0.0.1
		   172.16.1.0000 0011
		   0  .0 .0.0000 0001
	
4-7		-> 172.16.1.0000 0100 -> WM: 0.0.0.3
			...
		   172.16.1.0000 0111
		   0  .0 .0.0000 0011

8		-> WM: 0.0.0.0

--------------------------------------------------------------------------------------------
VD2: 172.16.0.4 - 172.16.0.14
128  64  32  16  8  4  2  1

172.16.0.4 - 172.16.0.7 
172.16.0.4 - 01 00
..
172.16.0.7 - 01 11
khác nhau 2 bit cuối -> 2 bit cuối = 1, các bit còn lai = 0
128  	64  	32  	16  	8  	4  	2  	1
 0 	0	 0       0       0      0       1       1 
-> WM: 0.0.0.3

8 - 1000 
9 - 1001
10- 1010
11- 1011
khác nhau 2 bit cuối -> 2 bit cuối = 1, các bit còn lai = 0
128  	64  	32  	16  	8  	4  	2  	1
 0 	0	 0       0       0      0       1       1 
-> WM: 0.0.0.3

12- 1100
13- 1101
khác nhau 2 bit cuối -> 1 bit cuối = 1, các bit còn lai = 0
128  	64  	32  	16  	8  	4  	2  	1
 0 	0	 0       0       0      0       0       1 

14- 1110 
1 host sẽ so với chính nó -> không có bit khác nhau
WM: 0.0.0.0

172.16.0.4 - 7  -> WM: 0.0.0.3
172.16.0.8 - 11 -> WM: 0.0.0.3
172.16.0.12 - 13 ->WM: 0.0.0.1
172.16.0.14 	 ->WM: 0.0.0.0

--------------------------------------------------------------------------------------------
Ví dụ: trong 1 công ty có network 192.168.1.0/24 
PC có IP 7-17, 20-30, 43-50 không cho không truy cập Web, chỉ cho truy cập duy nhất DHCP
7-17
7 -> 111 ->  WM: 0.0.0.0
8 -> 1000 -> WM: 0.0.0.7
...
15-> 1111

16 -> 10000 -> WM: 0.0.0.1
17 -> 10001

20 -> 10100 -> WM: 0.0.0.3
...
23 -> 10111

24 -> 11000 -> WM: 0.0.0.3
...
27 -> 11011

28 -> 11100 -> WM: 0.0.0.1
29 -> 11101

30 -> 11110 -> WM: 0.0.0.0



43 -> 101011 ->	WM: 0.0.0.0	

44 -> 101100 -> WM: 0.0.0.3
...
47 -> 101111 

48 -> 110000 -> WM: 0.0.0.1
49 -> 110001

50 -> 110010 -> WM: 0.0.0.0

Giống Summarization (CIDR) lấy ngược

Phân biệt chẵn lẻ:
số chẵn luôn kết thúc = 0
số lẻ luôn kết thúc = 1

--------------------------------------------------------------------------------------------
VD: 0-7 viết Policy chẵn không truy cập web, lẻ không truy cập Mail
Chẵn:
0: 000
2: 010
4: 100
6: 110
Giống nhau bit cuối (bit cuối = 0), khác nhau 2 bit trước đó (2 bit tiếp theo = 1)
-> 110 
WM: 0.0.0.6

Lẻ:
1: 001
3: 011
5: 101
7: 111
Giống nhau bit cuối (bit cuối = 0), khác nhau 2 bit trước đó (2 bit tiếp theo = 1)
-> 110
WM: 0.0.0.6

172.16.0.0 - WM: 0.0.0.6 (0,2,4,6 sẽ không truy cập web)
172.16.0.1 - WM: 0.0.0.6 (1,3,5,7 sẽ không truy cập Mail)

--------------------------------------------------------------------------------------------
VD: 192.168.1.0/24 - chẵn không truy cập web, lẻ không truy cập Mail
0,2,4,6,...,254 chỉ giống nhau 1 bit cuối (bit cuối = 0) và khác nhau 7 bit trước đó (7 bit = 1)
0: 	0000 0000
2: 	0000 0010
4: 	0000 0100
6: 	0000 0110
...
254:	1111 1110
-> WM: 0.0.0.254

192.168.1.0 - WM: 0.0.0.254 thì IP chẵn sẽ không truy cập được Web

192.168.1.1-255
1: 	0000 0001
3: 	0000 0011
5: 	0000 0101
7: 	0000 0111
...
255:	1111 1111
chỉ giống nhau 1 bit cuối (bit cuối = 0) và khác nhau 7 bit trước đó (7 bit = 1)
-> WM: 0.0.0.254

192.168.1.1 - WM: 0.0.0.254 thì IP lẻ sẽ không truy cập được Mail

192.168.1.0 - WM: 0.0.0.255 là IP toàn bộ lớp mạng /24

192.168.1.0/24
SM: 255.255.255.0
WM: 0.0.0.255

############################################################################################
Viết lại WM các bài như sau
Bài 1: viết WM cho dãy IP 172.16.1.3 - 172.16.1.16 
Bài 2: viết WM cho dãy IP 172.16.1.1 - 172.16.1.19
Bài 3: viết WM cho dãy IP 172.16.1.12 - 172.16.1.31
Bài 4: viết WM cho dãy IP 172.16.1.15 - 172.16.1.127

--------------------------------------------------------------------------------------------
Bài 1: viết WM cho dãy IP 172.16.1.3 - 172.16.1.16 
172.16.1.3 -> WM: 0.0.0.0

172.16.1.4 -> WM: 0.0.0.3
...
172.16.1.7
-> 172.16.1.4 & WM: 0.0.0.3

172.16.1.8 -> WM: 0.0.0.7
...
172.16.1.15
-> 172.16.1.8 & WM: 0.0.0.7

172.16.1.16 -> WM: 0.0.0.0

--------------------------------------------------------------------------------------------
Bài 2: viết WM cho dãy IP 172.16.1.1 - 172.16.1.19
172.16.1.1 -> WM: 0.0.0.0

172.16.1.2 -> WM: 0.0.0.1
172.16.1.3

172.16.1.4 -> WM: 0.0.0.3
...
172.16.1.7
-> 172.16.1.4 & WM: 0.0.0.3

172.16.1.8 -> WM: 0.0.0.7
...
172.16.1.15 
-> 172.16.1.8 & WM: 0.0.0.7

172.16.1.16 -> WM: 0.0.0.3
...
172.16.1.19
-> 172.16.1.16 & WM: 0.0.0.3

--------------------------------------------------------------------------------------------
Bài 3: viết WM cho dãy IP 172.16.1.12 - 172.16.1.31
172.16.1.12 -> WM: 0.0.0.3
...
172.16.1.15
-> 172.16.1.12 & WM: 0.0.0.3

172.16.1.16 -> WM: 0.0.0.15
...
172.16.1.31
-> 172.16.1.16 & WM: 0.0.0.15

172.16.1.32 -> WM: 0.0.0.31
...
172.16.1.63
-> 172.16.1.32 & WM: 0.0.0.31

172.16.1.64 -> WM: 0.0.0.63
...
172.16.1.127
-> 172.16.1.64 & WM: 0.0.0.63

--------------------------------------------------------------------------------------------
Bài 4: viết WM cho dãy IP 172.16.1.15 - 172.16.1.127
172.16.1.15 -> WM: 0.0.0.0

172.16.1.16 
...
172.16.1.31
-> 172.16.1.16 & WM: 0.0.0.15

172.16.1.32 -> WM: 0.0.0.31
...
172.16.1.63
-> 172.16.1.32 & WM: 0.0.0.31

172.16.1.64 -> WM: 0.0.0.63
...
172.16.1.127
-> 172.16.1.64 & WM: 0.0.0.63

--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
					Access Control List
--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
Khi router định tuyến xong, thì tất cả đều truy cập được không bị giới hạn bởi Policy. (định tuyến là Allow All)
Dùng Access Control List (ACL) có 2 vai trò:
- Filter: thông thường tác động là Deny
- Classification: thông thường tác động là Permit
Khi viết ACL thì Apply sẽ tác động lên Interface. 
Nếu ACL tác động lên Router thì sẽ Apply trên All Interface.
Mỗi Interface chỉ được Apply 1 ACL.
ACL sẽ theo thứ tự từ trên xuống (nên khi viết cẩn thận thứ tự)
Number 1-99 mặc định là Standard
--------------------------------------------------------------------------------------------
VD1: ACL cấm truy cập web nhưng không tác dụng
permit any
deny 172.16.1.0 any http
deny 172.16.1.0 any https
deny any any (default)
0.0.0.0 255.255.255.255 = any
172.16.1.1 0.0.0.0 = host 172.16.1.1

--------------------------------------------------------------------------------------------
VD2: ACL cấm truy cập web có tác dụng nhưng không hiệu quả.
deny 172.16.1.0 any http
deny 172.16.1.0 any https
permit any any
Lưu ý: ACL của Cisco thì có 1 dòng mặc định là Implicit Deny (deny all) ở dòng cuối cùng.

############################################################################################
 			Access Control List (ACL)
############################################################################################
Khi Router định tuyến xong, và truy cập được tất cả mọi nơi trong mạng thì tương đương là Router đang Permit all IP
ACL là chính sách được viết ra (làm luật, soạn luật, soạn chính sách)
Apply lên interface (ban hành luật, ban hành chính sách)
Lưu ý: 1 interface chỉ được Apply 1 ACL
ACL khi viết thì sẽ được tính từ trên xuống (theo thứ tự từ trên xuống dòng cuối cùng), dòng mới nhất sẽ ở trên cùng
-> dòng cuối cùng mặc định luôn tồn tại là deny any any (Implicit Deny)

--------------------------------------------------------------------------------------------------
			Các loại Access Control List (ACL)
--------------------------------------------------------------------------------------------------
Access Control List (ACL)
Có 2 chức năng chính là Filter/Classification và 2 tác động chính là Permit(cho phép)/Deny (cấm)
được chia thành 2 loại Standard (Classification)/Extended (Filter/Classification)

--------------------------------------------------------------------------------------------------
Access Control List (ACL)
Có 2 chức năng chính là Filter/Classification và 2 tác động chính là Permit(cho phép)/Deny (cấm)
được chia thành 2 loại Standard (Classification)/Extended (Filter/Classification)

--------------------------------------------------------------------------------------------------
ACL Standard (đơn giản/simple): chỉ tác động source IP thôi (hoàn toàn là Layer 3)
và Destination mặc định là ANY
deny ip 172.16.1.0 0.0.0.255
- Lưu ý: không dùng ACL Standard để cấm những gì liên quan đến SERVICE (Port/thuộc Layer 4/ TCP/UDP)
Nên ACL Standard chỉ phù hợp Apply ở Destination và thường dùng để Classification (không nên dùng cho Filter)

--------------------------------------------------------------------------------------------
VD: viết ACL cấm lớp mạng VLAN10 172.16.10.0/24
Router(config)#access-list 1 deny 172.16.10.0 0.0.0.255 
Router(config-acl)#access-list 1 permit any

Router(config-acl)#no access-list 1
sau đó copy lại toàn nội dung của access-list 1

Router(config)#ip access-list standard Deny-VLAN10
Router(config-acl)#deny 172.16.10.0 0.0.0.255
Router(config-acl)#permit any

- Để chèn thêm thì thêm số phía trước, còn muốn thay đổi nội dung trong ACL thì NO câu lệnh cần đổi
và sau đó chèn thêm số phía trước cho phù hợp.

############################################################################################################
			ACL Extended
############################################################################################################
ACL Extended (phức tạp/Flexible): tác động bao gồm source-ip & destination-ip & port
Lưu ý: ACL Extended phù hợp cho viết tất cả các Policy từ đơn giản đến phức tạp (phù hợp với tất cả yêu cầu)
Number 100-199 là Extended

Extended: Source & Destination IP (Layer3: icmp, traceroute/tracert, IP, ...), Source & Destination Port (Layer 4)
-> Policy để Filter - ("Deny")
1 IP sẽ có 65535 port TCP & 65535 UDP

IP thì gồm 2 thành phần IP - Wildcast Mask
TCP/UDP gồm IP - Wildcast Mask - port của TCP/UDP
8.8.8.8 	DNS:53
		HTTP: 80 -> http://google.com
		HTTPs:443 -> https://google.com
		SMTP/POP3 -> gmail
		Google Meet: 3060 - 5060

8.8.8.8:53
8.8.8.8:80
8.8.8.8:443

1-1027: port quốc tế

--------------------------------------------------------------------------------------------------
VD: viết ACL cấm lớp mạng VLAN10 172.16.10.0/24
Router(config)#access-list 100 deny ip 172.16.10.0 0.0.0.255 any 
Router(config)#access-list 100 permit ip any any

--------------------------------------------------------------------------------------------------
VD: Viết ACL cấm 
VLAN10 truy cập Web và File Server 172.16.40.254 
VLAN20 truy cập SSH 
VLAN30 truy cập Internet
Cách 1: viết theo dạng Number
Router(config)#access-list 100 deny tcp 172.16.10.0 0.0.0.255 any eq 80
Router(config)#access-list 100 deny tcp 172.16.10.0 0.0.0.255 host 172.16.40.254 eq ftp
""Router(config)#access-list 100 deny tcp 172.16.10.0 0.0.0.255 172.16.40.254 0.0.0.0 eq ftp""  <-- cách viết thứ 2 thay Host bằng 0.0.0.0
Router(config)#access-list 100 deny tcp 172.16.20.0 0.0.0.255 any eq ssh
Router(config)#access-list 100 deny ip 172.16.30.0 0.0.0.255 any
Router(config)#access-list 100 permit any any

Cách 2: Viết theo dạng Name (recommend)
Router(config)#ip access-list extended Deny-Web-FTP-VLAN10-SSH-VLAN20-Internet-VLAN30
Router(config-acl)#deny tcp 172.16.10.0 0.0.0.255 any eq 80
Router(config-acl)#deny tcp 172.16.10.0 0.0.0.255 host 172.16.40.254 eq ftp
Router(config-acl)#deny tcp 172.16.20.0 0.0.0.255 any eq ssh
Router(config-acl)#deny ip 172.16.30.0 0.0.0.255 any
Router(config-acl)#permit any any

bạn muốn chuyển đổi phần VLAN20 lên đầu ACL Deny-Web-FTP-VLAN10-SSH-VLAN20-Internet-VLAN30
Router(config)#ip access-list extended Deny-Web-FTP-VLAN10-SSH-VLAN20-Internet-VLAN30
Router(config-acl)#no deny access-list 100 deny tcp 172.16.20.0 0.0.0.255 any eq ssh
Router(config-acl)#9 deny access-list 100 deny tcp 172.16.20.0 0.0.0.255 any eq ssh

--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
ACL Extended sẽ phù hợp Apply ở phía Source.
Tác động của ACL sẽ theo quy luật Inbound và Outbound.
--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
ACL gồm 2 phần là viết nội dụng (làm luật), Apply lên Interface hoặc lên thiết bị(ban hành luật)
-> ACL chỉ có tác động khi được Apply lên interface hoặc lên thiết bị.


VD: viết ACL cho các trường hợp như sau
1. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 không được truy cập Web
2. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 không được truy cập Server File 192.168.1.100
3. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 không được ping Server File 192.168.1.100
4. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 Cấm traceroute đến Server File 192.168.1.100

Giải:
1. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 không được truy cập Web
deny 	tcp 	192.168.1.0 0.0.0.3 	0.0.0.0 255.255.255.255 	eq 	80
~
deny 	tcp 	192.168.1.0 0.0.0.3 		any			eq 	80

2. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 không được truy cập Server File 192.168.1.100
deny	tcp	192.168.1.0 0.0.0.3	192.168.1.100 0.0.0.0		eq	20
deny	tcp	192.168.1.0 0.0.0.3	192.168.1.100 0.0.0.0		eq	21
~
deny	tcp	192.168.1.0 0.0.0.3	host 	192.168.1.100 		eq	20
deny	tcp	192.168.1.0 0.0.0.3	host	192.168.1.100 		eq	21

3. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 không được ping Server File 192.168.1.100	
deny	icmp	192.168.1.0 0.0.0.3	host	192.168.1.100		echo-reply 	(ping)			

4. 192.168.1.0/24 - 192.168.1.0 đến 192.168.1.3 Cấm traceroute đến Server File 192.168.1.100
deny 	icmp	192.168.1.0 0.0.0.3	host 	192.168.1.100		time-exceeded 	(tracert)
---------------------------------------------------------------------------------------------------
Sau khi Deny phải permit lại cho những phần còn lại
permit	ip	any				any	
deny	ip	any				any	(Implicit Deny) (Always Hidden) 




