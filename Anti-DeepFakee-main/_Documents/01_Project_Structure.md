# CẤU TRÚC VÀ PHÂN CÔNG DỰ ÁN (PROJECT STRUCTURE)

## 1. Tổng quan hệ thống
Hệ thống AI nhận diện Deepfake đa phân lớp (Multiclass Deepfake Detection).
- **Mục tiêu:** Phân loại ảnh/video đầu vào thuộc 1 trong 3 nhãn:
  - `0`: Real (Ảnh thật)
  - `1`: Face Swap (Ghép mặt)
  - `2`: Reenactment (Mô phỏng biểu cảm)
- **Công nghệ lõi:** Python, PyTorch (Mạng Xception), OpenCV, RetinaFace (Dò khuôn mặt), Grad-CAM (Trực quan hóa).
- **Web/API:** FastAPI (Backend), HTML/CSS/JS thuần (Frontend).

## 2. Cấu trúc mã nguồn chi tiết
```text
project_root/
├── dataset/
│   ├── mydataset.py        # Class MyDataset (kế thừa torch.utils.data.Dataset)
│   ├── transform.py        # Tiền xử lý (Resize 299x299, Normalize, Augmentation)
├── network/
│   ├── proposed_model.py   # Class DeepfakeMultiClassModel (Wrapper của Xception)
│   ├── xception.py         # Kiến trúc mạng Xception cơ sở
├── weights/
│   └── 19_multiclass_colab.pkl # Trọng số mô hình đã huấn luyện (Model Weights)
├── train_CNN_multiclass.py # Script vòng lặp huấn luyện, tính Loss/Accuracy
├── detect_from_video_multiclass.py # Logic cắt ảnh, vẽ bounding box và dự đoán
├── gradcam.py              # Thuật toán trích xuất bản đồ nhiệt (Heatmap)
├── main.py                 # (MỚI) Web Server FastAPI bọc logic AI thành API
├── index.html              # Giao diện người dùng tải file và xem kết quả
├── .gitignore              # Bỏ qua file rác (__pycache__/, *.pkl, dataset_raw/)
└── requirements.txt        # Danh sách thư viện Python