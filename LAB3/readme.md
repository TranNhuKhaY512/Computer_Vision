## Thực hành môn CV - Buổi 3 
## Lab 3: Histogram and Intensity Transformations
### Sinh viên thực hiện: Trần Như Khả Ý _ 2374802010582
### GVHD: Đỗ Hữu Quân
---

### Giới thiệu chung
- Bài lab này nhằm mục đích giúp làm quen với các thao tác xử lý ảnh cơ bản trong Computer Vision, tập trung vào:
  - Histogram và các phép biến đổi cường độ ảnh.
  - Các thao tác xử lý ảnh cơ bản bằng Pillow (PIL) và OpenCV.
  - Giới thiệu và làm việc với tập dữ liệu Uber phục vụ cho phân tích dữ liệu không gian.
- Bài gồm 2 file .ipynb:
  - 2.3.2_ Histograms and Intensity Transformations.ipynb : Thao tác xử lý ảnh cơ bản, Histogram và biến đổi cường độ ảnh
  - Consumption.ipynb : Giới thiệu và thao tác với tập dữ liệu Uber
---

### Công nghệ sử dụng:
- Ngôn ngữ: Python 3
- Thư viện
  + NumPy: Dùng để biểu diễn ảnh dưới dạng mảng nhiều chiều và thao tác trực tiếp trên pixel.
  + Matplotlib: Dùng để hiển thị ảnh trong quá trình xử lý.
  + OpenCV (cv2): Thư viện xử lý ảnh mạnh, tối ưu hiệu năng, thường dùng trong thị giác máy tính.
  + Pandas (pd) : dùng để thao tác với bảng (dạng csv, excel...)
---

### Cách hoạt động
#### 1. Thao tác xử lý ảnh cơ bản, Histogram và biến đổi cường độ ảnh
1.1. Đọc ảnh đầu vào bằng `cv2.imread()`
1.2. Chuyển ảnh sang ảnh xám (grayscale)
```python
plt.imshow(goldhill,cmap="gray")
```
1.3. Tính và vẽ Histogram
```python
hist = cv2.calcHist([goldhill],[0], None, [256], [0,256])  # tính
intensity_values = np.array([x for x in range(hist.shape[0])]) # vẽ
```
- KQ:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2f93085f-82f1-4816-8f85-ac51a834362d" />

---

1.4. Áp dụng các phép biến đổi cường độ ảnh
1.4.1 Image Negatives -  biến đổi âm bản:  
- Công thức: s = L - 1 - r
- Code chính: 
```python
#neg_toy_image = -1 * toy_image + 255
neg_toy_image = -1 * toy_image.astype('int16') + 255
neg_toy_image = neg_toy_image.astype('uint8')  # Chuyển lại về uint8 sau khi tính toán

print("toy image\n", neg_toy_image)
print("image negatives\n", neg_toy_image)
```
- KQ:
<img width="1148" height="542" alt="image" src="https://github.com/user-attachments/assets/8f23b793-2c9c-4add-b8a6-8f7c010698e6" />

---

1.4.2 Brightness & Contrast Adjustment - phép biến đổi độ sáng & độ tương phản tuyến tính:
- Sử dụng hàm convertScaleAbs(). Dùng để chia tỉ lệ, tính toán giá trị vaf chuyển sang định dạng 8bit.
- Công thức: $g(x,y) = \alpha f(x,y) + \beta$.
- Code chính: 
```python
new_image = cv2.convertScaleAbs(goldhill, alpha=alpha, beta=beta)
```
- KQ:
<img width="827" height="422" alt="image" src="https://github.com/user-attachments/assets/6fa5e16c-f814-4cda-9be2-0408fe78730e" />
<img width="416" height="250" alt="image" src="https://github.com/user-attachments/assets/7dccd0f1-eee1-484b-9cea-98a31261cce5" />

---

<img width="1641" height="681" alt="image" src="https://github.com/user-attachments/assets/928db045-774a-4929-b3f4-8b0ba4827ac9" />
<img width="1050" height="570" alt="image" src="https://github.com/user-attachments/assets/6da995b5-a7a4-4575-bde5-5078aa913ac6" />

---

1.4.3 Histogram Equalization - cân bằng biểu đồ:
- Dùng để làm tăng độ tương phản của hình ảnh bằng cách mở rộng phạm vi của các pixel thang độ xám.
- Code chính: 
```python
zelda = cv2.imread("zelda.png",cv2.IMREAD_GRAYSCALE)
new_image = cv2.equalizeHist(zelda)
```
- KQ:
<img width="930" height="457" alt="image" src="https://github.com/user-attachments/assets/84a1b427-4d5f-48c4-872b-95fcaa17bbca" />
<img width="925" height="532" alt="image" src="https://github.com/user-attachments/assets/f4782b37-fd95-48f9-a534-8ee7942f1af7" />

---

1.4.4 Thresholding and Simple Segmentation
- Dùng để phân đoạn , trích xuất các đối tượng từ một bức ảnh.
- Code chính:
```python
thresholding_toy = thresholding(toy_image, threshold=threshold, max_value=max_value, min_value=min_value)
```
- KQ:
<img width="1442" height="628" alt="image" src="https://github.com/user-attachments/assets/5afe7c42-8ee4-445c-a506-d9d2c2a07bc3" />
---

<img width="722" height="767" alt="image" src="https://github.com/user-attachments/assets/d3d93bce-7c05-4cdd-bda6-adabbb1f1396" />

---

- Ngưỡng cơ bản: THRESH_BINARY: là kiểu dữ liệu được dùng trong hàm thresholding và nó chỉ là con số.
- Code chính:
```python
 ret, new_image = cv2.threshold(image,threshold,max_value,cv2.THRESH_BINARY)
```
- KQ:
<img width="965" height="729" alt="image" src="https://github.com/user-attachments/assets/3929778c-4284-4c43-a8aa-e213b2af7d02" />

---

- THRESHOLDING_TRUNC : không làm thay đổi giá trị nếu các pixel nhỏ hơn giá trị ngưỡng
- Code chính:
```python
ret, new_image = cv2.threshold(image,86,255,cv2.THRESH_TRUNC)
```
- KQ:
<img width="751" height="680" alt="image" src="https://github.com/user-attachments/assets/c94afefc-63b7-4584-b16e-07653d9f8f7a" />

---

- THRESHOLDING_OTSU: là phương pháp tránh việc chọn một giá trị và tự động xác định giá trị đó bằng sử dụng biểu đồ tần số.
- Code chính:
```python
ret, otsu = cv2.threshold(image,0,255,cv2.THRESH_OTSU)
```
- KQ:
<img width="759" height="666" alt="image" src="https://github.com/user-attachments/assets/24d34b1c-417e-43c1-aa70-5fa96ab84b98" />

--- 

#### 2. Giới thiệu và thao tác với tập dữ liệu    Uber_basic_image_manipulation_open_CV.ipynb):
2.1. Đọc và hiển thị dữ liệu từ file data 
-  File data được đọc bằng pd.read_csv()
- Code chính:
```python
uber_data = pd.read_csv("uber-raw-data-jul14.csv")
print(uber_data.tail(10))
uber_data.info()
```
- KQ :
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/345bc3e0-5f51-43f0-b18c-8d3f366de972" />

---

2.2. Xử lý dữ liệu thời gian (Datetime):
- Chuyển Date/Time sang kiểu datetime, làm tròn thời gian theo giờ và đếm số chuyến theo từng giờ
- Code chính:
```python
uber_data["Date/Time"] = pd.to_datetime(uber_data["Date/Time"])
hourly_data = uber_data["Date/Time"].dt.floor('1h').value_counts()
hourly_data = hourly_data.sort_index()
```
- KQ:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9ffeba2b-64aa-42ca-a169-feeed92c0d9a" />

---

<img width="1827" height="402" alt="image" src="https://github.com/user-attachments/assets/9afe34d9-f574-41ff-952f-902ad10de130" />

---

2.3. Phân tích theo ngày trong tuần & giờ
- Tách: giờ (Hour), thứ trong tuần (Week Day), gom nhóm theo Date – Week Day – Hour, và lấy trung bình số chuyến
- Code chính:
```python
weekly_data = weekly_data.groupby(["Date","Week Day", "Hour"]).size()
print(weekly_data.head(10))
weekly_data = weekly_data.unstack(level=0)
print(weekly_data)
```
- KQ:
<img width="1747" height="377" alt="image" src="https://github.com/user-attachments/assets/13b48475-70af-4fd1-991b-5adb8f9fe24d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c7d1ce4d-4585-4219-9504-08c445200fc3" />

---

2.4 Trực quan hóa Heatmap (Seaborn)
- Dùng heatmap để thể hiện mật độ chuyến đi, màu càng đậm → số chuyến càng nhiều.
- Code chính:
```python
import seaborn as sns
#Plot a heatmap of the data
sns.heatmap(weekly_data)
plt.show()
```
- KQ:
<img width="694" height="619" alt="image" src="https://github.com/user-attachments/assets/4ea0f1a4-aa3b-4b32-92c1-04edd8dc4ee1" />

---

2.5 hân tích khoảng cách địa lý
- Tính khoảng cách từ điểm Uber đến: Metropolitan Museum , Empire State Building, sử dụng Haversine formula.
- Code chính:
```python
Metro_art_coordinates = (40.7794, -73.9632)      # New York (Mỹ)
empire_state_building_coordinates = (11.291434, 106.994658)  # Việt Nam (Bình Dương)
def haversine(coordinates1, coordinates2):
...
```
- KQ:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6af2622-defa-4076-84f6-f7109303979b" />

---

2.6 Phân tích số chuyến theo bán kính
- Đếm số chuyến trong các khoảng cách tăng dần
- Vẽ biểu đồ tích lũy
- Code chính:
```python
distance_range = np.arange(.1,5.1,.1)
distance_data.plot(kind="line")
plt.show()
```
- KQ:
<img width="1182" height="618" alt="image" src="https://github.com/user-attachments/assets/4e41e574-d00f-4927-8868-431900de9dfe" />

---

2.7 Bản đồ & Heatmap với Folium
- Hiển thị vị trí Uber trên bản đồ:  Trường Đại Học Văn Lang cơ sở chính
- Dùng: Marker, HeatMap, HeatMapWithTime
- Code chính:
```python
uber_map = folium.Map(location=[10.82841, 106.69990], zoom_start=25)
```
- KQ:
<img width="1442" height="823" alt="image" src="https://github.com/user-attachments/assets/2c86ce43-14ee-40f8-af13-32b5c55cc3e3" />

---

3. Bài tập LAB01:
- Kết nối với drive để đọc ảnh
```python
from google.colab import drive
drive.mount('/content/drive')

 # Tạo thư mục lưu model trong Drive
!mkdir -p "/content/drive/MyDrive/CV_2024_data"
```
3.1 Đọc ảnh bằng OpenCV và hiển thị chiều ảnh
- Dùng cv2.imread()
- Code chính:
```python
img = cv2.imread("/content/drive/MyDrive/CV_2024_data/picture.png")
print(type(img))
print(img.shape)
```
3.2 Hiển thị ảnh bằng plt.show(thay plt bằng TranNhuKhaY)
- Dùng để trực quan hóa
- Code chính:
```python
TranNhuKhaY.imshow(img)
TranNhuKhaY.title("Image using plt.imshow")
TranNhuKhaY.axis("off")
TranNhuKhaY.show()
```
- KQ:
<img width="1630" height="622" alt="image" src="https://github.com/user-attachments/assets/00b4bf54-527b-4049-af33-5f29299eddc8" />
- Cách hiển thị đúng ảnh:
  - Cách 1: đảo chiều matrix.
  - Code chính:
```python
img_rgb = img[:, :, ::-1]
TranNhuKhaY.imshow(img_rgb)
TranNhuKhaY.axis("off")
TranNhuKhaY.show()
```
  - KQ :
<img width="952" height="693" alt="image" src="https://github.com/user-attachments/assets/d0fe8ec6-6fe4-484e-b5e6-d010dd828460" />

  - Cách 2: convert chế độ màu
  - Code chính:
```python
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
TranNhuKhaY.imshow(img_rgb)
TranNhuKhaY.axis("off")
TranNhuKhaY.show()
```
  - KQ:
<img width="931" height="692" alt="image" src="https://github.com/user-attachments/assets/6dc0b885-2c13-485a-8b6a-356f7373b9fe" />

3.3 Phóng to ảnh:
- Dùng cv2.resize()
- Code chính:
```python
h, w = img.shape[:2]
img_resize = cv2.resize(img, (w*2, h*2))
TranNhuKhaY.imshow(cv2.cvtColor(img_resize, cv2.COLOR_BGR2RGB))
```
- KQ:
<img width="1124" height="844" alt="image" src="https://github.com/user-attachments/assets/1f57bce0-8f14-4d3f-9f6f-aa22f10c5ca5" />

3.4 Crop ảnh:
- Crop bằng slicing NumPy, cắt ảnh: y1:y2, x1:x2
- Code chính:
```python
crop_img = img[100:300, 150:400]

TranNhuKhaY.imshow(cv2.cvtColor(crop_img, cv2.COLOR_BGR2RGB))
```
- KQ:
<img width="1009" height="988" alt="image" src="https://github.com/user-attachments/assets/a5901c85-8e3a-4ff3-a161-c8bfe697fd19" />

3.5 Ghép 2 ảnh (ngang & dọc)
- Ghép ngang 
- Code chính:
```python
import numpy as np
horizontal = np.hstack((img1, img2))
TranNhuKhaY.imshow(cv2.cvtColor(horizontal, cv2.COLOR_BGR2RGB))
```
- KQ:
<img width="1175" height="952" alt="image" src="https://github.com/user-attachments/assets/30500b18-db81-4e1c-90ec-2c99f7c56c66" />

- Ghép dọc
- Code chính:
```python
vertical = np.vstack((img1, img2))

TranNhuKhaY.imshow(cv2.cvtColor(vertical, cv2.COLOR_BGR2RGB))
```
- KQ :
<img width="1125" height="849" alt="image" src="https://github.com/user-attachments/assets/b7dd93d0-2755-4268-940d-2cbc086fd196" />

###  Tài liệu tham khảo 
- Tài liệu thực hành – ĐH Văn Lang  
- Pillow Documentation  
- OpenCV Documentation
- Gonzalez & Woods – Digital Image Processing, 2017







