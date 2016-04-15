# ARP
## I.Khái niệm
- ARP (Address Resolution Protocol) là giao thức phân giải địa chỉ vật lý từ địa chỉ IP.

## II.Cơ chế

<img src="http://i.imgur.com/ZCEJa4I.png">

- Ở đây tôi giải thích cơ chế của ARP theo 4 bước như sau:

### a.Bước 1

- Thiết bị A sẽ kiểm tra cache của mình (giống như quyển sổ danh bạ nơi lưu trữ tham chiếu giữa địa chỉ IP và địa chỉ MAC).
 <ul>
  <li>Nếu đã có địa chỉ MAC của IP 192.168.1.120 thì lập tức làm việc với thiết bị B.</li>
  <li>Nếu chưa có thì quá trình khởi tạo gói tin ARP Request bắt đầu. Nó gửi một gói tin broadcast đến toàn bộ các máy khác trong mạng với địa chỉ MAC và IP máy gửi là địa chỉ của chính nó, ví dụ ở đây địa chỉ IP máy nhận là 192.168.1.120, và địa chỉ MAC máy nhận là ff:ff:ff:ff:ff:ff.</li>
  </ul>

### b.Bước 2

- Các thiết bị trong mạng nhận được gói tin ARP Request sẽ kiểm tra trường địa chỉ Target Protocol Address.
 <ul>
  <li>Nếu trùng với địa chỉ của mình thì tiếp tục xử lý</li>
  <li>Nếu không thì hủy gói tin</li>
  </ul>

### c.Bước 3

- Thiết bị B có IP trùng với IP trong trường Target Protocol Address sẽ bắt đầu quá trình khởi tạo gói tin ARP Reply bằng cách:
 <ul>
  <li>Lấy các trường Sender Hardware Address và Sender Protocol Address trong gói tin ARP nhận được đưa vào làm Target trong gói tin gửi đi.</li>
  <li>Đồng thời lấy địa chỉ MAC của mình để đưa vào trường Sender Hardware Address.</li>
  </ul>
- Thiết bị B gửi gói tin Reply đã được khởi tạo đến thiết bị A đồng thời cập nhật bảng ánh xạ địa chỉ IP và MAC của thiết bị nguồn vào bảng ARP cache của mình để giảm bớt thời gian xử lý cho các lần sau (hoạt động cập nhật danh bạ).

### d.Bước 4

- Thiết bị A nhận được gói tin reply và xử lý bằng cách lưu trường Sender Hardware Address trong gói reply vào địa chỉ phần cứng của thiết bị B.
- Đồng thời thiết bị A update vào ARP cache của mình giá trị tương ứng giữa địa chỉ IP (địa chỉ network) và địa chỉ MAC (địa chỉ datalink) của thiết bị B. Lần sau sẽ không còn cần tới request.

### e. Lưu ý
- Hoạt động của ARP phức tạp hơn khi là hai hệ thống mạng gắn với nhau thông qua một router. Khi đó route sẽ là trung gian chuyển các gói ARP request và reply cho các thiết bị.

## III.ARP Header

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

## IV.Bắt gói tin ARP bằng Wireshark

- Bước 1: Khởi động phần mềm Wireshark
- Bước 2: Xóa cache ARP có sẵn trên máy bằng lệnh `netsh interface ip delete arpcache`
- Bước 3: Tìm gói ARP trong Wireshark

<img src="http://i.imgur.com/4JA8Zfh.png">

- Bước 4: Đọc gói tin ARP request

<img src="http://i.imgur.com/kWVkUdy.png">

Gói tin được gửi dạng Broadcast có thông tin IP, MAC của nguồn và IP của đích

<img src="http://i.imgur.com/ATsTAtr.png">

- Bước 5: Đọc gói tin ARP reply

<img src="http://i.imgur.com/5Fk6AjO.png">

Gói reply được gửi đến MAC nguồn và có thêm thông tin MAC mà nguồn cần

## V.Tham khảo:

- https://viblo.asia/pham.tri.thai/posts/mPjxMeZKG4YL
