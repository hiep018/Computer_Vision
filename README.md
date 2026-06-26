# Deepfake-Detection

Đây là phiên bản triển khai bằng **PyTorch** của mô hình **phát hiện Deepfake**, được xây dựng dựa trên dự án **FaceForensics++**.

Mạng Backbone được sử dụng là **XceptionNet**. Ngoài ra, nhóm tác giả cũng đã tái triển khai **MesoNet** bằng PyTorch, vì vậy bạn có thể sử dụng mạng **MesoNet** trong dự án này.

---

# Cài đặt & Yêu cầu

Mã nguồn đã được kiểm thử với:

- **PyTorch 1.3.1**
- **Python 3.6**

Để biết đầy đủ các thư viện cần thiết, hãy xem trong tệp `requirements.txt`.

## Cài đặt các thư viện Python

Chạy lệnh:

```bash
python -m pip install -r requirements.txt
```

Mặc dù bạn có thể cài đặt toàn bộ các thư viện cùng một lúc, nhưng thư viện **dlib** thường dễ cài đặt hơn bằng Conda:

```bash
conda install -c conda-forge dlib
```

---

# Bộ dữ liệu (Dataset)

Nếu bạn muốn sử dụng bộ dữ liệu mã nguồn mở **FaceForensics++**, hãy sử dụng script:

```text
./download-FaceForensics_v3.py
```

để tải dữ liệu theo hướng dẫn trong phần **Download** của FaceForensics++.

Bạn có thể huấn luyện mô hình trên toàn bộ ảnh, tuy nhiên tác giả **khuyến nghị chỉ sử dụng vùng khuôn mặt (Face Region)** làm đầu vào vì sẽ cho hiệu quả tốt hơn.

---

# Mô hình huấn luyện sẵn (Pretrained Model)

Các mô hình được cung cấp **chỉ nhằm mục đích kiểm tra tính hiệu quả của mã nguồn**.

Tác giả **khuyến khích bạn tự huấn luyện mô hình trên bộ dữ liệu của mình** để đạt kết quả tốt hơn.

Trong tương lai, nhóm sẽ tiếp tục cập nhật các mô hình có hiệu suất cao hơn.

Các mô hình pretrained được cung cấp (được huấn luyện trên FaceForensics++):

- `FF++_c23.pth`
- `FF++_c40.pth`

---

# Cách sử dụng (Usage)

## 1. Kiểm tra trên video

```bash
python detect_from_video.py \
    --video_path ./videos/003_000.mp4 \
    --model_path ./pretrained_model/df_c0_best.pkl \
    -o ./output \
    --cuda
```

### Giải thích tham số

| Tham số | Ý nghĩa |
|----------|----------|
| `--video_path` | Đường dẫn tới video cần phát hiện Deepfake. |
| `--model_path` | Đường dẫn tới mô hình đã huấn luyện. |
| `-o` | Thư mục lưu kết quả đầu ra. |
| `--cuda` | Sử dụng GPU để tăng tốc quá trình suy luận. |

---

## 2. Kiểm tra trên ảnh

```bash
python test_CNN.py \
    -bz 32 \
    --test_list ./data_list/Deepfakes_c0_299.txt \
    --model_path ./pretrained_model/df_c0_best.pkl
```

### Giải thích tham số

| Tham số | Ý nghĩa |
|----------|----------|
| `-bz 32` | Batch Size = 32 ảnh mỗi lần xử lý. |
| `--test_list` | Danh sách ảnh dùng để kiểm tra. |
| `--model_path` | Đường dẫn tới mô hình đã huấn luyện. |

---

## 3. Huấn luyện mô hình

```bash
python train_CNN.py
```

> **Lưu ý:** Trước khi huấn luyện, bạn nên đọc mã nguồn và thiết lập các tham số phù hợp với bộ dữ liệu của mình.

---

# Quy trình sử dụng dự án

```text
Bộ dữ liệu FaceForensics++
            │
            ▼
Tiền xử lý dữ liệu
(Tách khuôn mặt khỏi ảnh/video)
            │
            ▼
Huấn luyện mô hình
(train_CNN.py)
            │
            ▼
Sinh mô hình (.pth hoặc .pkl)
            │
            ▼
Kiểm tra
├── test_CNN.py (Ảnh)
└── detect_from_video.py (Video)
            │
            ▼
Xuất kết quả dự đoán
```

---

# Tóm tắt dự án

Dự án này là một hệ thống **phát hiện Deepfake (Deepfake Detection)** sử dụng **PyTorch**.

Các đặc điểm chính:

- Sử dụng **XceptionNet** làm mạng Backbone chính.
- Hỗ trợ sử dụng **MesoNet**.
- Huấn luyện trên bộ dữ liệu **FaceForensics++**.
- Hỗ trợ phát hiện Deepfake trên:
  - Ảnh
  - Video
- Hỗ trợ:
  - Huấn luyện mô hình mới.
  - Kiểm thử bằng mô hình đã huấn luyện.
- Khuyến nghị sử dụng **vùng khuôn mặt** thay vì toàn bộ ảnh để tăng độ chính xác.

---

# Giới thiệu

Nếu dự án này hữu ích đối với bạn, nhóm tác giả rất mong nhận được sự ủng hộ bằng cách:

- ⭐ Star dự án.
- 🍴 Fork dự án.

Nếu có bất kỳ câu hỏi hoặc góp ý nào, bạn có thể liên hệ trực tiếp với nhóm phát triển.

Xin chân thành cảm ơn sự quan tâm và ủng hộ của bạn!

---

# Giấy phép (License)

Phiên bản triển khai này **chỉ được cung cấp cho mục đích nghiên cứu và học thuật**.

Nếu bạn muốn sử dụng công nghệ này cho **mục đích thương mại**, vui lòng liên hệ trực tiếp với nhóm tác giả để được cấp phép.
