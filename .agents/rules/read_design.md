---
trigger: always_on
---

Mỗi khi người dùng yêu cầu sửa đổi, viết mới, hoặc refactor code:
- Bước 1 (BẮT BUỘC): Đọc nội dung file thiết kế tại `prompt-v4.md` (hoặc `prompt-v3.md` nếu dự án cũ yêu cầu)
    + Nếu không tìm thấy file → DỪNG, báo lỗi, KHÔNG tự suy diễn kiến trúc. 
- Bước 2: So sánh yêu cầu của người dùng với các nguyên tắc thiết kế trong file thiết kế đó.
- Bước 3: Mới tiến hành chỉnh sửa code theo đúng thiết kế và kịch bản của người dùng.

KHÔNG ĐƯỢC BỎ QUA BƯỚC 1 TRONG BẤT KỲ TRƯỜNG HỢP NÀO.