### File 3: `03_Project_Workflow.md`
```markdown
# QUY TRÌNH PHÁT TRIỂN (PROJECT WORKFLOW)

## Giai đoạn 1: Xử lý Dữ liệu thô (Data Preparation)
1. Đưa video/ảnh gốc vào thư mục `dataset_raw/`.
2. Chạy script `extract_faces_to_dataset.py` để RetinaFace dò và cắt khuôn mặt.
3. Sinh ra các file `.txt` chứa danh sách đường dẫn ảnh và nhãn tương ứng (Train/Val set).

## Giai đoạn 2: Huấn luyện AI (Model Training)
1. Cấu hình Hyperparameters (Epochs, Batch Size, Learning Rate) trong `train_CNN_multiclass.py`.
2. Chạy lệnh: `python train_CNN_multiclass.py --batch_size 32 --epoches 20`
3. Theo dõi hàm Loss và Accuracy. Lưu lại bộ trọng số tốt nhất (`best_multiclass.pkl`) vào thư mục `weights/`.

## Giai đoạn 3: Xây dựng & Tích hợp API (Backend Integration)
1. Tái sử dụng logic của `detect_from_video_multiclass.py` và thuật toán trong `gradcam.py`.
2. Khởi tạo FastAPI trong `main.py`, load Model và Weights một lần duy nhất lúc bật server (Cold Start).
3. Đảm bảo API trả về Base64 Heatmap và xử lý triệt để lỗi khi ảnh không có khuôn mặt.

## Giai đoạn 4: Triển khai & Kiểm thử (Deploy & Testing)
1. Cài đặt môi trường: `pip install -r requirements.txt`
2. Khởi động API Server: `uvicorn main:app --reload`
3. Mở `index.html` bằng trình duyệt (hoặc Live Server).
4. **Kiểm thử (BVA):** Đẩy các ảnh khó (thiếu sáng, ảnh phong cảnh không có người, mặt quay nghiêng) vào hệ thống để đảm bảo API không bị crash mà chỉ trả về JSON lỗi hợp lệ.
5. Push code lên GitHub (Đảm bảo `.gitignore` đã chặn file weights > 100MB).