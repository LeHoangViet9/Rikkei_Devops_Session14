1. Thao tác Import Dashboard bằng ID từ Grafana Labs
Thay vì mất thời gian vẽ từng biểu đồ thủ công, bạn có thể tận dụng các template có sẵn từ cộng đồng:

Lấy ID Template: Sử dụng Dashboard nổi tiếng dành cho JVM (Ví dụ: ID 4701 - JVM (Micrometer)).

Truy cập giao diện Import:

Trên thanh menu bên trái của Grafana, rê chuột vào biểu tượng Dashboards hoặc chọn Dashboards > Nhấp vào nút New (hoặc dấu +) > Chọn Import.

Nhập ID:

Tại ô Find and import dashboards on grafana.com, nhập số 4701.

Nhấp nút Load.

Cấu hình và Hoàn tất:

Ở trang tiếp theo, bạn có thể đổi tên Dashboard nếu muốn.

Tại mục Prometheus (phía dưới cùng, chỗ chọn Data Source), hãy chọn đúng Data Source Prometheus mà bạn đã kết nối ở Bài 1.

Nhấp nút Import để hoàn tất.

2. Top 3 Biểu đồ quan trọng nhất đối với ứng dụng Java
Sau khi import thành công, bạn sẽ thấy rất nhiều biểu đồ giám sát chi tiết. Dưới đây là 3 biểu đồ quan trọng nhất mà một lập trình viên/Tech Lead cần quan tâm:

JVM Heap Memory (Bộ nhớ Heap):

Ý nghĩa: Theo dõi dung lượng bộ nhớ mà ứng dụng Java đang sử dụng so với dung lượng tối đa được cấp phát.

Tầm quan trọng: Giúp phát hiện sớm hiện tượng rò rỉ bộ nhớ (Memory Leak). Nếu biểu đồ cứ tăng dần đều sau mỗi lần Garbage Collection mà không giảm xuống, ứng dụng của bạn có nguy cơ cao gặp lỗi OutOfMemoryError.

Garbage Collection (GC) Pause Time / Frequency (Thời gian/Tần suất dọn rác):

Ý nghĩa: Thể hiện thời gian JVM tạm dừng ứng dụng để thực hiện việc thu gom rác bộ nhớ.

Tầm quan trọng: Thời gian GC pause quá lâu hoặc diễn ra quá dày đặc sẽ làm tăng độ trễ (latency) của các HTTP request, gây giật lag cho người dùng cuối.

Thread Count (Số lượng luồng đang hoạt động):

Ý nghĩa: Đo lường tổng số lượng Thread đang chạy, trạng thái Thread (Runnable, Waiting, Blocked, Timed_Waiting).

Tầm quan trọng: Giúp kiểm soát lượng tải của hệ thống. Nếu số lượng Thread tăng phi mã (Thread Spike), hệ thống có thể bị cạn kiệt tài nguyên CPU hoặc gây ra tình trạng chết tróc (Deadlock).

3. Giải đáp "Cái bẫy" lỗi "No data" và cách khắc phục
Nguyên nhân lỗi:
Khi import Dashboard về, biến lọc application ở đầu trang hiển thị trống rỗng hoặc không có dữ liệu. Lý do là vì Spring Boot của bạn chưa cấu hình gắn thẻ (tag) application vào các metrics đẩy sang Prometheus. Khi đó, Micrometer không tạo ra tag application để Grafana lọc.

Cách khắc phục trong file application.yml (hoặc application.properties):
Bạn cần bổ sung cấu hình management.metrics.tags.application trong file application.yml của ứng dụng Spring Boot để định danh tên ứng dụng:

YAML
management:
  metrics:
    tags:
      application: ${spring.application.name}   # Hoặc điền cứng tên ứng dụng của bạn, ví dụ: my-java-app
Giải thích: Dòng cấu hình này yêu cầu Micrometer tự động gắn thêm một nhãn (tag) có tên là application kèm theo tên dịch vụ vào tất cả các metrics xuất ra. Khi Prometheus cào về và Grafana gọi câu truy vấn, biến lọc application sẽ nhận diện được giá trị này và hiển thị biểu đồ bình thường (hết lỗi "No data").