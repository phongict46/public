
[[$ Park_Foreman_Vulnerability_Management,_Auerbach_Publications_2019.pdf]]
1. SW:
   - để xác định hiệu năng của một thiết bị sw có thể dựa vào nhiều yếu tố như : phần cứng (Ram, CPU), khả năng lưu trữ của bảng Mac, tốc độ của cổng kết nối & cổng uplink
     - case study: 
       1. mặc định thời gian xoá địa chỉ mac của thiết bị không sử dụng là khoản 5' tuỳ nhà sản xuất quy định
       2. để tránh hiện tượng đầy bảng Mac có thể dẫn đến chậm lag mạng có thể cấu hình thời gian xoá địa chỉa mac không sử dụng 
       3. với các máy chủ hoặc thiết bị yêu cầu kết nối liên tục và ưu tiên không học lại địa chỉ mac có thể sử dụng giải pháp gán cố định Sticky Mac kết hợp đặt ip tĩnh + chia vlan riêng + công cụ quản lý và giám sát ip (Microsoft DNS Manage, SolarWinds IPAM, ManageEngine OpManager) 
2. Router:
   - Router chỉ quan tâm đến Subnet không quan tâm IP (trường hợp khai IP đúng khai Subnet sai router vẫn không thể định tuyến)
   - sử dụng thuật toán AND trên các Subnet để định tuyến các mạng với nhau
   - Subnet /24 sẽ tiêu tốn ít tài nguyên của router trong việc định tuyến so với /8 việc kết nối và truyền tải thông tin nhanh hơn (quy hoạch subnet hợp lý để tránh làm giảm hiệu năng thiết bị)
3. FW: 
