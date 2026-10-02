# Kế hoạch nâng cấp Hello Calculator MVP v1.1.0

## Mục tiêu

Nâng cấp Hello Calculator MVP v1.0 thành phiên bản v1.1.0, giữ kiến trúc web tĩnh dễ học, bổ sung tính năng hữu ích, mở rộng kiểm thử, phân tích rõ mối liên hệ giữa HTML, CSS và JavaScript, sau đó triển khai và xác minh bản chạy thật.

## 1. Khảo sát trạng thái hiện tại

- Kiểm tra toàn bộ HTML, CSS, JavaScript và test.
- Kiểm tra Git, branch, remote GitHub và các thay đổi chưa commit.
- Chạy lại 12 unit test hiện tại để tạo mốc so sánh.
- Kiểm tra khả năng chạy giao diện bằng HTTP server.

## 2. Phạm vi MVP v1.1.0

- Thêm nút đổi dấu `±`.
- Thêm phép tính phần trăm `%`.
- Hiển thị lịch sử các phép tính gần nhất.
- Thêm chức năng xóa lịch sử.
- Hỗ trợ bàn phím đầy đủ hơn.
- Cải thiện thông báo lỗi và trạng thái thao tác.
- Nâng cấp Learning Panel để quan sát rõ Event, Action và State.
- Giữ responsive từ viewport 320 px.
- Giữ kiến trúc web tĩnh, không thêm framework hoặc package không cần thiết.

## 3. Cải tiến HTML

- Thêm các nút chức năng và khu vực lịch sử bằng HTML ngữ nghĩa.
- Bổ sung nhãn ARIA và vùng thông báo phù hợp.
- Cập nhật tên phiên bản thành `MVP v1.1.0`.
- Đảm bảo thứ tự Tab hợp lý và có thể sử dụng hoàn toàn bằng bàn phím.

## 4. Cải tiến CSS

- Điều chỉnh CSS Grid cho keypad mới.
- Thiết kế khu vực lịch sử rõ ràng nhưng vẫn gọn.
- Hoàn thiện trạng thái hover, active, focus, selected và error.
- Kiểm tra giao diện tại 320, 375, 768 px và desktop.
- Tiếp tục hỗ trợ `prefers-reduced-motion`.

## 5. Cải tiến JavaScript

- Mở rộng `CalculatorModel` cho `±`, `%` và lịch sử.
- Giữ logic tính toán độc lập với DOM.
- Chuẩn hóa luồng `Event → Action → State → Render → DOM`.
- Xử lý các trường hợp biên như `%` khi đang chờ toán hạng, đổi dấu sau kết quả, chia cho 0 và số quá dài.
- Không dùng `eval()` để logic dễ hiểu và an toàn.

## 6. Mở rộng kiểm thử

- Giữ toàn bộ 12 test hiện tại.
- Thêm test tự động cho tính năng mới và trường hợp biên.
- Cập nhật `docs/TEST_CASES.md`.
- Chạy unit test bằng Node.js.
- Chạy kiểm tra HTTP cho HTML, CSS và JavaScript.
- Kiểm tra bàn phím, responsive, accessibility và lỗi JavaScript trong khả năng của môi trường.

## 7. Viết tài liệu phân tích

Tạo `doc/PHAN_TICH_CALCULATOR_MVP_V1.1.0.md`, bao gồm:

- yêu cầu ánh xạ tới file mã nguồn;
- thành phần giao diện ánh xạ tới kiến thức HTML;
- quy tắc trình bày ánh xạ tới kiến thức CSS;
- hành động và state ánh xạ tới kiến thức JavaScript;
- từng bước của một phép tính qua Event, State, Render và DOM;
- test case ánh xạ tới quy tắc nghiệp vụ;
- thay đổi từ v1.0 tới v1.1.0;
- mẫu trình bày để người học giải thích lại dự án.

## 8. Cập nhật tài liệu dự án

- Cập nhật `README.md`.
- Cập nhật hướng dẫn chạy và kiểm thử.
- Cập nhật hướng dẫn GitHub Pages nếu cần.
- Ghi rõ tính năng, giới hạn và phiên bản mới.

## 9. Triển khai

- Xác minh repository và remote trước khi thao tác.
- Commit thay đổi với nội dung rõ ràng nếu repository sẵn sàng.
- Push lên branch được GitHub Pages sử dụng nếu có quyền truy cập phù hợp.
- Theo dõi trạng thái triển khai và mở URL HTTPS.
- Chạy lại các kiểm tra quan trọng trên bản deploy.
- Nếu chưa có remote hoặc xác thực GitHub, hoàn thiện và xác minh source cục bộ, sau đó ghi rõ đúng bước còn thiếu.

## 10. Tiêu chí hoàn thành

- Test cũ không bị lỗi và test mới đều PASS.
- Không có lỗi JavaScript được phát hiện.
- Không cuộn ngang ở viewport 320 px.
- Có thể sử dụng bằng chuột và bàn phím.
- Learning Panel phản ánh đúng state.
- Tài liệu giải thích được mối liên hệ HTML–CSS–JavaScript.
- Bản triển khai có URL HTTPS, hoặc có mô tả rõ ràng về điều kiện bên ngoài còn thiếu.

