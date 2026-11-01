Để tạo menu thả xuống (Dropdown) giúp chuyển đổi linh hoạt giữa các server, bạn thực hiện theo các bước sau trong Grafana:

Mở Dashboard bạn muốn thêm biến.

Nhấp vào biểu tượng Gear (Settings) ở góc trên bên phải thanh công cụ của Dashboard.

Ở menu bên trái, chọn Variables > Nhấp vào nút Add variable.

Cấu hình các thông số cơ bản cho biến:

Name: instance (đặt đúng tên này để dễ gọi trong các biểu đồ).

Type: Chọn Query.

Label: Server (tên hiển thị trên giao diện menu thả xuống).

Data Source: Chọn đúng Data Source Prometheus của bạn.

2. Câu truy vấn PromQL để lấy danh sách máy chủ Node Exporter
Tại mục Query options > Query trong phần cấu hình biến, bạn sử dụng hàm label_values() để lọc ra toàn bộ danh sách các instance hiện có từ Prometheus:

Đoạn mã
label_values(node_uname_info, instance)
Hoặc có thể dùng: label_values(node_cpu_seconds_total, instance)

Giải thích: Hàm label_values() giúp trích xuất toàn bộ giá trị độc nhất của nhãn instance từ metric chỉ định, tạo thành một danh sách động để hiển thị trong menu thả xuống trên giao diện Grafana.

3. Cập nhật biểu đồ RAM để nhận biến động $instance (Giải quyết "Cái bẫy")
A. Tình huống lỗi cũ (Viết cứng):
Biểu đồ đang bị gán cứng địa chỉ cụ thể:

Đoạn mã
node_memory_MemFree_bytes{instance="192.168.1.10:9100"}
Cách viết này khiến biểu đồ chỉ hiển thị đúng một server đó, làm hỏng tính năng Dashboard động.

B. Cách sửa lại chuẩn xác bằng biến $instance:
Bạn sửa lại câu truy vấn trong biểu đồ RAM bằng cách gắn biến $instance vào:

Đoạn mã
node_memory_MemFree_bytes{instance="$instance"}
Hoặc nếu muốn chọn hiển thị tất cả các server cùng lúc (Multi-select):

Đoạn mã
node_memory_MemFree_bytes{instance=~"$instance"}
Giải thích: Khi người dùng chọn server nào từ menu thả xuống (ví dụ: app-server-1), Grafana sẽ tự động thay thế $instance bằng giá trị tương ứng vào câu truy vấn Prometheus, giúp biểu đồ tự động thay đổi dữ liệu theo đúng server được chọn mà không cần tạo nhiều Dashboard riêng biệt.