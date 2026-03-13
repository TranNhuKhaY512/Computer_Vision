# Thực hành môn Computer Vision
## KEYPOINT DETECTION
### Sinh viên thực hiện
**Trần Như Khả Ý -- 2374802010582**
### GVHD: **Đỗ Hữu Quân**

# Giới thiệu
Notebook này thực hiện nhiều kỹ thuật quan trọng trong **ComputerVision**, bao gồm:
- Feature Detection – Phát hiện đặc trưng trong ảnh  
- Corner Detection – Phát hiện điểm góc  
- Blob Detection – Phát hiện vùng blob trong ảnh  
- SIFT / ORB Feature Extraction – Trích xuất đặc trưng ảnh  
- Image Matching – So khớp đặc trưng giữa các ảnh  
- Image Stitching – Ghép ảnh panorama  
- Stereo Vision – Ước lượng độ sâu ảnh

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
    
# Cách hoạt động 
## 1. Đọc và hiển thị ảnh
Ảnh được đọc bằng OpenCV và hiển thị bằng Matplotlib.

``` python
img = cv2.imread('hinh1.jpg')
plt.imshow(img[:,:,::-1])
```
Do OpenCV đọc ảnh theo định dạng **BGR**, cần chuyển sang **RGB** khi
hiển thị.

<img width="1292" height="951" alt="Screenshot 2026-03-13 203614" src="https://github.com/user-attachments/assets/c2ef7840-8db4-4549-8fc8-5fd841213e4a" />


## 2. Harris Corner Detection
Thuật toán **Harris Corner** được sử dụng để phát hiện các điểm góc
trong ảnh.
``` python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
gray = np.float32(gray)

dst = cv2.cornerHarris(gray,2,3,0.04)
```

Ứng dụng:
-   tracking
-   object detection
-   image matching
<img width="987" height="896" alt="image" src="https://github.com/user-attachments/assets/d7a47941-afa9-4387-96a9-1e8225437b16" />


## 3. Difference of Gaussians (DoG)
DoG được sử dụng để phát hiện các vùng có thay đổi mạnh về cấu trúc
trong ảnh.
``` python
blur1 = cv2.GaussianBlur(gray,(5,5),1)
blur2 = cv2.GaussianBlur(gray,(9,9),2)

dog = blur1 - blur2
```

Ứng dụng:
-   blob detection
-   feature detection
-   SIFT algorithm
<img width="1020" height="869" alt="image" src="https://github.com/user-attachments/assets/d4cf5311-a5aa-4470-8fdf-9a253eab3056" />


## 4. Laplacian Edge Detection
Bộ lọc **Laplacian** được sử dụng để phát hiện biên trong ảnh.
``` python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
log = cv2.Laplacian(gray, cv2.CV_64F)
```
<img width="1040" height="862" alt="image" src="https://github.com/user-attachments/assets/ee112b97-ce54-450e-b024-f9f7d3d4995a" />

## 5. SIFT Feature Detection
Thuật toán **SIFT (Scale-Invariant Feature Transform)** được sử dụng để
phát hiện và mô tả các điểm đặc trưng.
``` python
sift = cv2.SIFT_create()
kp, des = sift.detectAndCompute(gray,None)
```

Ưu điểm:
-   bất biến theo **scale**
-   bất biến theo **rotation**
-   ổn định với thay đổi **ánh sáng**

Ứng dụng:
-   image matching
-   object recognition
-   panorama stitching
<img width="963" height="903" alt="image" src="https://github.com/user-attachments/assets/4bb55661-b559-41ec-82f4-71135d4a5f38" />

## 6. Blob Detection
Blob là các vùng ảnh có cường độ khác biệt so với vùng xung quanh.
``` python
params = cv2.SimpleBlobDetector_Params()
params.filterByArea = True
params.minArea = 100

detector = cv2.SimpleBlobDetector_create(params)
```
Ứng dụng:
-   medical image analysis
-   object detection
-   tracking
<img width="979" height="854" alt="image" src="https://github.com/user-attachments/assets/33a7bfe9-8d10-4043-9c8f-82759398a33f" />

## 7. Clustering Features bằng K-Means
Đặc trưng SIFT được phân cụm bằng **K-Means**.

``` python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=50)
kmeans.fit(des)
```

Ứng dụng:
-   Bag of Visual Words
-   Image classification
-   Visual search
<img width="1180" height="97" alt="image" src="https://github.com/user-attachments/assets/dcc637a9-0757-448e-97f5-e8071d251413" />

## 8. Image Stitching
OpenCV hỗ trợ ghép nhiều ảnh thành **panorama**.
``` python
stitcher = cv2.Stitcher_create()
status, pano = stitcher.stitch([img1, img2])
```
Ứng dụng:
-   panorama photography
-   drone mapping
-   satellite imagery
<img width="1064" height="693" alt="image" src="https://github.com/user-attachments/assets/a4fee6f3-cc6c-4abf-94f0-dcf06e4885fd" />

## 9. ORB Feature Matching
ORB là thuật toán phát hiện đặc trưng **nhanh và nhẹ hơn SIFT**.

``` python
orb = cv2.ORB_create()

kp1, des1 = orb.detectAndCompute(img1,None)
kp2, des2 = orb.detectAndCompute(img2,None)
```
Sau đó dùng **Brute Force Matcher** để so khớp đặc trưng.
<img width="1080" height="629" alt="image" src="https://github.com/user-attachments/assets/e54996cf-874f-45a3-ae29-fd1321f71fad" />


## 10. Stereo Vision (Depth Estimation)
Stereo Vision dùng hai ảnh để ước lượng **độ sâu (depth)**.
``` python
stereo = cv2.StereoBM_create(numDisparities=16, blockSize=15)

disparity = stereo.compute(left,right)
```

Ứng dụng:
-   robot navigation
-   autonomous vehicles
-   3D reconstruction

<img width="731" height="816" alt="image" src="https://github.com/user-attachments/assets/83cf90a8-118d-4246-861e-820b9e41edbc" />

## 11. Histogram Comparison
Histogram giúp so sánh phân bố màu giữa hai ảnh.

``` python
hist1 = cv2.calcHist([img1],[0,1,2],None,[8,8,8],[0,256,0,256,0,256])
hist2 = cv2.calcHist([img2],[0,1,2],None,[8,8,8],[0,256,0,256,0,256])
```
Ứng dụng:
-   image similarity
-   image retrieval
-   dataset analysis

<img width="814" height="106" alt="image" src="https://github.com/user-attachments/assets/3cb2a758-c711-454d-8d7e-27177bb1912a" />

<img width="1118" height="1089" alt="image" src="https://github.com/user-attachments/assets/937eef2e-13ba-4213-b23a-d070c80f9d79" />

# Kết luận
Notebook giúp thực hành nhiều kỹ thuật quan trọng trong **Computer
Vision**:
-   Harris Corner Detection\
-   Difference of Gaussians\
-   Laplacian Edge Detection\
-   SIFT Feature Detection\
-   Blob Detection\
-   Feature Clustering\
-   Image Stitching\
-   ORB Matching\
-   Stereo Vision\
-   Histogram Comparison

---

# Tài liệu tham khảo

-   OpenCV Documentation\
-   Scikit-learn Documentation\
-   Digital Image Processing -- Gonzalez & Woods\
-   Computer Vision: Algorithms and Applications -- Richard Szeliski
