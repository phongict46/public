**Mô tả:** 
- CSDL: cơ sở dữ liệu
- Máy chủ: máy chủ ảo hoá trên môi trường khách hàng
- Mạng: tất cả máy chủ web, csdl, app... chạy trên cùng một mạng 192.168.21.0/24 và được gán ip tĩnh
- Hệ điều hành chạy csdl: Oracale Linux
- Cơ sở dữ liệu: postgred sql
- Máy chủ csdl cũ: 192.168.21.56
- Máy chủ csdl mới: 192.168.21.71
- Thời gian dự kiến: 12h AM - 05 AM 16-04-2025
- Thời gian hoàn thành: 1h50 AM 16-04-2025
- Thời gian chuẩn bị: 11h PM 15-04-2025
- Thời gian bắt đầu: 12h AM 16-04-2025
  
![[Pasted image 20250416103034.png | 200x200]]
![[Pasted image 20250416103115.png | 200x200]]

**Nôi dung:** 
- Thực hiện triển khai di chuyển hệ thống cơ sở dữ liệu từ máy chủ cũ sang máy chủ mới  (DBIZ)
	- Tắt tất cả các dịch vụ đang chạy trên postgred sql (thời gian thực hiện 10')
	- Tắt tất cả dịch vụ đang chạy trên máy chủ app (thời gian thực hiện 10')
	- Duy chuyển dữ liệu bằng phương pháp copy với tổng dung lượng 300GB (thời gian thực hiện 1h)
- Thực hiện chuyển đổi IP 192.168.21.56 từ máy chủ csdl cũ sang máy chủ csdl mới (IT KHÁCH)
    - Tắt card mạng ảo trên vm máy chủ csdl cũ bằng phương pháp disable thủ công
    - Thêm ip tĩnh 192.168.21.56 trên máy chủ csdl mới + điều chỉnh file /etc/host từ 192.168.21.71 thành 192.168.21.56.
- Kiểm tra chạy thử (DBIZ & IT KHÁCH)
	- Đảm bảo mạng thông giữa các máy chủ sau khi chuyển đổi ip
		- Tắt tường lửa trên các máy chủ
		- Kiểm tra chính sách chặn trên thiết bị tường lửa (rule deny icmp protocol)
	- Đảm bảo các máy chủ và dịch vụ bật
	- Đảm bảo app hoat động và phát sinh giao dịch thành công
	- Đảm bảo thực hiện các công tác cài đặt lịch tạo bản sao dữ liệu cho máy chủ CSDL

**Kết quả:**
- Tất cả các bước thực hiện chuyển đổi csdl từ máy chủ cũ sang mới không phát sinh vấn đề ngoài dự kiến.
- Chuyển đổi ip từ server cũ sang server mới không phát sinh vấn đề ngoài dự kiến.
- Máy chủ app 192.168.21.37 đang chặn ping từ các server còn lại trong cùng mạng (vấn đề đã xảy ra từ trước khi tiến hành chuyển đổi máy chủ csdl)
	- Câu lệnh tắt tính năng tường lửa trên oracle linux 
	  ```bash
	  sudo firewall-cmd --permanent --add-icmp-block-inversion
	  sudo firewall-cmd --permanent --remove-icmp-block=echo-request
	  sudo firewall-cmd --reload
	  sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT #sử dụng cho iptable firewall#
	  sudo iptables-save > /etc/sysconfig/iptables #sử dụng cho iptable firewall#
	
	```

**Hỏi & Đáp:**
- Khi đã phát sinh giao dịch lại không thể rollback lại máy chủ csdl cũ ? (Dần chia sẻ)
- Ping thông giữa máy chủ app và các máy chủ còn lại mới có thể trao đổi thông tin ? (A.Minh chia sẻ).