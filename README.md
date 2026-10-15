# Bài thực hành 2 (Лабораторная 2)

## Kiến thức được áp dụng

Các khái niệm cơ bản của lập trình hướng đối tượng (OOP), GRASP, SOLID và các mẫu thiết kế cấu trúc (Structural Design Patterns).

## Thuật ngữ

- **Tara (Тара)** – Thùng chứa bằng nhựa, bên trong chứa các đơn vị hàng hóa.
- **Pallet (Палета)** – Tấm kê hàng bằng gỗ, trên đó đặt các thùng chứa.
- **Repository (Репозиторий)** – Lớp/đối tượng chịu trách nhiệm lưu trữ các thực thể.
- **ОВХ** – Các đặc tính về thể tích và trọng lượng (Объёмно-весовые характеристики).

## Giới thiệu

Các đơn vị hàng hóa trong kho được lưu trữ trong nhiều loại vật chứa khác nhau, có cấu trúc lồng nhau. Để tối ưu hóa việc lưu trữ, cần xây dựng những quy tắc nhất định nhằm phân phối các đơn vị hàng hóa vào các vật chứa khác nhau.

Khi đưa một đơn vị hàng hóa vào kho, cần đặt nó vào một thùng chứa (Tara), và thùng chứa này phải nằm trên một pallet nhất định. Sau đó, các pallet được sử dụng để lưu trữ và vận chuyển các thùng chứa trong kho.

Logic lưu trữ các đơn vị hàng hóa bao gồm nhiều ràng buộc khác nhau:
- Đặc tính thể tích và trọng lượng (ОВХ).
- Danh mục hàng hóa.
- Hạn sử dụng.

Để đảm bảo tuân thủ các quy tắc trên, cần triển khai một hệ thống có khả năng lựa chọn thùng chứa phù hợp dựa trên một tập hợp các quy tắc nghiệp vụ (Business Rules) đã được xác định.

## Yêu cầu bài tập

Thiết kế và triển khai một mô hình đối tượng (Object Model) mô tả các quy tắc phân phối các đơn vị hàng hóa vào các thùng chứa.

## Miền nghiệp vụ (Domain)

### SKU

- Mã định danh (Identifier).
- Danh mục (Category): điện tử, bánh nướng, ФРОВ (xem ghi chú 1).
- Đặc tính thể tích và trọng lượng (ОВХ).

### Đơn vị hàng hóa (Экземпляр)

- Mã định danh (Identifier).
- SKU.
- Hạn sử dụng (không bắt buộc).

### Nhóm hàng hóa (Товарная группа)

Một tập hợp các SKU được nhóm lại với nhau.

- Mỗi SKU có thể thuộc tối đa một nhóm hàng hóa.
- Một nhóm hàng hóa chứa một tập hợp các danh mục.

### Quy tắc tương thích hàng hóa (Правило товарного соседства)

Một số đơn vị hàng hóa bị cấm lưu trữ trong cùng một thùng chứa (ví dụ: diêm và xăng).

Việc kiểm tra xem các đơn vị hàng hóa có được phép lưu trữ trong cùng một thùng chứa hay không được gọi là kiểm tra tính tương thích hàng hóa.

- Việc kiểm tra tính tương thích hàng hóa sử dụng các nhóm hàng hóa.
- Nếu một đơn vị hàng hóa thuộc một nhóm hàng hóa nào đó, nó chỉ có thể được đặt vào thùng chứa có các đơn vị hàng hóa thuộc cùng nhóm hàng hóa đó.
- Nếu một đơn vị hàng hóa không thuộc bất kỳ nhóm hàng hóa nào, nó chỉ có thể được đặt vào thùng chứa có các đơn vị hàng hóa cũng không thuộc bất kỳ nhóm hàng hóa nào.

### Dịch vụ quản lý thùng chứa (Сервис тар)

Dịch vụ chịu trách nhiệm thực hiện thao tác thêm một đơn vị hàng hóa vào thùng chứa.

- Sử dụng Repository để truy xuất/lưu trữ dữ liệu về các đơn vị hàng hóa trong các thùng chứa.
- Khi thêm một đơn vị hàng hóa vào thùng chứa:
  - Truy xuất các đơn vị hàng hóa hiện đang có trong thùng chứa.
  - Kiểm tra xem việc thêm đơn vị hàng hóa có tuân thủ quy tắc tương thích hàng hóa hay không.
  - Kiểm tra xem việc thêm đơn vị hàng hóa có tuân thủ giới hạn ОВХ tối đa của thùng chứa hay không.
- Thao tác thêm có thể không thành công.

### Các quy tắc phân phối đơn vị hàng hóa

Logic phân phối các đơn vị hàng hóa được biểu diễn dưới dạng một cây quy tắc (Rule Tree).

#### Quy tắc cho thùng chứa (Для тар)

Đây là quy tắc đơn giản nhất.

Thực hiện việc thử thêm một đơn vị hàng hóa vào thùng chứa theo logic đã được mô tả trong dịch vụ quản lý thùng chứa.

#### Quy tắc cho pallet (Для палет)

Thực hiện việc thử thêm một đơn vị hàng hóa vào các thùng chứa nằm trên pallet, lần lượt theo các quy tắc dành cho thùng chứa, cho đến khi thao tác đầu tiên thành công.

#### Bộ định tuyến danh mục (Маршрутизатор категорий)

Quy tắc phân phối các đơn vị hàng hóa đến các quy tắc khác dựa trên danh mục của đơn vị hàng hóa.

- Chứa một ánh xạ giữa danh mục và quy tắc (Category–Rule Mapping).
- Chứa một quy tắc mặc định (Default Rule), được sử dụng nếu không tìm thấy quy tắc tương ứng với danh mục của đơn vị hàng hóa.

#### Các quy tắc bổ sung (Дополнительные правила)

Cần có khả năng bổ sung vào một quy tắc logic liên quan đến hạn sử dụng.

Một đơn vị hàng hóa chỉ có thể được đưa vào thùng chứa khi:

- Hạn sử dụng của nó chưa hết hoặc không có hạn sử dụng.
- Nó đã hết hạn sử dụng.

Cần có khả năng ghi log thông tin về kết quả thực thi quy tắc.

---

## Ghi chú

1. Có nhiều danh mục khác nhau, mỗi danh mục đều có một mã định danh.

Việc kiểm tra tính hợp lệ của cấu trúc lồng nhau giữa các quy tắc nằm ngoài phạm vi nghiệp vụ của bài thực hành này và không bắt buộc triển khai.

---

## Yêu cầu phi chức năng (Non-functional Requirements)

- Việc triển khai chức năng ghi log phải có khả năng kiểm thử. Ví dụ, không thể kiểm tra trực tiếp đầu ra trên Console khi gọi một số hành vi trong các bài kiểm thử. Cần có khả năng triển khai một đối tượng Mock trong các bài kiểm thử để đếm số lần gọi.
- Khi kiểm thử logic liên quan đến hạn sử dụng, cần có khả năng điều chỉnh thời gian hệ thống. Việc lấy thời gian hiện tại phải được ẩn sau một lớp trừu tượng (Abstraction).

## Các trường hợp kiểm thử (Test Cases)

### Test Case 1

Một thùng chứa đã có một đơn vị hàng hóa thuộc danh mục "giấy".

Quy tắc tương thích hàng hóa được cấu hình sao cho các danh mục "giấy", "kéo" và "đá" không giao nhau.

**Kết quả mong đợi:**
- Đơn vị hàng hóa thuộc danh mục "kéo" không được thêm vào thùng chứa.

### Test Case 2

Một thùng chứa có giới hạn ОВХ tối đa bằng đúng ОВХ của đơn vị hàng hóa đã có bên trong.

**Kết quả mong đợi:**
- Việc thêm đơn vị hàng hóa thứ hai, có ОВХ vượt quá phần dung lượng ОВХ còn lại, không thành công.

### Test Case 3

Một bộ định tuyến danh mục không có ánh xạ danh mục–quy tắc, nhưng có quy tắc mặc định là thêm vào thùng chứa.

**Kết quả mong đợi:**
- Đơn vị hàng hóa thuộc danh mục "bánh nướng" được thêm vào thùng chứa.

### Test Case 4

Một bộ định tuyến danh mục có ánh xạ:

- Danh mục "điện tử" → Quy tắc cho thùng chứa A.
- Danh mục "ФРОВ" → Quy tắc cho thùng chứa B.

**Kết quả mong đợi:**
- Đơn vị hàng hóa thuộc danh mục "điện tử" được thêm vào thùng chứa A.
- Đơn vị hàng hóa thuộc danh mục "ФРОВ" được thêm vào thùng chứa B.

Mỗi đơn vị hàng hóa được phân phối vào đúng thùng chứa theo ánh xạ đã cấu hình.

### Test Case 5

Một đơn vị hàng hóa không thuộc bất kỳ nhóm hàng hóa nào được thêm vào thùng chứa đã có một đơn vị hàng hóa thuộc một nhóm hàng hóa.

**Kết quả mong đợi:**
- Thao tác thêm không thành công.

### Test Case 6

Quy tắc ghi log (Logging Rule) được gọi trong cả hai trường hợp:

- Thêm đơn vị hàng hóa thành công.
- Thêm đơn vị hàng hóa không thành công.

**Kết quả mong đợi:**
- Logger ghi nhận cả hai kết quả.

### Test Case 7

Một pallet chứa hai thùng:

- Thùng thứ nhất dành cho các đơn vị hàng hóa đã hết hạn sử dụng.
- Thùng thứ hai dành cho các đơn vị hàng hóa chưa hết hạn sử dụng.

**Kết quả mong đợi:**
- Đơn vị hàng hóa đã hết hạn sử dụng được thêm vào thùng dành cho hàng hết hạn.
