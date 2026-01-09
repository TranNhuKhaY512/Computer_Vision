# Thị Giác Máy Tính  
## Buổi 1 – Xử lý ảnh với PIL (Pillow) và OpenCV (cv2)

---

**Sinh viên thực hiện:** Trần Như Khả Ý  
**MSSV:** 2374802015082
**Môn học:** Thị giác máy tính  
**Giảng viên:** Đỗ Hữu Quân

---

## 1. Giới thiệu  
Trong môn **Thị giác máy tính (Computer Vision)**, ảnh số đóng vai trò là dữ liệu đầu vào quan trọng nhất. Để giải quyết các bài toán như nhận diện khuôn mặt, phân loại ảnh hay xử lý video, trước hết cần hiểu cách ảnh được biểu diễn và xử lý bằng lập trình.

Trong bài thực hành, làm việc với **hai thư viện phổ biến của Python là PIL (Pillow) và OpenCV (cv2)** để thực hiện các thao tác xử lý ảnh cơ bản.

---

## 2. Mục tiêu bài Lab  
Sau khi hoàn thành bài lab, ta sẽ học được:
- Đọc ảnh từ file bằng PIL và OpenCV
- Hiển thị ảnh bằng thư viện matplotlib
- Hiểu ảnh số được biểu diễn dưới dạng mảng pixel
- Chỉnh sửa pixel
- Chuyển ảnh màu sang ảnh xám
- Tách và xử lý các kênh màu RGB
- Phân biệt giữa sao chép ảnh và gán ảnh
- Nhận biết ưu và nhược điểm của PIL và OpenCV

---

## 3. Công cụ và thư viện sử dụng  
- **Python** :Ngôn ngữ lập trình chính được sử dụng trong bài lab, có cú pháp đơn giản, dễ học và được hỗ trợ mạnh trong lĩnh vực xử lý ảnh và thị giác máy tính.
- **PIL (Pillow)** : Thư viện hỗ trợ xử lý ảnh cơ bản trong Python như đọc ảnh, hiển thị ảnh, chuyển đổi chế độ màu và chỉnh sửa pixel.
- **OpenCV (cv2)** : OpenCV hỗ trợ xử lý ảnh, xử lý video, phát hiện đối tượng và nhiều thuật toán nâng cao với hiệu suất cao.
- **NumPy** : Thư viện làm việc với mảng số đa chiều. Trong xử lý ảnh, NumPy giúp biểu diễn ảnh dưới dạng mảng pixel và thực hiện các phép toán nhanh trên ảnh.
- **Matplotlib** : Thư viện dùng để hiển thị ảnh và trực quan hóa dữ liệu
---

## 4. Nội dung thực hành  

### 4.1. Đọc và hiển thị ảnh  
Sử dụng:
- `Image.open()` với PIL  
- `cv2.imread()` với OpenCV  
Sau đó hiển thị ảnh bằng `matplotlib.pyplot.imshow()`.

---
### 4.2. Kiểm tra thông tin ảnh  
- Kích thước ảnh (rows, columns)  
- Số kênh màu  
- Kiểu dữ liệu pixel  
- Mục đích: hiểu ảnh được lưu trữ dưới dạng mảng số.

---
### 4.3. Chỉnh sửa pixel  
- Thay đổi giá trị pixel tại vị trí xác định  
- Quan sát ảnh sau khi chỉnh sửa  

---
### 4.4. Chuyển ảnh sang ảnh xám  
- PIL: sử dụng `convert("L")`  
- OpenCV: sử dụng `cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)`  
- Mục đích : giúp đơn giản hóa dữ liệu ảnh.

---
### 4.5. Giảm mức xám  
Thực hiện giảm số mức xám để quan sát ảnh hưởng đến chất lượng ảnh và khả năng phân biệt chi tiết.

---
### 4.6. Tách kênh màu RGB  
- Tách từng kênh **Red – Green – Blue**  
- Quan sát ảnh chỉ còn một kênh màu  
- Hiểu sự đóng góp của từng kênh vào ảnh gốc  

---
### 4.7. Sao chép và gán ảnh  
- Phân biệt `copy()` và phép gán trực tiếp  
- Hiểu khái niệm tham chiếu bộ nhớ trong NumPy  

---
## 5. So sánh PIL và OpenCV  

- **PIL (Pillow)**:  
  - Dễ sử dụng  
  - Phù hợp cho người mới học  
  - Thích hợp cho xử lý ảnh cơ bản  

- **OpenCV (cv2)**:  
  - Hiệu suất cao  
  - Hỗ trợ nhiều thuật toán CV  
  - Dùng cho các bài toán thực tế và AI  

---
## 6. Cách chạy chương trình  
Cài đặt thư viện cần thiết:

```bash
pip install pillow opencv-python numpy matplotlib
```
---
## 7. Tài liệu tham khảo  
- Tài liệu thực hành – ĐH Văn Lang  
- Pillow Documentation  
- OpenCV Documentation


