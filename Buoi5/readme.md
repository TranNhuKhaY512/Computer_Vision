## Thực hành môn CV - Buổi 5
## Lab 5: Spatial Filtering (PIL & OPENCV)
### Sinh viên thực hiện: Trần Như Khả Ý _ 2374802010582
### GVHD: Đỗ Hữu Quân
---

### Giới thiệu chung
- Bài lab này nhằm giúp sinh viên làm quen với các thao tác xử lý ảnh cơ bản trong **Computer Vision**, đặc biệt là các **phép biến đổi hình học (Geometric Operations)** và các **phép toán trên mảng, ma trận (Mathematical Operations)** trong xử lý ảnh số.
- Mục tiêu chính
  - Thực hiện các phép **Scaling (phóng to/thu nhỏ)**, **Translation (tịnh tiến)** và **Rotation (xoay ảnh)** bằng thư viện **OpenCV** và **PIL**.
  - Áp dụng các **Array Operations** và **Matrix Operations** trực tiếp lên ảnh số.
  - Hiểu mối liên hệ giữa **ảnh số** và **các phép toán đại số tuyến tính** trong xử lý ảnh.
- Bài gồm 2 file .ipynb:
  - 2.5.1_Spatial_Filtering-PIL.ipynb
  - 2.5.1_Spatial_Filtering.ipynb
  - 2374802010582_TranNhuKhaY_CV0101_Lab3.ipynb
---

### Công nghệ sử dụng
- **Ngôn ngữ:** Python 3  
- **Thư viện:**
  - **NumPy:** Dùng để biểu diễn ảnh dưới dạng mảng nhiều chiều (multi-dimensional array) và thao tác trực tiếp trên các giá trị pixel.
  - **Matplotlib:** Dùng để hiển thị và trực quan hóa ảnh trong quá trình xử lý.
  - **OpenCV (cv2):** Thư viện xử lý ảnh mạnh mẽ, tối ưu hiệu năng, thường được sử dụng trong các bài toán **Computer Vision**.
  - **PIL (Pillow):** Thư viện xử lý ảnh mức cao, dễ sử dụng, phù hợp cho các thao tác cơ bản.
  - **scikit-image:** Cung cấp các thuật toán xử lý ảnh như **Difference of Gaussians**.
  - **SciPy FFT:** Thực hiện **Fast Fourier Transform** để phân tích ảnh trong miền tần số.

---

- Các kỹ thuật xử lý ảnh được sử dụng trong bài lab bao gồm:
  - Linear Filtering: Bộ lọc tuyến tính sử dụng kernel để thực hiện phép tích chập nhằm làm mịn hoặc biến đổi ảnh.
  - Gaussian Blur: Bộ lọc làm mờ ảnh dựa trên phân bố Gaussian, giúp giảm nhiễu và làm mịn ảnh.
  - Image Sharpening: Kỹ thuật làm rõ chi tiết ảnh bằng cách tăng cường các cạnh thông qua bộ lọc thông cao.
  - Noise Filtering: Kỹ thuật loại bỏ nhiễu trong ảnh bằng các bộ lọc như Gaussian hoặc Median.
---

# PHẦN 1 – Spatial Filtering với PIL  
File: `2.5.1_Spatial_Filtering-PIL.ipynb`

## Cách hoạt động
- Quy trình hoạt động:

1. **Đọc ảnh**
   - Ảnh được đọc bằng thư viện PIL.
   - Chuyển ảnh sang dạng mảng NumPy để xử lý.
   - Code chính:
  ```python
image = Image.open("lenna.png")
```

2. **Áp dụng bộ lọc **
   Các bộ lọc được áp dụng trực tiếp trên pixel của ảnh thông qua kernel :
   - Linear Filtering (Lọc tuyến tính) : sử dụng **kernel convolution** để biến đổi giá trị pixel của ảnh dựa trên các pixel lân cận.

     + Sharpen Filter (làm sắc nét ảnh) : giúp làm nổi bật các chi tiết trong ảnh bằng cách tăng cường sự khác biệt giữa các pixel.
```python
# Common Kernel for image sharpening
kernel = np.array([[-1,-1,-1], 
                   [-1, 9,-1],
                   [-1,-1,-1]])
kernel = ImageFilter.Kernel((3,3), kernel.flatten())
# Applys the sharpening filter using kernel on the original image without noise
sharpened = image.filter(kernel)
# Plots the sharpened image and the original image without noise
plot_image(sharpened , image, title_1="Sharpened image",title_2="Image")
```
  + KQ:
<img width="1477" height="748" alt="image" src="https://github.com/user-attachments/assets/13151f9c-6de7-4e76-8c40-a73ee68eb445" />

```python
sharpened = image.filter(ImageFilter.SHARPEN)
```
  + KQ:
<img width="1429" height="734" alt="image" src="https://github.com/user-attachments/assets/c7e5b048-963a-4627-a82f-b0e9b0a3ea95" />

  + Gaussian Blur (Làm mờ Gaussian) : làm mịn ảnh bằng cách sử dụng Gaussian kernel, giúp giảm nhiễu nhưng vẫn giữ được cấu trúc ảnh
```python
image_filtered = noisy_image.filter(ImageFilter.GaussianBlur)
```
  + KQ:
<img width="1489" height="727" alt="image" src="https://github.com/user-attachments/assets/ba624786-20bf-4217-8385-7fdfd3191e9d" />

  + Filtering Noise (Lọc nhiễu) : 
```python
kernel = np.ones((5,5))/36
# Create a ImageFilter Kernel by providing the kernel size and a flattened kernel
kernel_filter = ImageFilter.Kernel((5,5), kernel.flatten())
image_filtered = noisy_image.filter(kernel_filter)
```
  + KQ:
<img width="1439" height="736" alt="image" src="https://github.com/user-attachments/assets/8a2b0af8-0d15-4589-b855-4e03fea70f0b" />

  - Edge Detection Filter (phát hiện biên) : làm nổi bật các đường biên (edges) trong ảnh bằng cách phát hiện sự thay đổi mạnh về cường độ sáng giữa các pixel lân cận.
```python
img_gray = img_gray.filter(ImageFilter.EDGE_ENHANCE)
```
  + KQ :
<img width="1268" height="1166" alt="image" src="https://github.com/user-attachments/assets/dfef90cf-f032-47e9-9a8f-e4143072b7b8" />


- Median : Loại bỏ nhiễu hiệu quả, đặc biệt là nhiễu dạng salt-and-pepper.
```python
image = image.filter(ImageFilter.MedianFilter)
```
   + KQ :
<img width="1196" height="1162" alt="image" src="https://github.com/user-attachments/assets/efd05baa-0fb2-4062-b6b3-d32a660ba07b" />

## Kết luận
- Linear Filtering: Làm mịn ảnh bằng cách lấy trung bình các pixel lân cận.
- Sharpening: Tăng độ sắc nét và làm nổi bật chi tiết ảnh.
- Gaussian Blur: Làm mịn ảnh và giảm nhiễu một cách tự nhiên.
- Median Filter: Loại bỏ nhiễu hiệu quả, đặc biệt là nhiễu dạng salt-and-pepper.
=> Các kết quả trên cho thấy các bộ lọc không gian có thể cải thiện chất lượng ảnh và làm nổi bật đặc trưng quan trọng.
---

# PHẦN 2 – Spatial Filtering với OpenCV  
File: `2.5.2_Spatial_Filtering.ipynb`

Quy trình xử lý:
1. **Đọc ảnh**
   - Ảnh được đọc bằng `cv2.imread()`.
   - Chuyển đổi sang RGB để hiển thị đúng màu bằng Matplotlib.
2. **Áp dụng các bộ lọc**
   Các bộ lọc phổ biến được sử dụng gồm:
- Linear Filtering:
  + Linear Filtering là phương pháp lọc ảnh sử dụng phép tích chập (convolution) giữa ảnh và một ma trận kernel. 
  + Giá trị pixel mới được tính từ tổng có trọng số của các pixel lân cận. Phương pháp này thường được dùng để làm mịn ảnh, giảm nhiễu hoặc tăng cường chi tiết.

  + Filtering Noise
    + Filtering Noise là kỹ thuật giảm nhiễu trong ảnh bằng các bộ lọc không gian. 
    + Mục tiêu là loại bỏ các pixel nhiễu nhưng vẫn giữ được thông tin quan trọng của ảnh.
  ```python
  kernel = np.ones((6,6))/36
  image_filtered = cv2.filter2D(src=noisy_image, ddepth=-1, kernel=kernel)
  ```
    + KQ :
  <img width="1215" height="633" alt="image" src="https://github.com/user-attachments/assets/a68f03bf-82d2-4153-8ae0-0eae17b4edd7" />

  + Gaussian Blur
    + Gaussian Blur là bộ lọc làm mờ ảnh dựa trên phân bố Gaussian. 
    + Bộ lọc này giúp làm mịn ảnh và giảm nhiễu bằng cách tính trung bình có trọng số của các pixel lân cận, trong đó các pixel gần trung tâm có trọng số lớn hơn.
```python
image_filtered = cv2.GaussianBlur(noisy_image,(5,5),sigmaX=4,sigmaY=4)
```
  + KQ :
<img width="1226" height="615" alt="image" src="https://github.com/user-attachments/assets/b4137a51-e1d5-41ff-b8e1-0c2b166e18b3" />

  + Image Sharpening
    + Image Sharpening là kỹ thuật làm nổi bật chi tiết và đường biên trong ảnh. 
    + Phương pháp này thường sử dụng các kernel thông cao (high-pass filter) để tăng cường sự khác biệt giữa các pixel lân cận.
```PYTHON
# Common Kernel for image sharpening
kernel = np.array([[-1,-1,-1], 
                   [-1, 9,-1],
                   [-1,-1,-1]])
# Applys the sharpening filter using kernel on the original image without noise
sharpened = cv2.filter2D(image, -1, kernel)
```
  + KQ :
<img width="1217" height="602" alt="image" src="https://github.com/user-attachments/assets/d80ab171-0c5e-41ab-bea1-24bdbc870a26" />

---

- Edge Detection
+ Edge Detection là kỹ thuật phát hiện các đường biên trong ảnh. 
+ Các bộ lọc phát hiện biên xác định những vùng có sự thay đổi lớn về cường độ sáng giữa các pixel lân cận, từ đó làm nổi bật các cạnh của đối tượng trong ảnh.
```PYTHON
ddepth = cv2.CV_16S
# Applys the filter on the image in the X direction
grad_x = cv2.Sobel(src=img_gray, ddepth=ddepth, dx=1, dy=0, ksize=3)
```
+ KQ:
<img width="716" height="611" alt="image" src="https://github.com/user-attachments/assets/6c31f1bf-b504-45e1-b91f-cb2a478289e1" />

```PYTHON
grad_y = cv2.Sobel(src=img_gray, ddepth=ddepth, dx=0, dy=1, ksize=3)
```
+ KQ:
<img width="688" height="626" alt="image" src="https://github.com/user-attachments/assets/75c45ad1-bc4e-4618-94aa-6f2a25b02160" />

---

- Median Filter
+ Median Filter là bộ lọc phi tuyến được sử dụng để giảm nhiễu trong ảnh, đặc biệt hiệu quả với nhiễu muối tiêu (salt-and-pepper noise). 
+ Bộ lọc hoạt động bằng cách thay thế pixel trung tâm bằng giá trị trung vị của các pixel trong vùng lân cận.
```PYTHON
filtered_image = cv2.medianBlur(image, 5)
```
+ KQ:
<img width="1390" height="1166" alt="image" src="https://github.com/user-attachments/assets/461823a6-6a9f-4e06-8be7-a7c04896659f" />

---

- Threshold
+ Threshold là kỹ thuật phân ngưỡng được sử dụng để chuyển ảnh xám thành ảnh nhị phân. 
+ Phương pháp này so sánh giá trị pixel với một ngưỡng xác định: nếu giá trị lớn hơn ngưỡng thì pixel được gán thành trắng, ngược lại sẽ được gán thành đen.
```PYTHON
ret, outs = cv2.threshold(src = image, thresh = 0, maxval = 255, type = cv2.THRESH_OTSU+cv2.THRESH_BINARY_INV)
```
+ KQ:
<img width="1210" height="1149" alt="image" src="https://github.com/user-attachments/assets/d53354e2-c89d-462a-aa23-5ed7b9b545d1" />

3. **Tích chập kernel**
   Kernel được áp dụng bằng các hàm của OpenCV như:

   - `cv2.filter2D()`
   - `cv2.GaussianBlur()`
   - `cv2.medianBlur()`

### Kết quả
Các kết quả thu được:
- Linear Filtering: Làm mịn ảnh bằng cách tính trung bình các pixel lân cận.
- Gaussian Blur: Làm mịn ảnh và giảm nhiễu bằng bộ lọc Gaussian.
- Sharpening: Tăng độ sắc nét và làm nổi bật chi tiết của ảnh.
- Median Filter: Loại bỏ nhiễu trong ảnh bằng cách sử dụng giá trị trung vị của các pixel lân cận.
- Thresholding: Chuyển ảnh xám thành ảnh nhị phân bằng cách so sánh giá trị pixel với một ngưỡng xác định.
=> Qua đó có thể thấy mỗi loại filter phù hợp với từng mục đích xử lý ảnh khác nhau.

---

# PHẦN 3 – Feature Detection và Image Matching  
File: `2374802010582_TranNhuKhaY_CV0101_Lab3.ipynb`

## Cách hoạt động
1. **Đọc ảnh**
   - Ảnh được đọc bằng OpenCV.
   - Chuyển đổi sang grayscale để phục vụ phát hiện đặc trưng.

2. **Phát hiện đặc trưng**
   Sử dụng các thuật toán như:
- Fast Fourier Transform (FFT) : sử dụng để chuyển ảnh từ miền không gian (Spatial Domain) sang miền tần số (Frequency Domain). Giúp phát hiện ảnh mờ và phan tích cấu trúc tần số của ảnh.
```python
img_float32 = np.float32(img)
dft = cv2.dft(img_float32, flags = cv2.DFT_COMPLEX_OUTPUT)
dft_shift = np.fft.fftshift(dft)
magnitude_spectrum = 20*np.log(cv2.magnitude(dft_shift[:,:,0],dft_shift[:,:,1]))
```
- KQ :
<img width="957" height="388" alt="image" src="https://github.com/user-attachments/assets/1267a5b2-090e-4874-9808-0a92e391be4b" />
---

- Fast Fourier Transform (FFT) Magnitude algorithm để phát hiện ảnh mờ
```python
filtered_image = difference_of_gaussians(image, 1, 12)
filtered_wimage = filtered_image * window('hann', image.shape)
im_f_mag = fftshift(np.abs(fftn(wimage)))
fim_f_mag = fftshift(np.abs(fftn(filtered_wimage)))
```
- KQ :
<img width="1025" height="980" alt="image" src="https://github.com/user-attachments/assets/37cab2ab-0f46-412f-ac29-b7691b39c7d0" />
---

- Harris Corner Detection : dùng để phát hiện các điểm góc (corners) trong ảnh.
```PYTHON
gray = np.float32(gray)
dst = cv2.cornerHarris(gray,2,3,0.04)
dst = cv2.dilate(dst,None)
img[dst>0.01*dst.max()] = [0,0,225]
```
- KQ:
<img width="668" height="609" alt="image" src="https://github.com/user-attachments/assets/ed82305e-5db8-4b81-ac80-b94c062d5b1b" />

### Kết quả
Kết quả cho thấy:
- Các thuật toán feature detection có thể xác định các điểm đặc trưng quan trọng trong ảnh.
- Image matching giúp tìm các vùng tương ứng giữa hai ảnh khác nhau.
- Phương pháp này thường được sử dụng trong:
  - Object recognition
  - Image stitching
  - Visual localization
  - SLAM trong robot và xe tự hành.

---

### Kết luận
Qua ba notebook giúp hiểu hơn về :
- Các kỹ thuật **Spatial Filtering**
- Các phương pháp **làm mịn và làm sắc nét ảnh**
---

###  Tài liệu tham khảo 
- Tài liệu thực hành – ĐH Văn Lang  
- Pillow Documentation  
- OpenCV Documentation
- Gonzalez & Woods – Digital Image Processing, 2017

