## Thực hành môn CV - Buổi 7
### Sinh viên thực hiện: Trần Như Khả Ý _ 2374802010582
### GVHD: Đỗ Hữu Quân

---

# Công nghệ sử dụng
### Ngôn ngữ
-   **Python 3**
### Thư viện
-   **OpenCV (cv2)** -- Thư viện xử lý ảnh và thị giác máy tính mạnh mẽ.
-   **NumPy** -- Thao tác với mảng và ma trận trong xử lý ảnh.
-   **Matplotlib** -- Hiển thị và trực quan hóa ảnh.
-   **Scikit-learn** -- Sử dụng thuật toán **K-Means clustering** để
    phân cụm đặc trưng.

---

### PHẦN 1 – Image Thresholding & Geometric TransformationsFile: 2374802010582_TranNhuKhaY_Lab05.ipynb
#### Cách hoạt động: Notebook này tập trung vào việc thay đổi cấu trúc và phân loại điểm ảnh
1. Phân ngưỡng ảnh (Thresholding)
- Cơ chế: Sử dụng cv2.threshold để chuyển đổi ảnh xám thành ảnh nhị phân (trắng đen).
- Các phương pháp: - Simple Thresholding: Áp dụng một giá trị ngưỡng cố định cho toàn bộ ảnh.
  - Adaptive Thresholding: Ngưỡng thay đổi theo từng vùng nhỏ, giúp xử lý ảnh có độ sáng không đồng đều.
  - Otsu Algorithm: Tự động tìm ngưỡng tối ưu dựa trên phân bố histogram, đặc biệt hiệu quả trong phân đoạn ảnh vân tay.
- Code chính :
```PYTHON
# Thresholding 
ret, thresh1 = cv.threshold(img_gray, 127, 255, cv.THRESH_BINARY)
-----
# Otsu
    img2_gray = cv.cvtColor(img2, cv.COLOR_BGR2GRAY)
    ret2, th2 = cv.threshold(img2_gray, 0, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)
```
- KQ :
<img width="1814" height="880" alt="image" src="https://github.com/user-attachments/assets/e7d66a2c-1330-436c-bbd9-bdd176823ba9" />

<img width="1787" height="531" alt="image" src="https://github.com/user-attachments/assets/ce2f900a-3f31-478a-886a-db6df0d7c851" />

2. Phân đoạn ảnh nâng cao (Segmentation)
- Clustering (K-means): Phân nhóm các pixel có màu sắc/cường độ tương đồng vào $K$ cụm khác nhau.
- Code chính :
```python
criteria = (cv.TERM_CRITERIA_EPS + cv.TERM_CRITERIA_MAX_ITER, 10, 1.0)
K = 3 # Số lượng vùng muốn phân đoạn
_, labels, centers = cv.kmeans(pixel_values, K, None, criteria, 10, cv.KMEANS_RANDOM_CENTERS)
```
- KQ :
<img width="1246" height="957" alt="image" src="https://github.com/user-attachments/assets/71b9a412-27ce-4b28-839c-e212ed52444d" />

- Region Growing: Phát triển vùng từ một điểm hạt giống (seed) dựa trên sự tương đồng của các pixel lân cận.
- Code chính :
```python
def region_growing(img, seed):
    list_seeds = [seed]
    out_img = np.zeros_like(img)
    reg_threshold = 5
    while(len(list_seeds) > 0):
        pix = list_seeds.pop(0)
        out_img[pix[0], pix[1]] = 255
        for i in [-1, 0, 1]:
            for j in [-1, 0, 1]:
                if 0 <= pix[0] + i < img.shape[0] and 0 <= pix[1] + j < img.shape[1]:
                    if out_img[pix[0] + i, pix[1] + j] == 0 and abs(int(img[pix[0] + i, pix[1] + j]) - int(img[pix[0], pix[1]])) < reg_threshold:
                        out_img[pix[0] + i, pix[1] + j] = 255
                        list_seeds.append((pix[0] + i, pix[1] + j))
    return out_img
```
- KQ :
<img width="1313" height="921" alt="image" src="https://github.com/user-attachments/assets/509e95ec-40cf-4637-87f6-c1f5922be2fc" />

- Split and Merge: Chia nhỏ ảnh thành các vùng con (Split) và gộp chúng lại (Merge) dựa trên tính đồng nhất về cấu trúc.
- Code chính:
```python
def split_image(image, x, y, w, h, out_img, threshold):
    vung_anh = image[y:y+h, x:x+w]
    if is_homogeneous(vung_anh, threshold):
        # Nếu đồng nhất, gán giá trị trung bình cho toàn vùng
        average_color = np.mean(vung_anh)
        out_img[y:y+h, x:x+w] = average_color
        # Vẽ đường viền để dễ hình dung cấu trúc Quadtree (tùy chọn)
        cv.rectangle(out_img, (x, y), (x+w, y+h), (0), 1)
    else:
        # Nếu không đồng nhất, chia làm 4
        half_w = w // 2
        half_h = h // 2
 # Đệ quy cho 4 vùng con: Top-Left, Top-Right, Bottom-Left, Bottom-Right
        split_image(image, x, y, half_w, half_h, out_img, threshold)
        split_image(image, x + half_w, y, half_w, half_h, out_img, threshold)
        split_image(image, x, y + half_h, half_w, half_h, out_img, threshold)
        split_image(image, x + half_w, y + half_h, half_w, half_h, out_img, threshold)
```
- KQ :
<img width="1794" height="918" alt="image" src="https://github.com/user-attachments/assets/65c96eb2-fd01-4e82-af92-02f76cf390f9" />


- Edge-based Segmentation: Phân đoạn dựa trên việc tìm kiếm ranh giới giữa các vùng bằng các bộ lọc biên.
- Code chính:
```python
edges = cv.Canny(img_gray, 100, 200)

# Dùng morphology để đóng các lỗ hổng trên cạnh
kernel = np.ones((5,5), np.uint8)
closing = cv.morphologyEx(edges, cv.MORPH_CLOSE, kernel)
```
- KQ :
<img width="1145" height="939" alt="image" src="https://github.com/user-attachments/assets/078cc718-3653-4b2c-95f9-34084d106c97" />

---

### PHẦN 2 – Nhận diện đối tượng (Image Detection) : 2374802010582_TranNhuKhaY_Lab06.ipynb 
- Cơ chế: Sử dụng bộ phân loại Haar Cascade (tệp .xml) để quét ảnh và tìm kiếm các đặc trưng (Haar-like features).
- Ứng dụng :
  - Nhận diện khuôn mặt người (Face Detection).
  - Code chính :
```python
stop_data = cv2.CascadeClassifier('/content/haarcascade_frontalface_default.xml')
found = stop_data.detectMultiScale(img_gray,minSize =(20, 20))
amount_found = len(found)
# vẽ khung 
if amount_found != 0:
  for (x, y, width, height) in found:
      cv2.rectangle(img_rgb, (x, y),
      (x + height, y + width),
      (0, 255, 0), 5)
```
- KQ :
  <img width="1407" height="701" alt="image" src="https://github.com/user-attachments/assets/a3b84b77-97f4-43bc-900a-21864f5ff0e5" />

  - Nhận diện chi tiết: Mắt (Eyes), Nụ cười (Smile), Mũi, Miệng... bằng cách quét bên trong vùng khuôn mặt.
  - Code chính :
```python
# Load các bộ phân loại
face_data = cv2.CascadeClassifier("/content/haarcascade_frontalface_default.xml")
eye_data = cv2.CascadeClassifier("/content/haarcascade_eye.xml")
smile_data = cv2.CascadeClassifier("/content/haarcascade_smile.xml")
nose_data = cv2.CascadeClassifier("/content/haarcascade_mcs_nose.xml")
found = face_data.detectMultiScale(img_gray, minSize=(20, 20))
amount_found = len(found)

if amount_found != 0:
    for (x, y, width, height) in found:
        # Vẽ khung khuôn mặt (Màu Xanh lá)
        cv2.rectangle(img_rgb, (x, y), (x + width, y + height), (0, 255, 0), 3)

        # Cắt vùng ROI (vùng khuôn mặt) để tìm các bộ phận khác
        roi_gray = img_gray[y:y+height, x:x+width]
        roi_color = img_rgb[y:y+height, x:x+width]
        # 1. Detect mắt (Màu Xanh dương)
        eyes = eye_data.detectMultiScale(roi_gray)
        for (ex, ey, ew, eh) in eyes:
            cv2.rectangle(roi_color, (ex, ey), (ex + ew, ey + eh), (255, 0, 0), 2)
        # Detect miệng / nụ cười (Màu Đỏ)
        smiles = smile_data.detectMultiScale(roi_gray, 1.8, 20)
        for (sx, sy, sw, sh) in smiles:
            cv2.rectangle(roi_color, (sx, sy), (sx + sw, sy + sh), (0, 0, 255), 2)
        #  Detect Mũi (Màu Vàng)
        noses = nose_data.detectMultiScale(roi_gray, 1.3, 5)
        for (nx, ny, nw, nh) in noses:
            cv2.rectangle(roi_color, (nx, ny), (nx + nw, ny + nh), (255, 255, 0), 2)
```
- KQ :
<img width="1316" height="777" alt="image" src="https://github.com/user-attachments/assets/7e103004-aa23-4edf-80cb-21f0a7796ae6" />

---

# Tài liệu tham khảo

-   OpenCV Documentation\
-   Scikit-learn Documentation\
-   Digital Image Processing -- Gonzalez & Woods\
-   Computer Vision: Algorithms and Applications -- Richard Szeliski




