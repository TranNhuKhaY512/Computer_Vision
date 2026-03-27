## THỰC HÀNH MÔN COMPUTER VISION
### LAB VIDEO OBJECT DECTECTION
#### SVTH: TRẦN NHƯ KHẢ Ý - 2374802010582
#### GVHD : ĐỖ HỮU QUÂN 

---

## Công cụ sử dụng:
- **Python:** Ngôn ngữ lập trình chính.
- **ImageAI:** Thư viện cấp cao cung cấp các API tiện lợi, giúp đơn giản hóa toàn bộ quá trình nhận diện vật thể phức tạp chỉ với vài dòng code.
- **PyTorch & Torchvision:** Nền tảng học sâu (Deep Learning framework) cốt lõi hoạt động ngầm để tính toán và chạy mô hình mạng nơ-ron.
- **OpenCV (cv2):** OpenCV được ImageAI gọi ngầm bên dưới để đảm nhiệm việc đọc luồng video, trích xuất khung hình và ghi lại video kết quả.

## Cách hoạt động
1. **Khởi tạo và Định vị:** dùng `os.getcwd()` để xác định thư mục gốc đang làm việc, để trỏ đường dẫn tới file model và file video gốc.
2. **Nạp Mô hình vào Bộ nhớ (Load Model):** Khởi tạo `VideoObjectDetection`, khai báo kiến trúc là RetinaNet và nạp file trọng số `.pth`.
3. Sử dụng kiến trúc RetinaNet với bộ trọng số huấn luyện sẵn retinanet_resnet50_fpn_coco-eeacb38b.pth được tham khảo từ tài liệu hướng dẫn trên GitHub của thư viện ImageAI do Olafenwa Moses phát triển.
4. **Giải mã Video (Video Decoding):** Đọc file video đầu vào (`39890-423345734.mp4`) được tải về từ Pixabay và tách video đó ra thành hàng loạt các bức ảnh tĩnh liên tiếp (gọi là các frames).
5. **Nhận diện và Đóng khung (Detection & Bounding Box):** Đưa lần lượt từng frame qua mạng nơ-ron RetinaNet. Thuật toán sẽ quét qua bức ảnh, phát hiện tọa độ các vật thể, vẽ khung chữ nhật (bounding box) bao quanh chúng và dán nhãn (ví dụ: "person 99%", "car 85%"). Tính năng `log_progress=True` sẽ in tiến trình này ra terminal.
6. **Mã hóa và Xuất Video (Video Encoding):** Sau khi xử lý xong, chương trình sẽ ghép các frames đã được vẽ khung nhận diện lại với nhau để tạo thành một video hoàn chỉnh. Kết quả được lưu xuống ổ cứng với tên `video_detection_output.mp4` ở tốc độ 5 khung hình/giây (`frames_per_second=5`).
- Code chính:
```python

from imageai.Detection import VideoObjectDetection
import os
execution_path = os.getcwd()
detector = VideoObjectDetection()
detector.setModelTypeAsRetinaNet()
# Truyền thẳng đường dẫn tuyệt đối
detector.setModelPath( os.path.join(execution_path , "retinanet_resnet50_fpn_coco-eeacb38b.pth"))
detector.loadModel()

video_path = detector.detectObjectsFromVideo(input_file_path=os.path.join(execution_path, "39890-423345734.mp4"),
          output_file_path=os.path.join(execution_path, "video_detection_output")
          , frames_per_second=5, log_progress=True)
print(video_path)
```
- KQ :
<img width="2560" height="1440" alt="Screenshot 2026-03-27 213314" src="https://github.com/user-attachments/assets/bc07873e-79c4-48c3-8a87-e874e7461682" />
<img width="2560" height="1440" alt="Screenshot 2026-03-27 213254" src="https://github.com/user-attachments/assets/73021b12-d6d5-47d1-ba1e-876c50c26827" />
<img width="2560" height="1440" alt="Screenshot 2026-03-27 213224" src="https://github.com/user-attachments/assets/191757d6-1daa-4c18-b526-27824e8a7672" />
<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/8e08f907-9596-4304-989c-048e54d2b6d6" />

---

### Tài liệu tham khảo
- OpenCV Documentation
- Scikit-learn Documentation
- Digital Image Processing -- Gonzalez & Woods
- Computer Vision: Algorithms and Applications -- Richard Szeliski
- Pixabay, "Video bản quyền miễn phí chủ đề Lễ hội.", "https://pixabay.com/vi/videos/search/lễ%20hội/"(Dữ liệu video đầu vào sử dụng trong quá trình thử nghiệm và đánh giá mô hình).
- O. Moses, "Video Object Detection - ImageAI," GitHub: "https://github.com/OlafenwaMoses/ImageAI/blob/master/imageai/Detection/VIDEO.md" (Tham khảo tài liệu hướng dẫn tích hợp và tải trọng số mô hình nhận diện retinanet_resnet50_fpn_coco-eeacb38b.pth).
