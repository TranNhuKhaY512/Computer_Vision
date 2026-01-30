## Thực hành môn CV - Buổi 4
## Lab 4: GEOMETRIC TRANSFORMATIONS (PIL & OPENCV)
### Sinh viên thực hiện: Trần Như Khả Ý _ 2374802010582
### GVHD: Đỗ Hữu Quân
---

### Giới thiệu chung
- Bài lab này nhằm mục đích giúp làm quen với các thao tác xử lý ảnh cơ bản trong Computer Vision, tập trung vào các phép biến đổi hình học (Geometric Operations) và các phép toán mảng, ma trận (Mathematical Operations) trong xử lý ảnh.
- Mục tiêu chính:
  - Thực hiện các phép Scaling, Translation, Rotation bằng OpenCV và PIL.
  - Áp dụng Array Operations và Matrix Operations trực tiếp lên ảnh.
  - Hiểu mối liên hệ giữa ảnh số và các phép toán đại số tuyến tính.
- Bài gồm 2 file .ipynb:
  - 2.4.1_Gemetric_trasfroms_PIL
  - 2.4.1_Gemetric_trasfroms_OpenCV
---

### Công nghệ sử dụng:
- Ngôn ngữ: Python 3
- Thư viện
  + NumPy: Dùng để biểu diễn ảnh dưới dạng mảng nhiều chiều và thao tác trực tiếp trên pixel.
  + Matplotlib: Dùng để hiển thị ảnh trong quá trình xử lý.
  + OpenCV (cv2): Thư viện xử lý ảnh mạnh, tối ưu hiệu năng, thường dùng trong thị giác máy tính.
  + PIL (Pillow): Là thư viện xử lý ảnh mức cao, dễ sử dụng và phù hợp cho các thao tác cơ bản như xoay, phóng to, thu nhỏ, lật ảnh.
---

### Cách hoạt động
#### I. Geometric Operations PIL
##### 1. Geometric Operations (Biến đổi hình học)
1.1 Đọc và hiển thị ảnh
Ảnh đầu vào được đọc bằng Image.open() và hiển thị bằng plt.imshow()
```python
image = Image.open("lenna.png")
plt.imshow(image)
plt.show()
```
1.2 Scaling (Thay đổi kích thước ảnh)
- Sử dụng hàm resize() của PIL để thay đổi chiều rộng và chiều cao ảnh.
- Có thể phóng to hoặc thu nhỏ ảnh theo từng trục.
- Code chính:
```python
width, height = image.size
new_image = image.resize((2 * width, height))
```
- KQ:
<img width="1310" height="679" alt="image" src="https://github.com/user-attachments/assets/c545d55f-fb5c-452b-9fac-c929959a9ad0" />

- Nhận xét: Scaling làm thay đổi kích thước ảnh nhưng không thay đổi nội dung hình học bên trong.
  
1.3 Rotation (Xoay ảnh)
- Sử dụng hàm rotate(theta) để xoay ảnh một góc xác định.
- Ví dụ: xoay ảnh 45 độ.
- Code chính:
```python
theta = 45
new_image = image.rotate(theta)
```
- KQ:
<img width="1117" height="895" alt="image" src="https://github.com/user-attachments/assets/22c14595-6f3d-49c0-a8ef-e3ab142b98d9" />

- Nhận xét: Rotation có thể làm xuất hiện vùng trống (màu đen) ở các góc ảnh.
---

##### 2. Mathematical Operations (Các phép toán toán học trên ảnh)
2.1 Array Operations (Phép toán mảng)
- Ảnh được chuyển từ PIL Image sang NumPy array để xử lý.
```PYTHON
image_np = np.array(image)
```
a) Cộng một hằng số vào ảnh:
- Làm tăng độ sáng của ảnh bằng cách cộng một giá trị cố định vào mỗi pixel.
```python
new_image = image_np + 20
```
- KQ:
<img width="1521" height="1114" alt="image" src="https://github.com/user-attachments/assets/12b239c9-6c73-4d93-80aa-3dea0b519ce6" />

b) Nhân ảnh với một hằng số
- Làm thay đổi độ tương phản của ảnh.
```PYTHON
new_image = 10 * image_np
```
- KQ:
<img width="1579" height="1031" alt="image" src="https://github.com/user-attachments/assets/8934bc3e-78de-47f1-9696-afe4526b63cd" />

c) Cộng và nhân hai mảng ảnh
- Tạo nhiễu Gaussian và cộng vào ảnh gốc để mô phỏng ảnh bị nhiễu.
```PYTHON
Noise = np.random.normal(0, 20, (height, width, 3)).astype(np.uint8)
new_image = image_np + Noise
```
- KQ:
<img width="1557" height="1118" alt="image" src="https://github.com/user-attachments/assets/806f7e94-60e1-4801-84b4-b8ccac12530f" />

- Nhận xét: Các phép toán mảng giúp mô phỏng nhiễu và tăng cường dữ liệu ảnh.
---

2.2 Matrix Operations (Phép toán ma trận)
2.2.1 Chuyển ảnh sang ảnh xám
- Ảnh xám được xem như một ma trận 2 chiều.
```PYTHON
from PIL import ImageOps
im_gray = ImageOps.grayscale(Image.open("barbara.png"))
im_gray = np.array(im_gray)
```
2.2.2 Singular Value Decomposition (SVD)
Áp dụng SVD để phân rã ảnh thành 3 ma trận: U, S, V.

```PYTHON
U, s, V = np.linalg.svd(im_gray, full_matrices=True)
```
- Tạo ma trận đường chéo S từ vector s.
```PYTHON
S = np.zeros((im_gray.shape[0], im_gray.shape[1]))
S[:im_gray.shape[0], :im_gray.shape[0]] = np.diag(s)
```
<img width="1756" height="1001" alt="image" src="https://github.com/user-attachments/assets/38f882b0-2e33-4e41-86df-faeca28aa977" />

2.2.3 Tái tạo và xấp xỉ ảnh
- Tái tạo ảnh gốc bằng phép nhân ma trận:
```PYTHON
A = U.dot(S.dot(V))
```
<img width="1814" height="1186" alt="image" src="https://github.com/user-attachments/assets/87fdac2a-e975-42f3-9f43-ff77c0ca6909" />

- Xấp xỉ ảnh với số thành phần nhỏ hơn để giảm dữ liệu:
```PYTHON
for n_component in [1, 10, 100, 200, 500]:
    S_new = S[:, :n_component]
    V_new = V[:n_component, :]
    A = U.dot(S_new.dot(V_new))
```
<img width="1375" height="1124" alt="image" src="https://github.com/user-attachments/assets/18d74924-6b80-4a0e-96c6-445291522e46" />

- Nhận xét: Chỉ cần khoảng 100–200 components đã có thể biểu diễn ảnh khá tốt.
--- 

#### 2. Geometric Operations OpenCV
##### 1. Geometric Operations
1.1 Scaling (Thay đổi kích thước ảnh)
- Sử dụng hàm cv2.resize() để phóng to hoặc thu nhỏ ảnh.
- Có thể scale theo từng trục hoặc chỉ định kích thước cụ thể.
- Code chính:
```PYTHON
new_image = cv2.resize(image, None, fx=2, fy=1, interpolation=cv2.INTER_CUBIC)
```
- fx, fy: hệ số scale theo trục x và y.
- interpolation: phương pháp nội suy (INTER_NEAREST, INTER_CUBIC).
<img width="1720" height="892" alt="image" src="https://github.com/user-attachments/assets/6ccff491-b516-4434-8f51-418e4135a4f2" />

1.2 Translation (Tịnh tiến ảnh)
- Dịch chuyển ảnh theo phương ngang (tx) và dọc (ty) bằng ma trận biến đổi affine.
- Code chính:
```PYTHON
tx, ty = 100, 0
M = np.float32([[1, 0, tx], [0, 1, ty]])
new_image = cv2.warpAffine(image, M, (cols + tx, rows + ty))
```
- Các vùng không có giá trị pixel sau dịch chuyển sẽ được gán bằng 0 (màu đen).
<img width="1268" height="878" alt="image" src="https://github.com/user-attachments/assets/ba43d647-ffb5-4835-a018-b1e2cf1e2f49" />

1.3 Rotation (Xoay ảnh)
- Xoay ảnh quanh tâm bằng ma trận quay getRotationMatrix2D().
- Code chính:
```PYTHON
M = cv2.getRotationMatrix2D(center=(cols//2, rows//2), angle=45, scale=1)
new_image = cv2.warpAffine(image, M, (cols, rows))
```
- Góc quay dương tương ứng xoay ngược chiều kim đồng hồ.
<img width="944" height="725" alt="image" src="https://github.com/user-attachments/assets/17fbc38e-3e8b-4b1a-8948-39e0387da9ff" />

##### 2. Mathematical Operations
2.1 Array Operations
- Thực hiện các phép toán cộng, nhân trực tiếp lên toàn bộ pixel ảnh bằng broadcasting của NumPy.
- Code chính:
```PYTHON
new_image = image + 20
new_image = image * 10
```
- Có thể cộng hoặc nhân ảnh với nhiễu ngẫu nhiên để mô phỏng nhiễu ảnh.
<img width="2000" height="938" alt="image" src="https://github.com/user-attachments/assets/96f4d28f-01b0-4628-9b9f-ac06b9588ece" />
<img width="1710" height="903" alt="image" src="https://github.com/user-attachments/assets/b4d98847-58a2-4498-8a96-9d806b439a88" />

2.2 Matrix Operations – Singular Value Decomposition (SVD)
- Ảnh xám được xem như một ma trận 2D.
- Áp dụng SVD để phân rã ảnh thành 3 ma trận: U, S, V.
```PYTHON
U, s, V = np.linalg.svd(im_gray, full_matrices=True)
```
- Tạo ma trận đường chéo S từ vector s.
```PYTHON
S = np.zeros((im_gray.shape[0], im_gray.shape[1]))
S[:im_gray.shape[0], :im_gray.shape[0]] = np.diag(s)
```
- Khôi phục ảnh bằng số lượng thành phần nhỏ hơn để xấp xỉ ảnh gốc.
<img width="1736" height="851" alt="image" src="https://github.com/user-attachments/assets/1f1debae-9826-4f8a-bb8b-82348d9d417d" />

- Thực nghiệm cho thấy chỉ cần 100–200 components vẫn giữ được chất lượng ảnh tốt.
<img width="1697" height="1121" alt="image" src="https://github.com/user-attachments/assets/1348324f-bf0f-4659-95dd-231aae8e1cbf" />

---

#### 3. Bài tập LAB02:
3.1 Đọc ảnh bằng OpenCV và hiển thị chiều ảnh
- Dùng cv2.imread()
- Code chính:
```python
img = cv2.imread('cat.jpg')
img = img[:,:,::-1]
```
3.2 2. Làm mờ ảnh bằng Box Filter hoặc Blur
- Giảm nhiễu bằng cách lấy trung bình giá trị pixel trong vùng lân cận.
- Code chính:
```python
img2= cv2.blur(img, (3,3))
img2= cv2.blur(img, (5,5))
img3 = cv2.boxFilter(img, -1, (5,5))
```
- KQ:
<img width="756" height="1058" alt="image" src="https://github.com/user-attachments/assets/68a197da-e097-45e6-a6d8-c082759a6e40" />
<img width="876" height="965" alt="image" src="https://github.com/user-attachments/assets/3ab8c4b7-fa9e-46a9-b60b-9783f2693f22" />
<img width="1148" height="1081" alt="image" src="https://github.com/user-attachments/assets/c314f164-bf4a-40d1-b9e1-f9082f88feb2" />

3.3 Gaussian Filter
- Dùng làm mờ ảnh theo phân phối Gaussian, giữ biên tốt hơn blur thường.
- Code chính:
```python
img2 = cv2.GaussianBlur(img, (11,11) ,0)
```
- KQ:
<img width="908" height="951" alt="image" src="https://github.com/user-attachments/assets/5b5ef272-58f4-4396-8fd1-fd9e5797e8c9" />

3.4 Median Filter và xử lý Pepper Noise
- Dùng loại bỏ nhiễu muối tiêu (salt & pepper), lấy trung vị thay vì trung bình
- Code chính:
```python
img3 = cv2.medianBlur(img, 15,0)
```
- KQ:
<img width="864" height="917" alt="image" src="https://github.com/user-attachments/assets/d4541049-3cc4-4252-887a-db168a54d0c9" />

3.5 Kiểm tra các bộ lọc trên với ảnh peppernoise:
- Cho thấy sự khác nhau giữa các bộ lọc và thông số khác
- Code chính:
```python
img_pepper01a= cv2.blur(img_pepper01, (3,3))
img_pepper01b = cv2.GaussianBlur(img_pepper01, (11,11) ,0)
img_pepper01c = cv2.medianBlur(img_pepper01, 15,0)
```
- KQ:
<img width="1002" height="711" alt="image" src="https://github.com/user-attachments/assets/fceb3443-e4a5-4f6b-a0c4-f1e4487c019d" />

3.6 Cân bằng sáng – Histogram Equalization
- Dùng tăng độ tương phản ảnh xám.
- Code chính:
```python
blur = cv2.cvtColor(blur, cv2.COLOR_RGB2GRAY)
equalized = cv2.equalizeHist(blur)
```
- KQ:
<img width="1476" height="510" alt="image" src="https://github.com/user-attachments/assets/b01b56cf-b0fa-48cb-b669-973e1ad897a3" />

3.7 Phép toán số học & logic trên ảnh
a. Negative image: Làm nổi bật vùng sáng – tối, đảo giá trị pixel: 255 - pixel
```python
inv_img = cv2.bitwise_not(img)
```
<img width="1141" height="898" alt="image" src="https://github.com/user-attachments/assets/dc8e7c5a-270f-4cf6-8daa-d59ee0a15c00" />

b. Trừ ảnh & tách vùng khác biệt
- Code chính:
```python
diff = cv2.subtract(inv_img1, inv_img2)
gray = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)
_, mask = cv2.threshold(gray, 15, 255, cv2.THRESH_BINARY)
```
- Dùng để phát hiện vùng khác nhau giữa 2 ảnh
- Kết hợp threshold và morphology để làm sạch nhiễu
- KQ:
<img width="908" height="707" alt="image" src="https://github.com/user-attachments/assets/c13306b8-22db-4de0-8b33-b45d2528f805" />

3.8. Phát hiện biên và hình học
a. Gradient Filters (Forward, Backward, Central):
- Tính đạo hàm ảnh
- Central gradient cân bằng nhất
- Code chính:
```python
kx_f = np.array([[0, -1, 1]], dtype=np.float32)
kx_b = np.array([[-1, 1, 0]], dtype=np.float32)
kx_c = np.array([[-1, 0, 1]], dtype=np.float32) / 2
```
<img width="1733" height="671" alt="image" src="https://github.com/user-attachments/assets/c6e0ea4a-6a21-4622-95b1-6fe4d7d21b5b" />

b. Finite Difference Filter:
- Phát hiện thay đổi cường độ theo trục X, Y
- Kết hợp để phát hiện biên
- Code chính:
```python
k_fd_x = np.array([[1, -1]], dtype=np.float32)
k_fd_y = np.array([[1], [-1]], dtype=np.float32)
```
<img width="1647" height="613" alt="image" src="https://github.com/user-attachments/assets/e0a09fb4-f6c2-475c-8e68-7c3eff6f266e" />

c. Gaussian filter để lọc ảnh:
- Làm mờ ảnh theo phân phối Gaussian, giữ biên tốt hơn blur thường.
- Code chính
```python
gauss = cv2.GaussianBlur(gray, (5, 5), 0)
```
<img width="1319" height="609" alt="image" src="https://github.com/user-attachments/assets/94ceacd9-f7d6-42ab-ba4e-9ae2c1750598" />

d. Sobel Edge Detector
- Phát hiện biên theo hướng ngang & dọc
- Giữ biên tốt hơn gradient đơn giản
- Code chính:
```python
sx = cv2.Sobel(gray, cv2.CV_64F, 1, 0, ksize=3)
sy = cv2.Sobel(gray, cv2.CV_64F, 0, 1, ksize=3)
```
<img width="1698" height="635" alt="image" src="https://github.com/user-attachments/assets/eee91f92-af93-49b6-9df9-db42ede247b4" />

e. Canny edge detector 
- Phát hiện biên mạnh, rõ
- Ít nhiễu, dùng phổ biến trong thực tế
- Code chính:
```python
edges = cv2.Canny(gray, 100, 200)
```
<img width="1362" height="616" alt="image" src="https://github.com/user-attachments/assets/2804e918-76f6-47ed-b5fd-b5d947c75ae9" />

f. Hough Transform
- Biến đổi không gian ảnh → không gian tham số
- Phát hiện các đường thẳng trong ảnh
- Code chính:
```python
lines = cv2.HoughLinesP(
    edges,
    rho=1,
    theta=np.pi/180,
    threshold=100,
    minLineLength=50,
    maxLineGap=10
)
```
<img width="942" height="757" alt="image" src="https://github.com/user-attachments/assets/8e6637df-df14-47ee-90c4-01ccad6dd284" />

---

###  Tài liệu tham khảo 
- Tài liệu thực hành – ĐH Văn Lang  
- Pillow Documentation  
- OpenCV Documentation
- Gonzalez & Woods – Digital Image Processing, 2017







