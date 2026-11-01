Giải thích dựa trên kiến trúc mạng của Docker Compose:
Mạng nội bộ Docker (Docker Bridge Network): Khi bạn khởi động các service bằng Docker Compose, Docker sẽ tự động tạo một mạng ảo riêng cho các container trong cùng cụm đó.

Cơ chế phân giải tên miền (Internal DNS): Trong mạng nội bộ này, Docker cung cấp một hệ thống DNS nội bộ, cho phép các container nhận diện và gọi nhau thông qua tên service (tên được định nghĩa trong file docker-compose.yml, ở đây là prometheus).

Lý do không dùng localhost: Nếu bạn cấu hình là http://localhost:9090 bên trong container Grafana, Grafana sẽ hiểu localhost là chính nó (container Grafana), dẫn đến việc không tìm thấy Prometheus và gây lỗi kết nối (Connection Refused). Trong khi đó, localhost:9090 chỉ dùng được trên máy tính cá nhân (Host machine) của bạn để truy cập vào Prometheus mà thôi.