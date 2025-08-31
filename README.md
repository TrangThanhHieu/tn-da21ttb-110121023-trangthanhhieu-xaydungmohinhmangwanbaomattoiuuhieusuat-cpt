# Đồ án: Xây dựng mô hình mạng WAN với bảo mật và tối ưu hiệu suất

## 1. Mục tiêu chính
- Thiết kế mô hình mạng WAN mô phỏng kết nối đa chi nhánh gồm Router và Switch.
- Áp dụng các công nghệ mạng: **VLAN, STP, EtherChannel, OSPF (multi-area), GRE Tunnel**.
- Đảm bảo tiêu chí: **Khả năng dự phòng, tối ưu băng thông, bảo mật**, đồng thời dễ mở rộng và triển khai.

## 2. Kiến trúc hệ thống
- **Routers**:
  - **R1**: Kết nối trung tâm, Router-ID 1.1.1.1, tạo GRE Tunnel đến R2 và R3.
  - **R2**: Chi nhánh 1, Router-ID 2.2.2.2, Loopback cho kiểm thử.
  - **R3**: Chi nhánh 2, Router-ID 3.3.3.3, Redistribute các kết nối (Loopback).

- **Switches (Layer 3 / Layer 2)**:
  - **S1**: L3 switch, thực hiện VLAN (10/20/30), trunk, EtherChannel, định tuyến liên VLAN bằng OSPF (area 1/2/3).
  - **S2**: L2 switch, trunk & EtherChannel kết nối VLAN 20.
  - **S3**: L2 switch, trunk & EtherChannel cho VLAN 30.

- **PC/End Devices**:
  - Các máy PC được cấu hình IP thủ công trong từng VLAN để kiểm thử kết nối.

## 3. Các phần mềm và công cụ cần thiết
- **Cisco Packet Tracer 8.x** .
- **Visual Studio Code / Notepad++** (Tùy chọn) để mở và chỉnh sửa file `.cfg`.

## 4. Hướng dẫn triển khai

### Bước 1 – Clone repository
```bash
git clone https://github.com/TrangThanhHieu/tn-da21ttb-110121023-trangthanhhieu-xaydungmohinhmangwanbaomattoiuuhieusuat-cpt.git
cd tn-da21ttb-...-wanbaomattoiuuhieusuat-cpt
```

### Bước 2 – Mở mô hình mô phỏng
Trong thư mục `src/` có file mô phỏng **WAN_Model.pkt**.  
Mở **Cisco Packet Tracer**, sau đó:  
1. Chọn **File → Open**.  
2. Duyệt đến thư mục `src/`.  
3. Chọn file `WAN_Model.pkt` để mở mô hình mạng WAN.  

### Bước 3 – Import cấu hình thiết bị
Trong thư mục `configs/`, có các file: `R1.cfg`, `R2.cfg`, `R3.cfg`, `S1.cfg`, `S2.cfg`, `S3.cfg`.  
Trong Cisco Packet Tracer:  
- Nhấp chọn thiết bị (ví dụ: **R1**) → tab **CLI**.  
- Copy nội dung file `.cfg` tương ứng → paste vào CLI.  
- Thiết bị sẽ tự động áp dụng cấu hình.  

### Bước 4 – Cấu hình IP cho PC
Các PC trong VLAN 10, 20, 30 được cấu hình thủ công:  
- **IP**: ví dụ `192.168.10.10`, `192.168.20.10`, `192.168.30.10`  
- **Subnet mask**: `255.255.255.0`  
- **Default gateway**: `192.168.10.1`, `192.168.20.1`, `192.168.30.1`  

Thực hiện trong Packet Tracer: chọn PC → tab **Desktop → IP Configuration**.  

### Bước 5 – Kiểm tra kết nối
- **Ping** giữa các PC khác VLAN để kiểm tra **Inter-VLAN Routing** trên S1.  
- **Ping** giữa R1, R2, R3 qua **GRE Tunnel** để kiểm tra kết nối WAN.  

- Kiểm tra trên Router
```bash
show ip interface brief        # Xem trạng thái các interface
show running-config            # Kiểm tra cấu hình hiện tại
show ip route ospf             # Kiểm tra bảng định tuyến OSPF
show ip ospf neighbor          # Kiểm tra danh sách neighbor OSPF
ping 192.168.x.x               # Ping thử tới IP VLAN hoặc router khác
traceroute 192.168.x.x         # Kiểm tra đường đi gói tin
```

- Kiểm tra trên Switch
```bash
show vlan brief                # Kiểm tra VLAN đã tạo và port gán vào VLAN
show spanning-tree             # Kiểm tra Root Bridge và trạng thái STP
show etherchannel summary      # Kiểm tra EtherChannel có hoạt động không
show interfaces trunk          # Xem danh sách cổng trunk và VLAN cho phép
```

- Kiểm tra trên PC
```bash
ipconfig                       # Kiểm tra IP/Subnet/Gateway đã set đúng
ping 192.168.x.x               # Kiểm tra kết nối giữa các PC
````

## 6. Một vài lưu ý khi kiểm tra:

- Nếu PC không ping được → kiểm tra Default Gateway đã nhập đúng chưa.
- Nếu Router không ping qua Tunnel → kiểm tra lại tunnel source và tunnel destination.
- Nếu OSPF không lên neighbor → nhớ kiểm tra mạng quảng bá (network statement), chúng phải cùng area.
- Với EtherChannel → cần đảm bảo mode của 2 bên (active/passive hoặc desirable/auto) tương thích.

**Người thực hiện:** 

- Họ tên: Trang Thành Hiếu
- MSSV: 110121023
- Lớp: DA21TTB (Đại học Công nghệ thông tin B)
- Email: thanhhieu.160618@gmail.com
- SĐT: 0343024720

**Giảng viên hướng dẫn:** 
- ThS.Huỳnh Văn Thanh

