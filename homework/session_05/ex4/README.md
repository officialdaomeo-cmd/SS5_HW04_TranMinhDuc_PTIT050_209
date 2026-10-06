# Bài 4: Mô phỏng quy trình Hotfix & Gitflow thực tế

## Sơ đồ phân nhánh Gitflow đã sử dụng

Trong bài tập này, chúng ta áp dụng mô hình phân nhánh Gitflow cơ bản với 3 nhánh chính:

1. **`main`**: Nhánh chứa mã nguồn ổn định, đang chạy trên production (phiên bản `v1.0.0`).
2. **`develop`**: Nhánh chứa các tính năng mới đang được phát triển, chuẩn bị cho bản phát hành trong tương lai, không thể deploy ngay.
3. **`hotfix/v1.0.1`**: Nhánh được tạo ra trực tiếp từ `main` nhằm mục đích sửa lỗi khẩn cấp (sự cố lộ dữ liệu người dùng).

### Quy trình thực hiện:
- Hệ thống gặp lỗi khẩn cấp trên phiên bản `v1.0.0` ở nhánh `main`.
- **Tách nhánh sửa lỗi**: Tạo nhánh `hotfix/v1.0.1` trực tiếp từ `main`.
- **Sửa lỗi**: Chỉnh sửa mã nguồn và commit trên `hotfix/v1.0.1`.
- **Gộp ngược nhánh hotfix**:
  - Chuyển sang `main`, merge `hotfix/v1.0.1` vào `main` và đánh tag `v1.0.1` chuẩn bị release bản vá.
  - Chuyển sang `develop`, merge `hotfix/v1.0.1` vào `develop` để đảm bảo lỗi được sửa cũng có mặt trong các phiên bản phát triển tương lai. Trong quá trình này nếu có conflict với các tính năng mới thì cần resolve conflict.
- **Dọn dẹp**: Xóa nhánh `hotfix/v1.0.1` ở máy cục bộ sau khi đã gộp thành công vào cả 2 nhánh.

## Đồ thị commit (Commit Graph)

Dưới đây là đồ thị commit của repository thể hiện rõ lịch sử tạo, sửa lỗi và merge các nhánh:

```text
*   78d76f0 (HEAD -> develop) Merge branch 'hotfix/v1.0.1' into develop
|\  
| * 589cec5 (tag: v1.0.1, main) Fix data leak issue on production
* | 9d7967f Start developing features for future release
|/  
* 18687ee (tag: v1.0.0) Initial commit for production v1.0.0
```
