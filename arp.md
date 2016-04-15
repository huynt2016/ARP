# ARP
## I.Khái niệm
- ARP (Address Resolution Protocol) là giao thức phân giải địa chỉ vật lý từ địa chỉ IP.
## II.Cơ chế

<img src="http://i.imgur.com/ZCEJa4I.png">
- Ở đây tôi giải thích cơ chế của ARP theo 9 bươc như sau

### a.Bước 1
- Thiết bị A sẽ kiểm tra cache của mình (giống như quyển sổ danh bạ nơi lưu trữ tham chiếu giữa địa chỉ IP và địa chỉ MAC).
- Nếu đã có địa chỉ MAC của IP 192.168.1.120 thì lập tức chuyển sang bước 9.

### b.Bước 2
-  Bắt đầu khởi tạo gói tin ARP Request. Nó sẽ gửi một gói tin broadcast đến toàn bộ các máy khác trong mạng với địa chỉ MAC và IP máy gửi là địa chỉ của chính nó, ví dụ ở đây địa chỉ IP máy nhận là 192.168.1.120, và địa chỉ MAC máy nhận là ff:ff:ff:ff:ff:ff.

### c.Bước 3
- Thiết bị A phát gói tin ARP Request trên toàn mạng. Khi switch nhận được gói tin broadcast nó sẽ chuyển gói tin này tới tất cả các máy trong mạng LAN đó.

### d.Bước4
- Các thiết bị trong mạng đều nhận được gói tin ARP Request. Máy tính kiểm tra trường địa chỉ Target Protocol Address.
- Nếu trùng với địa chỉ của mình thì tiếp tục xử lý, nếu không thì hủy gói tin.

### e.Bước 5
- Thiết bị B có IP trùng với IP trong trường Target Protocol Address sẽ bắt đầu quá trình khởi tạo gói tin ARP Reply bằng cách:
 <ul>
  <li>Lấy các trường Sender Hardware Address và Sender Protocol Address trong gói tin ARP nhận được đưa vào làm Target trong gói tin gửi đi.</li>
  <li>Đồng thời lấy địa chỉ MAC của mình để đưa vào trường Sender Hardware Address.</li>
  </ul>

### f.Bước 6
- Thiết bị B cập nhật bảng ánh xạ địa chỉ IP và MAC của thiết bị nguồn vào bảng ARP cache của mình để giảm bớt thời gian xử lý cho các lần sau (hoạt động cập nhật danh bạ).

### g.Bước 7
- Thiết bị B bắt đầu gửi gói tin Reply đã được khởi tạo đến thiết bị A

### h.Bước 8
- Thiết bị A nhận được gói tin reply và xử lý bằng cách lưu trường Sender Hardware Address trong gói reply vào địa chỉ phần cứng của thiết bị B.

### i.Bước 9
- Thiết bị A update vào ARP cache của mình giá trị tương ứng giữa địa chỉ IP (địa chỉ network) và địa chỉ MAC (địac chỉ datalink) của thiết bị B. Lần sau sẽ không còn cần tới request.

### j. Lưu ý
- Hoạt động của ARP phức tạp hơn khi là hai hệ thống mạng gắn với nhau thông qua một router. Khi đó route sẽ là trung gian chuyển các gói ARP request và reply cho các thiết bị.

## ARP Header

<img src="http://i.imgur.com/hXRNka5.png">

| Trường | Mô tả |
|--------|-------|
| Hardware Type | Xác định kiểu bộ giao tiếp phần cứng máy gửi cần biết với giá trị 1 cho Ethernet |
| Protocol Type | Xác định kiểu giao thức địa chỉ cấp cao máy gửi cung cấp. Có giá trị 080016 cho giao thức IP |
| HLEN | Độ dài địa chỉ vật lý (bit) |
| PLEN | Độ dài địa chỉ logic (bit); 1: là một ARP request; 2: là một ARP reply; 3: là một RARP request; 4: là một RARP reply. |
| Sender HA | Địa chỉ MAC của máy gửi |
| Sender PA | Địa chỉ IP máy gửi |
| Target HA | Địa chỉ MAC của máy nhận |
| Target PA | Địa chỉ IP máy nhận |
| Hàng 4 | 4 x 1 |
| Hàng 3 | 3 x 1 |
| Hàng 4 | 4 x 1 |
