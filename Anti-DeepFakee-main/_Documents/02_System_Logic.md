# LOGIC XỬ LÝ HỆ THỐNG (SYSTEM LOGIC)

## 1. Luồng dữ liệu End-to-End (Data Pipeline)
1. **[Frontend]** Người dùng chọn file (Ảnh/Video) trên `index.html` -> Nhấn nút Upload.
2. **[Backend API]** Nhận multipart/form-data tại endpoint `POST /predict` (`main.py`).
3. **[Tiền xử lý]** Chuyển byte array sang OpenCV Image (BGR) -> Gọi `RetinaFace.detect_faces()`.
4. **[Bắt lỗi]** Nếu Confidence của RetinaFace < 0.9 hoặc không có mặt -> Trả về JSON báo lỗi.
5. **[Cắt khuôn mặt]** Tính toán Bounding Box (Scale = 1.3) -> Crop mặt -> Chuyển sang RGB (PIL).
6. **[Transform]** Resize ảnh mặt về `299x299`, Normalize chuẩn PyTorch -> Chuyển thành Tensor `(1, 3, 299, 299)` đưa lên GPU/CPU.
7. **[Model Inference]** Mạng `Xception` xử lý Tensor -> Tính Softmax ra xác suất của 3 class.
8. **[Grad-CAM]** Lấy gradient từ layer `model.model.conv4` -> Tính toán và phủ màu Heatmap lên ảnh mặt (OpenCV COLORMAP_JET).
9. **[Đóng gói]** Encode Heatmap thành chuỗi Base64 -> Trả về JSON cho Frontend.
10. **[Frontend]** Bóc tách JSON, update DOM hiển thị kết quả và hình ảnh.

## 2. Đặc tả API (API Specification)
- **Endpoint:** `http://127.0.0.1:8000/predict`
- **Method:** `POST`
- **Body:** `multipart/form-data` (key: "file")
- **Thành công (200 OK) Response:**
  ```json
  {
    "type": "image",
    "label": "Face Swap",
    "confidence": 0.9854,
    "heatmap": "iVBORw0KGgoAAAANSUhEUgAA...[base64_string]...",
    "frames_used": 1
  }