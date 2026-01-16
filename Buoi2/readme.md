## Thực hành môn CV - Buổi 2 
## Lab 2: Thao tác ảnh cơ bản với PIL và OpenCV
### Sinh viên thực hiện: Trần Như Khả Ý _ 2374802010582
### GVHD: Đỗ Hữu Quân
---

### 1. Giới thiệu chung
- Bài lab này nhằm mục đích giúp làm quen với các thao tác xử lý ảnh cơ bản trong Python thông qua hai thư viện phổ biến là Pillow (PIL) và OpenCV.
Thông qua bài thực hành, ta hiểu được cách ảnh được lưu trữ dưới dạng mảng, cách sao chép ảnh để tránh lỗi aliasing, cũng như thực hiện các thao tác như lật ảnh, cắt ảnh, chỉnh sửa pixel, vẽ hình và chèn chữ lên ảnh.
- Bài gồm 2 file .ipynb:Thao tác ảnh cơ bản bằng PIL, thao tác ảnh cơ bản bằng OpenCV.
---

### 2. Công nghệ sử dụng:
- Ngôn ngữ: Python 3
- Thư viện
  + NumPy: Dùng để biểu diễn ảnh dưới dạng mảng nhiều chiều và thao tác trực tiếp trên pixel.
  + Matplotlib: Dùng để hiển thị ảnh trong quá trình xử lý.
  + Pillow (PIL): Hỗ trợ các thao tác ảnh cơ bản như lật ảnh, cắt ảnh, vẽ hình và ghi chữ.
  + OpenCV (cv2): Thư viện xử lý ảnh mạnh, tối ưu hiệu năng, thường dùng trong thị giác máy tính.
---

### 3, Cách hoạt động
#### 3.1 Thao tác với ảnh cơ bản với PIL: (2.2.1_basic_image_manipulation_PIL.ipynb)
- Ảnh được đọc bằng Image.open() và chuyển sang NumPy array khi cần thao tác pixel.
- Code chính:
```python
# Đọc ảnh baboon và chuyển sang mảng NumPy
baboon = np.array(Image.open('baboon.png'))
```
--- 

- Khi gán trực tiếp ảnh cho biến khác, hai biến sẽ cùng trỏ đến một vùng nhớ → dễ gây lỗi aliasing.
- Code chính:
```python
A = baboon   # A tham chiếu trực tiếp tới baboon (không copy)
id(A) == id(baboon)   # True → cùng vùng nhớ
```
---

- Sử dụng .copy() để tạo bản sao độc lập của ảnh.
- Code chính:
```python
B = baboon.copy()  # B là bản sao độc lập của baboon
id(B)==id(baboon)    # False → khác vùng nhớ
```
---

- Lật ảnh theo chiều dọc và ngang bằng ImageOps.flip() và ImageOps.mirror().
- Code chính:
```python
for i,row in enumerate(array):   # Lật ảnh theo chiều dọc bằng NumPy
    array_flip[width - 1 - i, :, :] = row
```
```python
# Lật ảnh theo chiều dọc bằng PIL và hiển thị ảnh
im_flip = ImageOps.flip(image)
```
- KQ:
<img width="975" height="577" alt="image" src="https://github.com/user-attachments/assets/872c69ef-c2ad-4ba8-afd4-a82dc2510178" />

```python
# Lật ảnh theo chiều ngang bằng PIL (left ↔ right)
im_mirror = ImageOps.mirror(image)
```
- KQ:
<img width="953" height="561" alt="image" src="https://github.com/user-attachments/assets/d71ca0f3-5680-43b7-8b47-2f429bbd9990" />

```python
# Flip ảnh bằng transpose với mã phép biến đổi = 1 (FLIP_TOP_BOTTOM)
im_flip = image.transpose(1)
```
- KQ:
<img width="989" height="573" alt="image" src="https://github.com/user-attachments/assets/046fe3cf-8bee-4ac4-968c-7ef09c463040" />
---

- Cắt ảnh bằng cách chỉ định tọa độ (left, upper, right, lower).
- Code chính:
  + Cắt ảnh theo trục dọc bằng NumPy (giữ nguyên chiều ngang)
```python
upper = 150
lower = 400
# Cắt ảnh theo trục dọc bằng NumPy (giữ nguyên chiều ngang)
crop_top = array[upper: lower,:,:]
```
- KQ:
<img width="918" height="155" alt="image" src="https://github.com/user-attachments/assets/219df44f-4da0-4abf-b779-43133ac23883" />
---


  + Cắt tiếp theo chiều ngang để lấy vùng chữ nhật:
```python
left = 150
right = 400

# Cắt tiếp theo chiều ngang để lấy vùng chữ nhật
crop_horizontal = crop_top[: ,left:right,:]
```
- KQ:
<img width="917" height="265" alt="image" src="https://github.com/user-attachments/assets/85f6d30c-5ddc-486f-ad81-67fa5a221dc5" />
---

  + Cắt ảnh bằng PIL với tuple (left, upper, right, lower)
```python
# Cắt ảnh bằng PIL với tuple (left, upper, right, lower)
crop_image = image.crop((left, upper, right, lower))
```
- KQ:
<img width="919" height="894" alt="image" src="https://github.com/user-attachments/assets/5babb723-81fc-4e45-a08e-4b6012bdf389" />
---

- Thay đổi một vùng pixel cụ thể bằng cách thao tác trên mảng NumPy.
- Code chính:
```python
# Tạo bản sao của ảnh gốc để tránh làm thay đổi dữ liệu ban đầu
array_sq = np.copy(array)

# Trong vùng crop: đặt kênh Green và Blue = 0 → chỉ còn Red
array_sq[upper:lower, left:right, 1:3] = 0
```
- Vẽ hình chữ nhật và ghi chữ lên ảnh bằng ImageDraw.
- Code chính:
```python
# Định nghĩa vùng hình chữ nhật
shape = [left, upper, right, lower] 
# Vẽ hình chữ nhật màu đỏ
image_fn.rectangle(xy=shape,fill="red")
```
- KQ:
<img width="1018" height="599" alt="image" src="https://github.com/user-attachments/assets/1e9da9dc-3c45-4dcd-9ecf-cb7aa82839db" />

```python
# Vẽ chữ "box" lên ảnh tại góc trên trái
image_fn.text(xy=(0,0),text="box",fill=(0,0,0))
```
- KQ:
<img width="991" height="586" alt="image" src="https://github.com/user-attachments/assets/1e5e19d5-067a-4e79-9d38-e7ecf73869df" />

---

#### 3.2 Thao tác với ảnh cơ bản với open CV (2.2.2_basic_image_manipulation_open_CV.ipynb):
- Ảnh được đọc bằng cv2.imread() (theo thứ tự màu BGR).
- Code chính:
```python
baboon = cv2.imread("baboon.png")
```
---

- Cần chuyển từ BGR sang RGB khi hiển thị bằng Matplotlib.
- Code chính:
```python
plt.imshow(cv2.cvtColor(baboon, cv2.COLOR_BGR2RGB))
```
---

- Khi không dùng .copy(), các biến ảnh có thể trỏ chung một vùng nhớ.
- Code chính:
```python
A = baboon  # A là biến tham chiếu, trỏ tới cùng vùng nhớ với baboon
id(A)==id(baboon)    # True → A và baboon dùng chung bộ nhớ
id(A)        # In ra địa chỉ vùng nhớ của A
```
- Dùng .copy(), địa chỉ vùng nhớ khác nhau
- Code chính:
```python
B = baboon.copy()        # Tạo bản sao độc lập của baboon
id(B) == id(baboon)      # False → B có vùng nhớ khác
```
---

- Lật ảnh bằng cv2.flip() với các flipCode:
  + 0: lật dọc
  + 1: lật ngang
  + -1: lật cả hai chiều
- Code chính:
```python
for flipcode in [0, 1, -1]:
    im_flip = cv2.flip(image, flipcode)  
    # 0: lật dọc | 1: lật ngang | -1: lật cả hai
    plt.imshow(cv2.cvtColor(im_flip, cv2.COLOR_BGR2RGB))
    plt.title("flipcode: " + str(flipcode))
    plt.show()
```
- KQ:
<img width="681" height="1167" alt="image" src="https://github.com/user-attachments/assets/1dbf2bc9-ee7d-4580-9bea-df977f1e54c9" />

---

- Xoay ảnh bằng cv2.rotate() với các hằng số có sẵn (90°, 180°).
- Code chính:
```python
flip = {  
    "ROTATE_90_CLOCKWISE": cv2.ROTATE_90_CLOCKWISE,               # Hằng số xoay ảnh 90° theo chiều kim đồng hồ  
    "ROTATE_90_COUNTERCLOCKWISE": cv2.ROTATE_90_COUNTERCLOCKWISE, # Hằng số xoay ảnh 90° ngược chiều kim đồng hồ  
    "ROTATE_180": cv2.ROTATE_180                                  # Hằng số xoay ảnh 180°  
}

```
- KQ:
- Xoay 90 độ chiều kim đồng hồ
<img width="1043" height="823" alt="image" src="https://github.com/user-attachments/assets/24c9579f-45c3-4177-8ade-d6e85b4aaeb6" />
---

- Xoay 90 độ ngược chiều kim đồng hồ
<img width="992" height="777" alt="image" src="https://github.com/user-attachments/assets/e5b9c671-30ab-4dd1-a389-1bb2964e1f62" />
---

- Xoay 180 độ: 
<img width="1032" height="362" alt="image" src="https://github.com/user-attachments/assets/369c080f-ece8-4c11-aad7-a4b79f006470" />

---

- Cắt ảnh trực tiếp bằng slicing của NumPy.
- Code chính:
```python
upper = 150
lower = 400
crop_top = image[upper: lower,:,:]  # Cắt ảnh theo chiều dọc
```
- KQ:
<img width="1000" height="177" alt="image" src="https://github.com/user-attachments/assets/867cb4f8-e90e-4b48-856b-45e8ae0654f1" />
---
```python
left = 150
right = 400
crop_horizontal = crop_top[: ,left:right,:]  # Cắt tiếp theo chiều ngang
```
- KQ:
<img width="925" height="229" alt="image" src="https://github.com/user-attachments/assets/57485047-a9e6-4f92-8870-130d88d890c6" />
---

- Thay đổi pixel trong một vùng bằng cách gán giá trị mới.
- Code chính:
```python
array_sq = np.copy(image)           # Tạo bản sao ảnh
array_sq[upper:lower, left:right, :] = 0  # Gán vùng crop thành màu đen
```
- KQ:
<img width="1000" height="334" alt="image" src="https://github.com/user-attachments/assets/1c17599c-0013-4088-b95c-b22fe812e635" />

---

- Vẽ hình chữ nhật bằng cv2.rectangle() và ghi chữ bằng cv2.putText().
- Code chính:
```python
# Vẽ hình chữ nhật màu xanh
cv2.rectangle(image_draw, start_point, end_point, (0,255,0), 3)

# Ghi chữ lên ảnh
image_draw=cv2.putText(img=image,text='Stuff',org=(10,500),color=(255,255,255),fontFace=4,fontScale=5,thickness=2)
```
- KQ: vẽ hinh chữ nhật lên ảnh:
<img width="1080" height="582" alt="image" src="https://github.com/user-attachments/assets/0d9cdff2-2c0f-40f8-aba0-e666d69e1a5b" />

- KQ: ghi chữ lên ảnh
<img width="984" height="577" alt="image" src="https://github.com/user-attachments/assets/90e3f232-41d5-47c5-bf93-8d0fc8e82423" />
---

### 4. Kết luận:
Qua bài lab này, ta đã nắm được các thao tác xử lý ảnh cơ bản bằng PIL và OpenCV, đồng thời hiểu rõ hơn về:
- Cách ảnh được biểu diễn trong máy tính
- Vấn đề aliasing khi làm việc với mảng ảnh
- Cách chỉnh sửa ảnh ở mức pixel
---

### 5. Tài liệu tham khảo 
- Tài liệu thực hành – ĐH Văn Lang  
- Pillow Documentation  
- OpenCV Documentation
- Gonzalez & Woods – Digital Image Processing, 2017







