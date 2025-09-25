<<<<<<< HEAD
Static Routing: dùng IP next-hop
ip route <subnet-destination-> <subnetmask-destination> <ip next-hop>
AD = 1
Connected Direct: AD = 1

-------------------------------------------------------------------------------------------------------------------------
CHỈ SỐ "AD" CỦA CÁC GIAO THỨC ĐỊNH TUYẾN:
Administrative Distance (AD) càng nhỏ càng ưu tiên 
Directed Connect: AD = 0 (kết nối trực tiếp)
Static Route: AD = 1
OSPF: AD = 110
RIP: AD = 120

Question: 1 Router đang định tuyến nhiều loại giao thức định tuyến
thì nó sẽ theo giao thức nào
Router đó đang dùng cả 3 giao thức: Static, RIP và OSPF
---> định tuyến đến 192.168.1.0/24: Static, RIP và OSPF
Static (AD = 1) 
RIP    (AD = 120)
OSPF   (AD = 110) 
Answer: chạy theo giao thức định tuyến có chỉ số AD thấp nhất
--> Static Route (AD = 1)

-------------------------------------------------------------------------------------------------------------------------
Dynamic Routing: học Route 1 cách tự động, 
NOTE: cấu hình định tuyến động là ENABLE interface để tham gia vào định tuyến
Các loại Dynamic Routing: có 3 loại
1. Distance Vector: RIP/RIPv2 (AD = 120), EIGRP (AD = 90)
2. Link State:  OSPF(AD = 110) -> Enterprisse
		IS-IS (AD = 100) -> ISP
3. Path Vector: BGP (định tuyến theo Policy dựa trên các Attribute)
A. Ưu điểm:
- Học route tự động
- Auto check được next-hop bị down và định tuyến lại.
- Phù hợp các mô hình lớn.
-> khi các router tham gia vào giao thức định tuyến động
thì sẽ Auto cập nhật theo định kỳ của mỗi loại giao thức.

B. Nhược điểm:
- Khó troubleshoot hơn so với Static Route 
--> do được học Route 1 cách tự động, nên hiểu được các gói
của loại giao thức định tuyến mà mình cấu hình.
- Tốn Performance của thiết bị: CPU, RAM

-------------------------------------------------------------------------------------------------------------------------
Distance Vector: sẽ dựa vào thông tin bảng định tuyến từ Neighbor (là những Router kết nối trực tiếp với nó)
để tính toán lại thông tin bảng định tuyến của chính nó.
Distance Vector:
	A. Ưu điểm: tính toán nhanh -> tốn ít performance thiết bị
	B. Khuyết điểm: 
	- Do chỉ nhìn thấy được neighbor, và không thấy được viễn cảnh của toàn mạng 
	--> Nó phải tin tưởng hoàn toàn vào Neighbor, và nhận thông tin Update từ Neighbor để tính toán ra Best Route.
	- Loop Route: đã khắc phục bằng cơ chế Split Horizon
-------------------------------------------------------------------------------------------------------------------------
GIỚI THIỆU GIAO THỨC ĐỊNH TUYEN ĐỘNG "RIP DYNAMIC ROUTING"
RIP Routing: Distance Vector
Tính best route dựa vào thông Metric
Metric của RIP: tính bằng Hop-Count (đếm số Router)
Hop = Router
Max Hops = 16 là vô cực (xem như không tồn tại, hoặc Down)
Rip gửi định kỳ 30s/1 lần để update thông tin bảng định tuyến.
30s x 8 = 4 phút mới xác định được mạng Down.
Do việc xác nhận mạng bị Down quá chậm do bị lỗi Update định kỳ

Giải quyết các vấn đề:
1. Chống loop Routing: Split Horizon
Là 1 cơ chế chống loop định tuyến của Distance
- Hoạt động của Split Horizon: 
Nhận thông tin định tuyến từ Interface nào, sẽ không gửi lại
thông tin định tuyến đó qua chính interface vừa nhận
2. Học route đến Vô cực (Hop = 16)
Khi 1 mạng thuộc Router C bị Down thì ngay lập tức lớp mạng bị Down
sẽ có thông số Metric (Hop = 16) --> Metric = vô cực ~ Down
--> Route bị đánh Metric = 16 này (Route Poisoning)

Gửi thông tin ngay lập tức cho các Neighbor đấu nối cùng giao thức
định tuyến RIP (bảng cập nhật ngay lập tức này --> Poisoning Reverse)
Toàn mạng sẽ được cập nhật ngay lập tức Poisoning Reverse
mà không cần đợi đến 30s định kỳ.

Router B nhận được Route Poisoning REverse sẽ rơi vào trạng thái
Holdown Time:

+ Holdown Time là trạng thái chỉ nhận Route từ bên gửi Poisoning Reverse không nhận bất cứ Route bị Down từ các Interface khác
Holdown Time (180s)
--> sau 180s mạng 10.4.0.0 vẫn được chưa loại ra khỏi bảng định tuyến

+ FLush Time (60s): sau khoảng thời gian này tính từ sau 180s của Holdown Time
Tổng time = Holdown Time + Flush Time = 180s + 60s = 240s
thì Route 10.4.0.0 -> Delete khỏi bảng định tuyến của Router B

Brief Note for RIP dynamic routing:
RIP: Distance Vector (30s cập nhật 1 lần) - AD = 120
Route bị Down: 		Route Poisoning (Metric = 16 - vô cực)
Cập nhật Route Down: 	Poisoning Reverse cho các neighbor
Holdown Time(180s): 	Nhận Poisoning Reverse sẽ rơi vào trạng thái này
Flush Time(60s):	Holdown Time + Flush Time -> Delete Route khỏi bảng định tuyến

-------------------------------------------------------------------------------------------------------------------------
A. Summary RIP Dynamic Routing:
RIP/RIPv2 (AD = 120): Metric = Hops (đếm Router, mỗi Router là 1 Hops, giá trị Metric tối đa là 15, Metric 16 = vô cực ~ DOWN) 
RIP (Broadcast)/RIPv2 (Multicast)
		Metric: Hop Count
		AD: 120
		Loop-Routing -> chống loop Split Horizon
--> chống Loop dựa vào cơ chế, nhận Route từ cổng nàothì không gửi lại thông tin Route vào cổng đã nhận
		Update Timer: 30s/lần
		Khi 1 subnet bị Down:
	+ Router của subnet bị Down sẽ lập tức bật Subnet bị Down là Routing Poisoning mà không cần phải đợi 30s/lần (Update Timer)
	+ Router có subnet bị Down sẽ gửi Poisoning Reverse cho các Router kết nối trực tiếp chạy định tuyến RIP (Neighbor) 
	+ Các Router nhận Poisoning Reverse sẽ vào chế độ Holdown Time (180s)
	+ Nếu sau thời gian Holdown Time(180s) vẫn chưa nhận được
thông tin Route bị Down -> Up thì chờ thêm 1 khoảng Flush Time (60s)
--> xóa Route bị Down ra khỏi bảng định tuyến (Routing table)

B. Configure RIP/RIPv2
RIP - Class Full cho Subnet (Auto Summary Subnet)
--> Communication: cơ chế Broadcast
RIPv2 - Class Less cho Subnet (No Auto Summary Subnet)
--> Communication: cơ chế Multicast (224.0.0.10)

RIPv1: Class Full
RouterA(config)#router rip
RouterA(config-router)#network 172.16.1.0
RouterA(config-router)#network 10.1.1.1
network 172.16.0.0 	0.3.255.255 (172.16.0.0/12)
network 10.0.0.0 	0.255.255.255 (10.0.0.0/8)
auto-summary

RIPv2: Class Less (có hỗ trợ Class Less, 
còn về mặt bình thường nó vẫn dùng được như RIPv1)
RouterA(config)#router rip
RouterA(config-router)#version 2
RouterA(config-router)#network 172.16.1.1 	0.0.0.0
RouterA(config-router)#network 10.1.1.1 	0.0.0.0
no auto-summary

NOTE: Cơ chế của định tuyến động là Enable cổng tham gia vào giao thức định tuyến

-------------------------------------------------------------------------------------------------------------------------
Note: PHân biệt Classful và Classless
RIP sử dụng Classful trong định tuyến
RIPv2, EIGRP, OSPF, ISIS thì sử dụng Classless

Classful: 	192.168.1.1/32 		-> 192.168.0.0/16
(auto summary)	192.168.129.0/23 	-> 192.168.0.0/16
		172.16.12.0/24		-> 172.16.0.0/12
		10.128.254.0/23		-> 10.0.0.0/8

Classless: xác định được rõ ràng subnet	
(no auto summary/ có hỗ trợ Wildcast Mask) 
		192.168.1.1/32		-> 192.168.1.1/32
		192.168.129.0/23	-> 192.168.129.0/23
--> đối với định tuyến RIP thì RIP/RIPv2 không cần cấu hình Wildcast Mask
Ripv2 sẽ tính toán thông tin định tuyến theo Classless khi cấu hình "no auto summary"
-> Mặc định của Ripv2 là "auto summary" (Classful)

RIPv1 sẽ gửi thông tin dưới dạng Broadcast (255.255.255.255)
RIPv2 sẽ gửi thông tin dưới dạng Multicast (224.0.0.9)

-------------------------------------------------------------------------------------------------------------------------
EIGRP (AD = 90): Metric được tính bằng 5 thông số (BW, Delay, Load, ...) có thể Load Balancing trên nhiều đường cùng 1 lúc mà có Metric không bằng nhau.
(chỉ có trên Router Cisco và tốc độ hội tụ mạng cực kỳ nhanh (very fast < 1s))
--> EIGRP được học trong chương trình CCNP Routing
EIGRP (Multicast) của Cisco
	là sự kết hợp giữa Distance Vector - Link State
	-> Hybrid Routing (tốc độ Hội tụ mạng - thiết lập Neighbor
		cực kỳ nhanh)
Juniper, HP, Draytek, Peplink --> không có giao thức EIGRP


-------------------------------------------------------------------------------------------------------------------------
2. Link State: các Router tham gia vào định tuyến sẽ gửi thông tin của chính nó cho toàn bộ các Router trong mạng
(các Router trong mạng sẽ nhìn thấy được toàn bộ trạng thái của nhau)
- Ưu điểm: tính toán hội tụ mạng rất nhanh (fast)
- Nhược điểm: tốn rất nhiều performance của mạng (RAM: lưu các trạng thái Routers, CPU: tính toán đường đi tối ưu nhất)
--> được sử dụng rộng rãi trong Enterprise lẫn ISP.

OSPF (AD = 110): định nghĩa theo vùng (Area) với Area 0 là Backbone, các Area khác Area 0 muốn thấy được nhau phải kết nối vào Area 0
- CCNA: chỉ học 1 vùng Area 0
- CCNP: học Multi Area (60% thời lượng CCNP Routing)

IS-IS (AD = 100): định nghĩa Area theo Level (học trong chương trình CCIE - chỉ dùng cho mạng ISP (Telecom))

-------------------------------------------------------------------------------------------------------------------------
3. Path Vector (BGP): dùng trong môi trường Internet (tất cả các tên miền truy cập, các Routing hình thành nên internet, trao đổi thông ting giữa các nhà mạng,
tập đoàn, ...) -> học trong chương trình CCIE (MPLS-BGP) - 60% thời lượng
-> tiền đề cho Course CCNA DataCenter
=======
Static Routing: dùng IP next-hop
ip route <subnet-destination-> <subnetmask-destination> <ip next-hop>
AD = 1
Connected Direct: AD = 1

-------------------------------------------------------------------------------------------------------------------------
CHỈ SỐ "AD" CỦA CÁC GIAO THỨC ĐỊNH TUYẾN:
Administrative Distance (AD) càng nhỏ càng ưu tiên 
Directed Connect: AD = 0 (kết nối trực tiếp)
Static Route: AD = 1
OSPF: AD = 110
RIP: AD = 120

Question: 1 Router đang định tuyến nhiều loại giao thức định tuyến
thì nó sẽ theo giao thức nào
Router đó đang dùng cả 3 giao thức: Static, RIP và OSPF
---> định tuyến đến 192.168.1.0/24: Static, RIP và OSPF
Static (AD = 1) 
RIP    (AD = 120)
OSPF   (AD = 110) 
Answer: chạy theo giao thức định tuyến có chỉ số AD thấp nhất
--> Static Route (AD = 1)

-------------------------------------------------------------------------------------------------------------------------
Dynamic Routing: học Route 1 cách tự động, 
NOTE: cấu hình định tuyến động là ENABLE interface để tham gia vào định tuyến
Các loại Dynamic Routing: có 3 loại
1. Distance Vector: RIP/RIPv2 (AD = 120), EIGRP (AD = 90)
2. Link State:  OSPF(AD = 110) -> Enterprisse
		IS-IS (AD = 100) -> ISP
3. Path Vector: BGP (định tuyến theo Policy dựa trên các Attribute)
A. Ưu điểm:
- Học route tự động
- Auto check được next-hop bị down và định tuyến lại.
- Phù hợp các mô hình lớn.
-> khi các router tham gia vào giao thức định tuyến động
thì sẽ Auto cập nhật theo định kỳ của mỗi loại giao thức.

B. Nhược điểm:
- Khó troubleshoot hơn so với Static Route 
--> do được học Route 1 cách tự động, nên hiểu được các gói
của loại giao thức định tuyến mà mình cấu hình.
- Tốn Performance của thiết bị: CPU, RAM

-------------------------------------------------------------------------------------------------------------------------
Distance Vector: sẽ dựa vào thông tin bảng định tuyến từ Neighbor (là những Router kết nối trực tiếp với nó)
để tính toán lại thông tin bảng định tuyến của chính nó.
Distance Vector:
	A. Ưu điểm: tính toán nhanh -> tốn ít performance thiết bị
	B. Khuyết điểm: 
	- Do chỉ nhìn thấy được neighbor, và không thấy được viễn cảnh của toàn mạng 
	--> Nó phải tin tưởng hoàn toàn vào Neighbor, và nhận thông tin Update từ Neighbor để tính toán ra Best Route.
	- Loop Route: đã khắc phục bằng cơ chế Split Horizon
-------------------------------------------------------------------------------------------------------------------------
GIỚI THIỆU GIAO THỨC ĐỊNH TUYEN ĐỘNG "RIP DYNAMIC ROUTING"
RIP Routing: Distance Vector
Tính best route dựa vào thông Metric
Metric của RIP: tính bằng Hop-Count (đếm số Router)
Hop = Router
Max Hops = 16 là vô cực (xem như không tồn tại, hoặc Down)
Rip gửi định kỳ 30s/1 lần để update thông tin bảng định tuyến.
30s x 8 = 4 phút mới xác định được mạng Down.
Do việc xác nhận mạng bị Down quá chậm do bị lỗi Update định kỳ

Giải quyết các vấn đề:
1. Chống loop Routing: Split Horizon
Là 1 cơ chế chống loop định tuyến của Distance
- Hoạt động của Split Horizon: 
Nhận thông tin định tuyến từ Interface nào, sẽ không gửi lại
thông tin định tuyến đó qua chính interface vừa nhận
2. Học route đến Vô cực (Hop = 16)
Khi 1 mạng thuộc Router C bị Down thì ngay lập tức lớp mạng bị Down
sẽ có thông số Metric (Hop = 16) --> Metric = vô cực ~ Down
--> Route bị đánh Metric = 16 này (Route Poisoning)

Gửi thông tin ngay lập tức cho các Neighbor đấu nối cùng giao thức
định tuyến RIP (bảng cập nhật ngay lập tức này --> Poisoning Reverse)
Toàn mạng sẽ được cập nhật ngay lập tức Poisoning Reverse
mà không cần đợi đến 30s định kỳ.

Router B nhận được Route Poisoning REverse sẽ rơi vào trạng thái
Holdown Time:

+ Holdown Time là trạng thái chỉ nhận Route từ bên gửi Poisoning Reverse không nhận bất cứ Route bị Down từ các Interface khác
Holdown Time (180s)
--> sau 180s mạng 10.4.0.0 vẫn được chưa loại ra khỏi bảng định tuyến

+ FLush Time (60s): sau khoảng thời gian này tính từ sau 180s của Holdown Time
Tổng time = Holdown Time + Flush Time = 180s + 60s = 240s
thì Route 10.4.0.0 -> Delete khỏi bảng định tuyến của Router B

Brief Note for RIP dynamic routing:
RIP: Distance Vector (30s cập nhật 1 lần) - AD = 120
Route bị Down: 		Route Poisoning (Metric = 16 - vô cực)
Cập nhật Route Down: 	Poisoning Reverse cho các neighbor
Holdown Time(180s): 	Nhận Poisoning Reverse sẽ rơi vào trạng thái này
Flush Time(60s):	Holdown Time + Flush Time -> Delete Route khỏi bảng định tuyến

-------------------------------------------------------------------------------------------------------------------------
A. Summary RIP Dynamic Routing:
RIP/RIPv2 (AD = 120): Metric = Hops (đếm Router, mỗi Router là 1 Hops, giá trị Metric tối đa là 15, Metric 16 = vô cực ~ DOWN) 
RIP (Broadcast)/RIPv2 (Multicast)
		Metric: Hop Count
		AD: 120
		Loop-Routing -> chống loop Split Horizon
--> chống Loop dựa vào cơ chế, nhận Route từ cổng nàothì không gửi lại thông tin Route vào cổng đã nhận
		Update Timer: 30s/lần
		Khi 1 subnet bị Down:
	+ Router của subnet bị Down sẽ lập tức bật Subnet bị Down là Routing Poisoning mà không cần phải đợi 30s/lần (Update Timer)
	+ Router có subnet bị Down sẽ gửi Poisoning Reverse cho các Router kết nối trực tiếp chạy định tuyến RIP (Neighbor) 
	+ Các Router nhận Poisoning Reverse sẽ vào chế độ Holdown Time (180s)
	+ Nếu sau thời gian Holdown Time(180s) vẫn chưa nhận được
thông tin Route bị Down -> Up thì chờ thêm 1 khoảng Flush Time (60s)
--> xóa Route bị Down ra khỏi bảng định tuyến (Routing table)

B. Configure RIP/RIPv2
RIP - Class Full cho Subnet (Auto Summary Subnet)
--> Communication: cơ chế Broadcast
RIPv2 - Class Less cho Subnet (No Auto Summary Subnet)
--> Communication: cơ chế Multicast (224.0.0.10)

RIPv1: Class Full
RouterA(config)#router rip
RouterA(config-router)#network 172.16.1.0
RouterA(config-router)#network 10.1.1.1
network 172.16.0.0 	0.3.255.255 (172.16.0.0/12)
network 10.0.0.0 	0.255.255.255 (10.0.0.0/8)
auto-summary

RIPv2: Class Less (có hỗ trợ Class Less, 
còn về mặt bình thường nó vẫn dùng được như RIPv1)
RouterA(config)#router rip
RouterA(config-router)#version 2
RouterA(config-router)#network 172.16.1.1 	0.0.0.0
RouterA(config-router)#network 10.1.1.1 	0.0.0.0
no auto-summary

NOTE: Cơ chế của định tuyến động là Enable cổng tham gia vào giao thức định tuyến

-------------------------------------------------------------------------------------------------------------------------
Note: PHân biệt Classful và Classless
RIP sử dụng Classful trong định tuyến
RIPv2, EIGRP, OSPF, ISIS thì sử dụng Classless

Classful: 	192.168.1.1/32 		-> 192.168.0.0/16
(auto summary)	192.168.129.0/23 	-> 192.168.0.0/16
		172.16.12.0/24		-> 172.16.0.0/12
		10.128.254.0/23		-> 10.0.0.0/8

Classless: xác định được rõ ràng subnet	
(no auto summary/ có hỗ trợ Wildcast Mask) 
		192.168.1.1/32		-> 192.168.1.1/32
		192.168.129.0/23	-> 192.168.129.0/23
--> đối với định tuyến RIP thì RIP/RIPv2 không cần cấu hình Wildcast Mask
Ripv2 sẽ tính toán thông tin định tuyến theo Classless khi cấu hình "no auto summary"
-> Mặc định của Ripv2 là "auto summary" (Classful)

RIPv1 sẽ gửi thông tin dưới dạng Broadcast (255.255.255.255)
RIPv2 sẽ gửi thông tin dưới dạng Multicast (224.0.0.9)

-------------------------------------------------------------------------------------------------------------------------
EIGRP (AD = 90): Metric được tính bằng 5 thông số (BW, Delay, Load, ...) có thể Load Balancing trên nhiều đường cùng 1 lúc mà có Metric không bằng nhau.
(chỉ có trên Router Cisco và tốc độ hội tụ mạng cực kỳ nhanh (very fast < 1s))
--> EIGRP được học trong chương trình CCNP Routing
EIGRP (Multicast) của Cisco
	là sự kết hợp giữa Distance Vector - Link State
	-> Hybrid Routing (tốc độ Hội tụ mạng - thiết lập Neighbor
		cực kỳ nhanh)
Juniper, HP, Draytek, Peplink --> không có giao thức EIGRP


-------------------------------------------------------------------------------------------------------------------------
2. Link State: các Router tham gia vào định tuyến sẽ gửi thông tin của chính nó cho toàn bộ các Router trong mạng
(các Router trong mạng sẽ nhìn thấy được toàn bộ trạng thái của nhau)
- Ưu điểm: tính toán hội tụ mạng rất nhanh (fast)
- Nhược điểm: tốn rất nhiều performance của mạng (RAM: lưu các trạng thái Routers, CPU: tính toán đường đi tối ưu nhất)
--> được sử dụng rộng rãi trong Enterprise lẫn ISP.

OSPF (AD = 110): định nghĩa theo vùng (Area) với Area 0 là Backbone, các Area khác Area 0 muốn thấy được nhau phải kết nối vào Area 0
- CCNA: chỉ học 1 vùng Area 0
- CCNP: học Multi Area (60% thời lượng CCNP Routing)

IS-IS (AD = 100): định nghĩa Area theo Level (học trong chương trình CCIE - chỉ dùng cho mạng ISP (Telecom))

-------------------------------------------------------------------------------------------------------------------------
3. Path Vector (BGP): dùng trong môi trường Internet (tất cả các tên miền truy cập, các Routing hình thành nên internet, trao đổi thông ting giữa các nhà mạng,
tập đoàn, ...) -> học trong chương trình CCIE (MPLS-BGP) - 60% thời lượng
-> tiền đề cho Course CCNA DataCenter
>>>>>>> origin/v4
