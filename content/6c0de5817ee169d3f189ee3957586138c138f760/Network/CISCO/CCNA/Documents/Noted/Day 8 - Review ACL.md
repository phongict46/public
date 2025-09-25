<<<<<<< HEAD
Access Control List - ACL
Là 1 công cụ để viết cách chính sách (Policy) cho thiết bị mạng 
(Router, SW, FW, IDS, IPS, NAC, Wireless Controller, ...)

I.Nhiệm vụ (mục tiêu) chính: 
1. Phân loại (Classification) - thường tác động là Permit 
2. Sàng lọc (Filter) - thường tác động là Deny

IP - Layer 3, Service(TCP/UDP) - Layer 4, 
Application (Next-gen FW) - Layer 7
-> CCNA chỉ hướng dẫn cho Layer 3 và Layer 4

II. Tác động:
Sau khi Classification hoặc Filter xong thì sẽ tác động, có 2 tác động chính:
1. Permit (cho phép) - đối với các thiết bị khác thì thường thấy là Allow
2. Deny (cấm) - đối với các thiết bị khác thì thường thấy là Drop 

Ví dụ:
Layer 3: 192.168.1.0/24 được truy cập All
access-list 100   permit    ip         192.168.1.0 0.0.0.255   any (0.0.0.0 255.255.255.255)
	  ID-ACL  action  protocol 	Source                Destination
--> Permit IP là permit All Service

Layer 4: 192.168.1.0/24 không truy cập được web (TCP - port 80)
access-list 100       deny       tcp         192.168.1.0 0.0.0.255    any         eq 80 (www)
	    ID-ACL    action    protocol        Source              Destination


III. Configuration (cấu hình ACL): có 2 loại
1. Standard (đơn giản): chỉ hỗ trợ IP & Source IP
ACL Standard không hỗ trợ cấu hình các Protocol khác (chỉ duy nhất IP)
ACL có ID từ 1 - 99: thì mặc định là ACL Standard
- Ví dụ: 
access-list 1 permit 192.168.1.0 0.0.0.255
2. Extended (phức tạp): hỗ trợ IP, Service(TCP/UDP), Source & Destination
- Ví dụ:
Layer 3: 192.168.1.0/24 được truy cập All
access-list 100   permit    ip         192.168.1.0 0.0.0.255   any (0.0.0.0 255.255.255.255)
	  ID-ACL  action  protocol 	Source                Destination
--> Permit IP là permit All Service

Layer 4: 192.168.1.0/24 không truy cập được web (TCP - port 80)
access-list 100       deny       tcp         192.168.1.0 0.0.0.255    any         eq 80 (www)
	    ID-ACL    action    protocol        Source 

+ Cấu hình ACL bằng name:
	Standard
Ví dụ: 
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)#access-list 1 permit 192.168.2.0 0.0.0.255
Router(config)#access-list 1 deny any   
--> chuyển cấu hình bằng NAME

Router(config)#ip access-list standard Permit_1.0
Router(config-acl)#permit 192.168.1.0 0.0.0.255
Router(config-acl)#deny ip any any
Router(config-acl)#exit

Router(config)#ip access-list standard Permit_1.0
Router(config-acl)#permit 192.168.2.0 0.0.0.255
Router(config-acl)#deny ip any any


Router(config)#ip access-list standard Permit_1.0
Router(config-acl)#permit 192.168.1.0 0.0.0.255
Router(config-acl)#permit 192.168.2.0 0.0.0.255
Router(config-acl)#deny ip any any
Router(config-acl)#exit

	Extended
Layer 3: 192.168.1.0/24 được truy cập All
access-list 100   permit    ip         192.168.1.0 0.0.0.255   any (0.0.0.0 255.255.255.255)
	  ID-ACL  action  protocol 	Source                Destination
--> Permit IP là permit All Service

Layer 3: 192.168.1.0/24 được truy cập All bằng ACL-name
Router(config)#ip access-list extended Permit_all_1.0
Router(config-acl)#permit ip 192.168.1.0 0.0.0.255 any
Router(config-acl)#deny ip any any

Lưu ý:
Đối với ACL khi cấu hình sẽ luôn có 1 Default là Implicit Deny ẩn trong ACL đứng ở dòng cuối cùng
Implicit Deny = deny ip any any
Layer 4: 192.168.1.0/24 không truy cập được web (TCP - port 80)
	 192.168.1.0/24 không truy cập được SSH tới 172.16.1.0/24 (Server)
access-list 100       deny       tcp         192.168.1.0 0.0.0.255    any         eq 80 (www)
	    ID-ACL    action    protocol        Source 

access-list 100 deny tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 permit ip any any
access-list 100 deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22

(access-list 100 deny ip any any)

no access-list 100 (phải NO access-list ID nếu muốn chèn thêm ACL vào)
access-list 100 deny tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22
access-list 100 permit ip any any

Layer 4: 192.168.1.0/24 không truy cập được web bằng name
	 192.168.1.0/24 không truy cập được SSH tới 172.16.1.0/24 (Server)
ip access-list extended deny_Web_1.0
	(10) deny tcp 192.168.1.0 0.0.0.255 any eq 80
	(20) permit ip any any

ip access-list extended Deny_Web_192.168.1.0
	11 deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22

ip access-list extended deny_Web_1.0
	(10) deny tcp 192.168.1.0 0.0.0.255 any eq 80
	(11) deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22
	(20) permit ip any any

Thứ tự của ACL rất quan trọng

Ví dụ 1: Cho phép lớp mạng 192.168.1.0 truy cập SSH, Telnet, Web, còn lại Deny
ip access-list Permit_1.0_SSH-Telnet-Web
	permit tcp 192.168.1.0 0.0.0.255 any eq 22
	permit tcp 192.168.1.0 0.0.0.255 any eq 23
	permit tcp 192.168.1.0 0.0.0.255 any eq 80
	
	(deny ip any any) - IMPLICIT DENY (ẨN)

Ví dụ 2: Cấm lớp mạng 192.168.1.0 truy cập SSH, Telnet, Web 
ip access-list Deny_1.0_SSH-Telnet-Web
	9 deny  icmp 192.168.1.0 0.0.0.255 any 	
	19 deny tcp 192.168.1.0 0.0.0.255 any eq 22
	20 deny tcp 192.168.1.0 0.0.0.255 any eq 23
	30 deny tcp 192.168.1.0 0.0.0.255 any eq 80
	40 PERMIT IP ANY ANY
	 
Số chèn chỉ chèn được khi viết thành dòng mới 


deny tcp 192.168.1.0 0.0.0.255 any eq 80 
permit tcp 192.168.1.0 0.0.0.255 any eq 80
-> không truy cập được Web

permit tcp 192.168.1.0 0.0.0.255 any eq 80
deny tcp 192.168.1.0 0.0.0.255 any eq 80 
--> truy cập được Web

	deny ip any any - IMPLICIT DENY
=======
Access Control List - ACL
Là 1 công cụ để viết cách chính sách (Policy) cho thiết bị mạng 
(Router, SW, FW, IDS, IPS, NAC, Wireless Controller, ...)

I.Nhiệm vụ (mục tiêu) chính: 
1. Phân loại (Classification) - thường tác động là Permit 
2. Sàng lọc (Filter) - thường tác động là Deny

IP - Layer 3, Service(TCP/UDP) - Layer 4, 
Application (Next-gen FW) - Layer 7
-> CCNA chỉ hướng dẫn cho Layer 3 và Layer 4

II. Tác động:
Sau khi Classification hoặc Filter xong thì sẽ tác động, có 2 tác động chính:
1. Permit (cho phép) - đối với các thiết bị khác thì thường thấy là Allow
2. Deny (cấm) - đối với các thiết bị khác thì thường thấy là Drop 

Ví dụ:
Layer 3: 192.168.1.0/24 được truy cập All
access-list 100   permit    ip         192.168.1.0 0.0.0.255   any (0.0.0.0 255.255.255.255)
	  ID-ACL  action  protocol 	Source                Destination
--> Permit IP là permit All Service

Layer 4: 192.168.1.0/24 không truy cập được web (TCP - port 80)
access-list 100       deny       tcp         192.168.1.0 0.0.0.255    any         eq 80 (www)
	    ID-ACL    action    protocol        Source              Destination


III. Configuration (cấu hình ACL): có 2 loại
1. Standard (đơn giản): chỉ hỗ trợ IP & Source IP
ACL Standard không hỗ trợ cấu hình các Protocol khác (chỉ duy nhất IP)
ACL có ID từ 1 - 99: thì mặc định là ACL Standard
- Ví dụ: 
access-list 1 permit 192.168.1.0 0.0.0.255
2. Extended (phức tạp): hỗ trợ IP, Service(TCP/UDP), Source & Destination
- Ví dụ:
Layer 3: 192.168.1.0/24 được truy cập All
access-list 100   permit    ip         192.168.1.0 0.0.0.255   any (0.0.0.0 255.255.255.255)
	  ID-ACL  action  protocol 	Source                Destination
--> Permit IP là permit All Service

Layer 4: 192.168.1.0/24 không truy cập được web (TCP - port 80)
access-list 100       deny       tcp         192.168.1.0 0.0.0.255    any         eq 80 (www)
	    ID-ACL    action    protocol        Source 

+ Cấu hình ACL bằng name:
	Standard
Ví dụ: 
Router(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)#access-list 1 permit 192.168.2.0 0.0.0.255
Router(config)#access-list 1 deny any   
--> chuyển cấu hình bằng NAME

Router(config)#ip access-list standard Permit_1.0
Router(config-acl)#permit 192.168.1.0 0.0.0.255
Router(config-acl)#deny ip any any
Router(config-acl)#exit

Router(config)#ip access-list standard Permit_1.0
Router(config-acl)#permit 192.168.2.0 0.0.0.255
Router(config-acl)#deny ip any any


Router(config)#ip access-list standard Permit_1.0
Router(config-acl)#permit 192.168.1.0 0.0.0.255
Router(config-acl)#permit 192.168.2.0 0.0.0.255
Router(config-acl)#deny ip any any
Router(config-acl)#exit

	Extended
Layer 3: 192.168.1.0/24 được truy cập All
access-list 100   permit    ip         192.168.1.0 0.0.0.255   any (0.0.0.0 255.255.255.255)
	  ID-ACL  action  protocol 	Source                Destination
--> Permit IP là permit All Service

Layer 3: 192.168.1.0/24 được truy cập All bằng ACL-name
Router(config)#ip access-list extended Permit_all_1.0
Router(config-acl)#permit ip 192.168.1.0 0.0.0.255 any
Router(config-acl)#deny ip any any

Lưu ý:
Đối với ACL khi cấu hình sẽ luôn có 1 Default là Implicit Deny ẩn trong ACL đứng ở dòng cuối cùng
Implicit Deny = deny ip any any
Layer 4: 192.168.1.0/24 không truy cập được web (TCP - port 80)
	 192.168.1.0/24 không truy cập được SSH tới 172.16.1.0/24 (Server)
access-list 100       deny       tcp         192.168.1.0 0.0.0.255    any         eq 80 (www)
	    ID-ACL    action    protocol        Source 

access-list 100 deny tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 permit ip any any
access-list 100 deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22

(access-list 100 deny ip any any)

no access-list 100 (phải NO access-list ID nếu muốn chèn thêm ACL vào)
access-list 100 deny tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22
access-list 100 permit ip any any

Layer 4: 192.168.1.0/24 không truy cập được web bằng name
	 192.168.1.0/24 không truy cập được SSH tới 172.16.1.0/24 (Server)
ip access-list extended deny_Web_1.0
	(10) deny tcp 192.168.1.0 0.0.0.255 any eq 80
	(20) permit ip any any

ip access-list extended Deny_Web_192.168.1.0
	11 deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22

ip access-list extended deny_Web_1.0
	(10) deny tcp 192.168.1.0 0.0.0.255 any eq 80
	(11) deny tcp 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255 eq 22
	(20) permit ip any any

Thứ tự của ACL rất quan trọng

Ví dụ 1: Cho phép lớp mạng 192.168.1.0 truy cập SSH, Telnet, Web, còn lại Deny
ip access-list Permit_1.0_SSH-Telnet-Web
	permit tcp 192.168.1.0 0.0.0.255 any eq 22
	permit tcp 192.168.1.0 0.0.0.255 any eq 23
	permit tcp 192.168.1.0 0.0.0.255 any eq 80
	
	(deny ip any any) - IMPLICIT DENY (ẨN)

Ví dụ 2: Cấm lớp mạng 192.168.1.0 truy cập SSH, Telnet, Web 
ip access-list Deny_1.0_SSH-Telnet-Web
	9 deny  icmp 192.168.1.0 0.0.0.255 any 	
	19 deny tcp 192.168.1.0 0.0.0.255 any eq 22
	20 deny tcp 192.168.1.0 0.0.0.255 any eq 23
	30 deny tcp 192.168.1.0 0.0.0.255 any eq 80
	40 PERMIT IP ANY ANY
	 
Số chèn chỉ chèn được khi viết thành dòng mới 


deny tcp 192.168.1.0 0.0.0.255 any eq 80 
permit tcp 192.168.1.0 0.0.0.255 any eq 80
-> không truy cập được Web

permit tcp 192.168.1.0 0.0.0.255 any eq 80
deny tcp 192.168.1.0 0.0.0.255 any eq 80 
--> truy cập được Web

	deny ip any any - IMPLICIT DENY
>>>>>>> origin/v4
