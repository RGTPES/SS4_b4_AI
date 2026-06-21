# BÀI 4: Phân tích & Lựa chọn (Kỹ thuật Prompt lặp tối ưu hóa thuật toán)

## 1. Đáp án lựa chọn
**Phương án B** là phương án phản hồi tối ưu nhất.

---

## 2. Giải thích lý do lựa chọn
Phương án B thể hiện xuất sắc kỹ thuật **Iterative Prompting (Prompt lặp)** nhờ tính cụ thể, rõ ràng và các ràng buộc kỹ thuật chặt chẽ:

1.  **Phản hồi thông tin định lượng (Quantitative Feedback)**: Chỉ rõ nhược điểm của code cũ là *"độ phức tạp O(2^N)"* và hậu quả thực tế là *"gây treo hệ thống khi n = 50"*. Điều này giúp AI nhận diện chính xác vấn đề nằm ở cấu trúc thuật toán đệ quy chồng chéo.
2.  **Định hướng giải pháp thuật toán rõ ràng**: Yêu cầu AI áp dụng trực tiếp kỹ thuật *"Quy hoạch động (Dynamic Programming - Tabulation hoặc Memoization)"*. Việc này thu hẹp phạm vi tìm kiếm giải pháp của AI, giúp AI tập trung thiết kế thuật toán tối ưu thay vì đoán mò.
3.  **Đặt mục tiêu hiệu năng cụ thể**: Yêu cầu đưa *"độ phức tạp thời gian về O(N)"* và *"độ phức tạp không gian O(1) hoặc O(N)"*. Đây là đích đến chuẩn xác nhất cho bài toán Fibonacci.
4.  **Ràng buộc kiểu dữ liệu cực kỳ quan trọng**: *"Giữ nguyên kiểu trả về long"*. 
    *   *Lý do:* Số Fibonacci thứ 50 có giá trị là **12,586,269,025**, vượt quá giới hạn lưu trữ tối đa của kiểu `int` trong Java (`Integer.MAX_VALUE = 2,147,483,647`). Nếu không có ràng buộc này, AI có thể trả về kiểu `int` dẫn đến lỗi tràn số (Arithmetic Overflow) hoặc tự ý đổi sang kiểu `BigInteger` làm thay đổi cấu trúc thiết kế ban đầu của hệ thống.

---

## 3. Phân tích nhược điểm của các phương án còn lại

### Phương án A: *"Code chạy chậm quá, viết lại bằng thuật toán khác tối ưu hơn giúp tôi."*
*   **Nhược điểm:**
    *   **Quá chung chung**: Không cung cấp thông số vì sao code chạy chậm (không phân tích độ phức tạp thời gian).
    *   **Không định hướng**: Từ khóa "thuật toán khác tối ưu hơn" quá mơ hồ. AI có thể viết lại bằng phương pháp đệ quy có nhớ (Memoization) nhưng vẫn giữ kiểu dữ liệu sai, hoặc chuyển sang một thuật toán ma trận phức tạp không cần thiết, thậm chí là giữ nguyên đệ quy và chỉ tối ưu hóa cục bộ.
    *   **Thiếu ràng buộc kiểu dữ liệu**: Có thể dẫn đến lỗi tràn số khi tính toán $n = 50$ nếu AI sử dụng kiểu trả về mặc định là `int`.

### Phương án C: *"Hãy viết lại code tính Fibonacci bằng cách sử dụng Java Stream API để code chạy song song (parallel) cho nhanh hơn."*
*   **Nhược điểm:**
    *   **Sai lệch hoàn toàn về mặt thuật toán**: Bản chất của số Fibonacci là sự phụ thuộc dữ liệu tuần tự ($F(N) = F(N-1) + F(N-2)$). Các phép tính sau phụ thuộc trực tiếp vào kết quả của các phép tính trước. Việc sử dụng Parallel Stream không thể giải quyết được sự phụ thuộc tuần tự này.
    *   **Gây treo hệ thống và cạn kiệt tài nguyên nhanh hơn**: Chạy song song cây đệ quy có độ phức tạp lũy thừa $O(2^N)$ sẽ tạo ra hàng tỷ tiến trình con chồng chéo đè lên ForkJoinPool. Điều này khiến CPU lập tức bị quá tải 100%, gây ra lỗi tràn bộ nhớ Stack (`StackOverflowError`) hoặc sập ứng dụng nhanh hơn nhiều so với chạy đơn luồng.
    *   **Overhead cao**: Stream API sinh ra rất nhiều đối tượng trung gian, gây gánh nặng cực lớn cho bộ dọn rác (Garbage Collector) của JVM khi xử lý số lượng cuộc gọi đệ quy khổng lồ.
