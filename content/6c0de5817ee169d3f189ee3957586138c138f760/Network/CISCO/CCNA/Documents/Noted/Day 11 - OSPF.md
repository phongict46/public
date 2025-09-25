Dynamic Routing: incloude 2 kind Distance Vector & Link-State
1. *<mark style="background: #FFB8EBA6;">Distance Vector:</mark>* nhận thông tin Routing từ Neighbor và sau đó tính toán lại dựa vào thông tin mà neighbor đã gửi.
	- Loop-Routing -> chống loop Split Horizon
	- Tin tưởng hoàn toàn vào neighbor, khi update Routing thì phải chờ vào neighbor update (những neighbor là những router kết nối trực tiếp và chạy cùng giao thức Routing thuộc họ Distance Vector)
	- *<mark style="background: #FFB86CA6;">2 giao thức phổ biến:</mark>*  
		- **<mark style="background: #FFF3A3A6;">RIP (Broadcast)/RIPv2 (Multicast):</mark>**
			- Metric: Hop Count.
			- AD: 120.
			- Loop-Routing -> chống loop Split Horizon --> chống Loop dựa vào cơ chế, nhận Route từ cổng nào thì không gửi lại thông tin Route vào cổng đã nhận
			- Update Timer: 30s/lần
			- Khi 1 subnet bị Down:
			+ Router của subnet bị Down sẽ lập tức bật Subnet bị Down là Routing Poisoning mà không cần phải đợi 30s/lần (Update Timer)
			+ Router có subnet bị Down sẽ gửi Poisoning Reverse cho các Router kết nối trực tiếp chạy định tuyến RIP (Neighbor) 
			+ Các Router nhận Poisoning Reverse sẽ vào chế độ Holdown Time (180s)
			+ Nếu sau thời gian Holdown Time(180s) vẫn chưa nhận được thông tin Route bị Down -> Up thì chờ thêm 1 khoảng Flush Time (60s)--> xóa Route bị Down ra khỏi bảng định tuyến (Routing table)

		- *<mark style="background: #FFF3A3A6;">EIGRP (Multicast) của Cisco</mark>*
			- là sự kết hợp giữa Distance Vector - Link State -> Hybrid Routing (tốc độ Hội tụ mạng - thiết lập Neighbor cực kỳ nhanh) Juniper, HP, Draytek, Peplink --> không có giao thức EIGRP

2. <mark style="background: #FFB8EBA6;">Link-State:</mark> trạng thái của Link
	- *B1.* 
		- Gửi toàn bộ thông tin trạng thái của các Interface tham gia vào OSPF --> Tất cả các Router trong mạng đều nhận được thông tin này.
		- Ví dụ: có 4 router tham gia định tuyến họ Link-state thì 4 Router sẽ gửi thông tin của chính nó cho toàn bộ Router trong mạng tham gia vào định tuyến họ Link-State (LSA - Link State advertise) --> gửi thông tin thô (thông tin ban đầu từ các Router)
	- *B2.* 
		-  Khi các Router nhận thông tin của toàn bộ Router trong mạng, sẽ thực hiện việc tính toán để tìm ra đường Route tốt nhất  đến các Subnet (đường có Metric thấp nhất -> là best Route)

------------------------------------------------------------------------------------------------------------------------
1. **<mark style="background: #BBFABBA6;">Giới thiệu giao thức định tuyến OSPF:</mark>** 
	- Giao thức thuộc họ Link-State
		+ Các Router sẽ gửi thông tin trạng thái của các interface tham gia vào OSPF cho các Router khác bằng gói LSA 
		+ Khi nhận LSA đầy đủ từ tất cả các Router sẽ đưa vào bảng Topological Database để tính toán.
		+ Sử dụng thuật toán Dijktra để tính toán ra được thông tin định tuyến tốt nhất đến tất cả các Destination. --> Việc tính toán sử dụng Cost để tính ra thông tin bảng định tuyến
		- AD: 110
		- Thuật toán: Dijktra để tính toán Best Route
		- LSA - LinkState Advertise (quảng bá thông tin trạng thái cổng)
		- LSU - LinkState Update (Update bảng định tuyến dựa vào các thông số của LSA bằng thuật toán Dijktra)

	Link-State OSPF:
	- Network của subnet (Ip interface) tham gia vào OSPF
	- Area: 
		+ Area 0: Backbone của OSPF
		+ Area còn lại:
			. Nếu Router biên (Border Router) của Area khác Area 0 thì sẽ không liên lạc được với nhau
			. Các Router thuộc các Area khác Area 0 muốn giao tiếp được với nhau phải thông qua Area 0
			
	<mark style="background: #BBFABBA6;">2. Các thông số OSPF</mark>

	A. Router-id: đại diện khi các Router khác kết nối và thiết lập neighbor với nó.
	
		a. Auto: Chọn Router-ID dựa vào IP lớn nhất trên các Interface.
		Ví dụ cho trường hợp Auto: có 2 trường hợp
		TH1: không có interface Loopback 0
		---> chọn ra IP lớn nhất trong các interface
			Router1: interface g0/0 - 172.16.1.1
			 	 interface g0/1 - 192.168.1.1
			--> chọn IP của G0/1 làm Router-id
			--> router-id: 192.168.1.1
		TH2: có interface Loopback 0
			Router1: interface g0/0 - 172.16.1.1
				 interface g0/1 - 192.168.1.1
	 			 interface Loopback 0 - 10.0.0.1
			--> Chọn Auto Loopback 0 làm Router-id
 			--> router-id: 10.0.0.1


		b. Manual: Router-id tùy vào người cấu hình
		Ví dụ cho trường hợp  Manual --> Router1: router-id 1.1.1.1
		
	B. Thiết lập Neighbor: 10s Update định kỳ/ 40s Time out bỏ Route 
	- 10s sẽ gửi LSA
	- 40s sẽ delete Route nếu không có phản hồi

	C. Bầu chọn Router chính - Router phụ: Broadcast MultiAccess
	- Router chính (DR - Designated Router): 
		+ nhận multicast từ DRother 224.0.0.5
		+ gửi Multicast 224.0.0.6 để update cho DRother
	- Router phụ   (BDR - Backup Designated Router):
		+ chỉ nhận multicast từ DRother 224.0.0.5
		+ chỉ gửi Multicast 224.0.0.6 để update cho DRother khi DR bị Down
	- Router còn lại (DRother): gửi Multicast 224.0.0.5 đến DR và BDR

		Cost = BW(Referrence) 10^8 / BW(Interface)
		Có thể thay đổi BW(Referrence) trên mode Router ospf
		Ví dụ: tính Cost cho cổng Fast Ethernet 100Mb = 10^8
		Cost = 10^11/ 10^8 = 1000
		Ví dụ: tính Cost cho cổng TenGiga Ethernet 10Gb= 10^10
		Cost = 10^11/ 10^10 = 10

3. <mark style="background: #BBFABBA6;">Các môi trường trong OSPD:</mark> ****
   
	<mark style="background: #FF5582A6;">A. Broadcast MultiAccess:</mark> các router được kết nối với nhau bằng Switch/Hub (> 2 router tham gia OSPF)-> sẽ có bầu chọn 1 Router chính (DR = Designated Router), 1 Router backup cho Router chính (BDR = Backup Designated Router) và các Router còn lại sẽ gửi thông tin LSA cho DR và BDR (không gửi LSA cho nhau).
	- DR: nhận thông tin LSA từ các Router thông qua Multicast 224.0.0.5, và trả lời cho các Router thông qua Multicast 224.0.0.6
	- BDR: chỉ nhận thông tin LSA từ các Router thông qua Multicast 224.0.0.5 và không trả lời gì cả (chỉ trả lời khi DR bị Down).
	- DR được bầu chọn khi có Priority lớn nhất (thông số này thay đổi để bình chọn DR cho phân đoạn) -> vào interface tăng Priority lên hoặc các Router cùng Priority thì sẽ chọn Router nào có Router-ID lớn nhất làm DR
	- Priority default = 1
	- Priority Maximun = 255 (luôn là DR)
	- Priority = 0 (không được tham gia bầu chọn DR)

	<mark style="background: #FF5582A6;">B. Point-to-Point (P2P):</mark> đoạn kết nối chỉ có 2 Router (kết nối bằng cáp Serial, PPP hoặc HDLC) Cáp Serial, PPP hoặc HDLC hiện nay không còn nữa nên phân đoạn P2P này thường kết nối bằng cáp Ethernet và khai báo inteface này theo P2P --> không có sự bầu chọn trong phân đoạn này.
