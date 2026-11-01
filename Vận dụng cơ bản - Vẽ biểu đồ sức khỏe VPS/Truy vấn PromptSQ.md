Để tính phần trăm sử dụng CPU từ metric node_cpu_seconds_total (loại trừ trạng thái idle), ta sử dụng hàm rate() kết hợp với sum(). Bạn dán đoạn PromQL này vào ô Metrics:

Đoạn mã
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
Giải thích: Lấy 100% trừ đi phần trăm thời gian CPU ở trạng thái nghỉ (idle) trong khoảng thời gian 5 phút gần nhất, kết quả trả về chính là % tải CPU hiện tại của hệ thống.

B. Cấu hình Panel
Panel Type (Loại biểu đồ): Chọn Time series (ở góc trên bên phải thanh Panel).

Panel Title: Đặt tên là CPU Usage (%).

Đơn vị (Unit): Ở thanh tùy chỉnh bên phải, chọn mục Standard options > Unit > Chọn Percent (0-100) hoặc percent (0.0-100.0) để hiển thị ký tự % tự động.

3. Biểu đồ 2: Lượng RAM còn trống (Stat) và Xử lý "Cái bẫy" đơn vị
A. Câu truy vấn PromQL
Bạn chọn Add panel tiếp theo và sử dụng metric đo dung lượng RAM trống node_memory_MemFree_bytes.

Câu truy vấn PromQL:

Đoạn mã
node_memory_MemFree_bytes
Cách giải quyết "Cái bẫy" (Không cần nhân chia thủ công):
Thay vì phải nhân chia phức tạp trong PromQL (ví dụ như chia cho 1024 * 1024 để ra MB), Grafana có sẵn tính năng tự động quy đổi đơn vị cực kỳ thông minh ở thanh cấu hình bên phải.

B. Cấu hình Panel
Panel Type (Loại biểu đồ): Chọn Stat (hiển thị 1 con số lớn trực quan).

Panel Title: Đặt tên là RAM Free.

Cài đặt đơn vị (Unit) - Vượt qua cái bẫy:

Nhìn sang thanh công cụ bên phải, tìm mục Standard options.

Tại ô Unit, nhấp vào và chọn danh mục Data (bytes).

Chọn đơn vị là Megabytes (MB) hoặc Gigabytes (GB) (hoặc chọn bytes(IEC) để Grafana tự động co giãn từ Bytes sang KB, MB, GB tùy theo lượng RAM lớn hay nhỏ).

Kết quả: Grafana sẽ tự động lấy dữ liệu thô (Bytes) từ Prometheus và tự động quy đổi, định dạng đẹp mắt ra MB/GB mà bạn không cần phải tự tính toán trong câu truy vấn!