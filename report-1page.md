# Report 1 page - Lab 4 DES / TripleDES

## Mục tiêu

Nắm vững nguyên lý hoạt động của hệ mật mã khối đối xứng DES. Cài đặt thành công thuật toán mã hóa và giải mã DES, TripleDES từ đầu (from scratch) bằng C++ để hiểu rõ cấu trúc mạng Feistel, các hàm hoán vị, thay thế (S-box) và thuật toán sinh khóa (Key Schedule).

## Cách làm / Method

- **Tái cấu trúc code**: Xóa bỏ các giá trị biến được gán cứng (hardcode) trong file gốc. Chuyển đổi mã nguồn sang dạng hướng đối tượng với các class `KeyGenerator` và `DES` để dễ quản lý.
- **Bổ sung Giải mã (Decryption)**: Thêm hàm `decrypt` bằng cách áp dụng 16 khóa con (round keys) theo thứ tự ngược lại (từ 16 về 1).
- **Xử lý Đa khối & Padding**: Viết hàm tự động chia bản rõ thành các khối 64-bit. Nếu khối cuối cùng bị thiếu bit, sử dụng cơ chế Zero-padding (chèn số 0) để làm đầy.
- **Tích hợp TripleDES**: Xây dựng hàm chạy liên tiếp 3 vòng mã hóa/giải mã của DES với 3 khóa K1, K2, K3 theo đúng quy trình E-D-E và D-E-D.
- **Giao diện I/O**: Cấu hình hàm `main()` nhận lệnh từ Menu (1, 2, 3, 4) và đọc/ghi dữ liệu qua `stdin/stdout` để khớp với hệ thống test tự động.

## Kết quả / Result

- Chương trình biên dịch thành công bằng `g++` (c++17) mà không gặp bất kỳ lỗi hay cảnh báo (warning) nào.
- Vượt qua 100% các bài test tự động (CI test Q2 và Q4):
  - Mã hóa đa khối và Zero-padding hoạt động chính xác.
  - Mã hóa và giải mã TripleDES cho kết quả đối chiếu khớp hoàn toàn với vector kiểm thử.
  - Quá trình giải mã (round-trip) khôi phục chính xác 100% bản rõ ban đầu. Nhận diện và xử lý lỗi an toàn khi truyền sai khóa (wrong key) hoặc dữ liệu bị can thiệp (tamper).

## Kết luận / Conclusion

- **Bài học rút ra**: Hiểu sâu sắc sự phức tạp của mã hóa mức bit và vai trò cốt lõi của S-box trong việc tạo ra tính phi tuyến (non-linearity) bảo vệ dữ liệu.
- **Hạn chế**: Khóa DES 56-bit đã lỗi thời và dễ bị tấn công vét cạn (Brute-force). Cơ chế Zero-padding được sử dụng trong bài lab không an toàn cho dữ liệu thực tế vì gây nhầm lẫn nếu bản rõ gốc kết thúc bằng các số 0.
- **Hướng mở rộng**: Mặc dù TripleDES khắc phục được điểm yếu về độ dài khóa (lên 168-bit), nhưng tốc độ xử lý quá chậm do phải lặp lại thuật toán 3 lần. Trong hệ thống thực tế (production), cần thay thế hoàn toàn bằng thuật toán AES kết hợp đệm PKCS#7.
