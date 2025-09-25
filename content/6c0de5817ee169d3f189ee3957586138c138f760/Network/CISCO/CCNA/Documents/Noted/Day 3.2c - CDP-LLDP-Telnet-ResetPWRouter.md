<<<<<<< HEAD
Router/Switch
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Các Mode của Router/Switch
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Router> mode user
- exit
- enable

Router>enable
Router#

---------------------------------------------------------------------------------------------------
Router# mode priviledge
- show các thông tin, thông số cấu hình
- show các thông số trạng thái của Router/Switch
-> mode này dùng để kiểm tra hoặc quản lý
Tại mode này chỉ được cấu hình duy nhất Network Time Protocol
- xoá thông số cấu hình
- lưu thông số cấu hình
Router#configure terminal
Router(config)#

---------------------------------------------------------------------------------------------------
Router(config)# đây là mode global config
- Cấu hình toàn bộ thông số cho Router.

CDP (Cisco), LLDP (Standard):
- dùng để kiểm tra thông số của các thiết bị sau khi đã kết nối lại với nhau.
CDP thì chỉ có trên thiết bị mạng của Cisco
LLDP là chuẩn quốc tế nên có trên tất cả các thiết bị mạng của tất cả các hãng bao gồm Cisco.

Bật tính năng CDP:
Router(config)#cdp run

Tắt năng CDP:
Router(config)#no cdp run

---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Save, Erase, Backup & Restore Router:
Khi cấu hình trên Router thì sẽ được lưu tạm thời ở RAM (running-config)
RAM chứa thông số cấu hình running-config
NVRAM chứa thông số cấu hình startup-config

1. Save cấu hình Router:
Router#copy running-config startup-config
		FROM		TO

---------------------------------------------------------------------------------------------------
2. Xoá tất cả cấu hình (erase) trên Router: 
Xoá startup-config do router khởi động sẽ lấy cấu hình ở startup-config
Router#erase startup-config

---------------------------------------------------------------------------------------------------
3. Backup cấu hình:
B1: Router#show running-config
B2: copy sang notepad

---------------------------------------------------------------------------------------------------
4. Restore cấu hình:
B1: erase toàn bộ cấu hình
B2: copy từ file notepad đã backup, dán vào trong router.


---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Cấu hình Password cho Console, enable.
Đặt password cho Console:
Router(config)#line console 0
Router(config-line)#password cisco
Router(config-line)#login

Đặt password khi vào mode priviledge:
Router(config)#enable password waren

Password:cisco
Router>enable
Password:waren
Router#

Đăng nhập bằng username và password:
B1: tạo username và password đăng nhập
Router(config)#username ccna password pass100%
B2: apply đăng nhập bằng username & password vào console
Router(config)#line console 0
Router(config-line)#login local

login: dùng password của line console 0
login local: dùng username và password được tạo trên Local Router (trên chính nó)


Che giấu thông tin password: mã hoá các password trong Router
bật service password-encryption password trong Router.
Router(config)#service password-encryption

---------------------------------------------------------------------------------------------------
Cấu hình remote bằng ứng dụng Telnet:
- B1: tạo enable password (nếu có rồi thì không cần tạo lại)
- B2: line vty 0 4 để cấu hinh telnet
Chỉ dùng password xác thực:
Router(config)#line vty 0 4
Router(config-line)#password waren123  ---> password Telnet
Router(config-line)#login ---> xác thực chỉ có password thôi
Router(config-line)#transport input telnet ---> enable Telnet.

Dùng username và password để xác thực:
Router(config)#line vty 0 4
Router(config-line)#login local

---------------------------------------------------------------------------------------------------
Mẹo:
Router(config)#no ip domain-lookup 
-> không bị hỏi domain khi gõ sai ở priviledge

Router(config)#line console 0
Router(config-line)#logging synchronous
-> tránh tình trạng đang cấu hình thì Log-Event xuất hiện chèn vào câu lệnh đang cấu hình.

Recovery Password Router(phần đọc thêm trong sách lab ở cuối sách):
Mục tiêu: để vào được mode priviledge
B1: tắt nguồn router
B2: bật nguồn Router sau đó bấm Ctrl+Break
-> vào mode ROMMON
B3: chuyển thanh ghi 0x2102 (boost startup khi khởi động) sang 0x2142 (boost không có cấu hình, không lấy ở startup-config)
B4: vào được priviledge với không có cấu hình
B5: đã vào được priviledge với không có password.
-> lấy lại cấu hình ban đầu copy startup-config running-config
Router#copy startup-config running-config
Router-Test#configure terminal

-> vào global config đổi password và chuyển thanh ghi sang 0x2102
Router-Test(config)#enable password ccna
Nếu quên password telnet hoặc console thì vào console và telnet thay đổi password.

B6: lưu lại cấu hình sau khi đổi password.













=======
Router/Switch
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Các Mode của Router/Switch
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Router> mode user
- exit
- enable

Router>enable
Router#

---------------------------------------------------------------------------------------------------
Router# mode priviledge
- show các thông tin, thông số cấu hình
- show các thông số trạng thái của Router/Switch
-> mode này dùng để kiểm tra hoặc quản lý
Tại mode này chỉ được cấu hình duy nhất Network Time Protocol
- xoá thông số cấu hình
- lưu thông số cấu hình
Router#configure terminal
Router(config)#

---------------------------------------------------------------------------------------------------
Router(config)# đây là mode global config
- Cấu hình toàn bộ thông số cho Router.

CDP (Cisco), LLDP (Standard):
- dùng để kiểm tra thông số của các thiết bị sau khi đã kết nối lại với nhau.
CDP thì chỉ có trên thiết bị mạng của Cisco
LLDP là chuẩn quốc tế nên có trên tất cả các thiết bị mạng của tất cả các hãng bao gồm Cisco.

Bật tính năng CDP:
Router(config)#cdp run

Tắt năng CDP:
Router(config)#no cdp run

---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Save, Erase, Backup & Restore Router:
Khi cấu hình trên Router thì sẽ được lưu tạm thời ở RAM (running-config)
RAM chứa thông số cấu hình running-config
NVRAM chứa thông số cấu hình startup-config

1. Save cấu hình Router:
Router#copy running-config startup-config
		FROM		TO

---------------------------------------------------------------------------------------------------
2. Xoá tất cả cấu hình (erase) trên Router: 
Xoá startup-config do router khởi động sẽ lấy cấu hình ở startup-config
Router#erase startup-config

---------------------------------------------------------------------------------------------------
3. Backup cấu hình:
B1: Router#show running-config
B2: copy sang notepad

---------------------------------------------------------------------------------------------------
4. Restore cấu hình:
B1: erase toàn bộ cấu hình
B2: copy từ file notepad đã backup, dán vào trong router.


---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Cấu hình Password cho Console, enable.
Đặt password cho Console:
Router(config)#line console 0
Router(config-line)#password cisco
Router(config-line)#login

Đặt password khi vào mode priviledge:
Router(config)#enable password waren

Password:cisco
Router>enable
Password:waren
Router#

Đăng nhập bằng username và password:
B1: tạo username và password đăng nhập
Router(config)#username ccna password pass100%
B2: apply đăng nhập bằng username & password vào console
Router(config)#line console 0
Router(config-line)#login local

login: dùng password của line console 0
login local: dùng username và password được tạo trên Local Router (trên chính nó)


Che giấu thông tin password: mã hoá các password trong Router
bật service password-encryption password trong Router.
Router(config)#service password-encryption

---------------------------------------------------------------------------------------------------
Cấu hình remote bằng ứng dụng Telnet:
- B1: tạo enable password (nếu có rồi thì không cần tạo lại)
- B2: line vty 0 4 để cấu hinh telnet
Chỉ dùng password xác thực:
Router(config)#line vty 0 4
Router(config-line)#password waren123  ---> password Telnet
Router(config-line)#login ---> xác thực chỉ có password thôi
Router(config-line)#transport input telnet ---> enable Telnet.

Dùng username và password để xác thực:
Router(config)#line vty 0 4
Router(config-line)#login local

---------------------------------------------------------------------------------------------------
Mẹo:
Router(config)#no ip domain-lookup 
-> không bị hỏi domain khi gõ sai ở priviledge

Router(config)#line console 0
Router(config-line)#logging synchronous
-> tránh tình trạng đang cấu hình thì Log-Event xuất hiện chèn vào câu lệnh đang cấu hình.

Recovery Password Router(phần đọc thêm trong sách lab ở cuối sách):
Mục tiêu: để vào được mode priviledge
B1: tắt nguồn router
B2: bật nguồn Router sau đó bấm Ctrl+Break
-> vào mode ROMMON
B3: chuyển thanh ghi 0x2102 (boost startup khi khởi động) sang 0x2142 (boost không có cấu hình, không lấy ở startup-config)
B4: vào được priviledge với không có cấu hình
B5: đã vào được priviledge với không có password.
-> lấy lại cấu hình ban đầu copy startup-config running-config
Router#copy startup-config running-config
Router-Test#configure terminal

-> vào global config đổi password và chuyển thanh ghi sang 0x2102
Router-Test(config)#enable password ccna
Nếu quên password telnet hoặc console thì vào console và telnet thay đổi password.

B6: lưu lại cấu hình sau khi đổi password.













>>>>>>> origin/v4
