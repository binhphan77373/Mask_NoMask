# Mask and No-mask Detection using YOLOv5s

## 1. Tổng quan
Trong dự án này, chúng ta sẽ huấn luyện một mô hình nhận diện khuôn mặt có và không có khẩu trang bằng YOLOv5s. YOLOv5s là một trong những phiên bản nhẹ và nhanh nhất của YOLO, giúp triển khai mô hình trên các thiết bị có tài nguyên hạn chế.

Bộ dữ liệu sử dụng gồm hai lớp:
- **Class 0**: Khuôn mặt có khẩu trang
- **Class 1**: Khuôn mặt không có khẩu trang

## 2. Cài đặt môi trường

### 2.1. Tạo môi trường trong Anaconda
Nếu sử dụng Anaconda, bạn có thể tạo một môi trường mới như sau:
```bash
conda create -n yolov5_env python=3.8 -y
conda activate yolov5_env
```

### 2.2. Clone YOLOv5
```bash
git clone https://github.com/ultralytics/yolov5.git
cd yolov5
pip install -r requirements.txt
```

## 3. Chuẩn bị dữ liệu
Dữ liệu cần được tổ chức theo định dạng YOLO:
```
MaskNoMaskDataset/
├── data.yaml
├── test
│   ├── images
│   └── labels
├── train
│   ├── images
│   └── labels
└── valid
    ├── images
    └── labels
```

Sau đó, tạo file `mask_nomask.yaml` với nội dung:
```yaml
train: /path/to/MaskNoMaskDataset/train/images
val: /path/to/MaskNoMaskDataset/valid/images

test: /path/to/MaskNoMaskDataset/test/images

nc: 2
names: ['Mask', 'No-Mask']
```

## 4. Huấn luyện mô hình
Chạy lệnh sau để bắt đầu quá trình huấn luyện:
```bash
python train.py --img 640 --batch 16 --epochs 50 --data mask_nomask.yaml --weights yolov5s.pt --device 0
```

- `--img 640`: Kích thước ảnh đầu vào
- `--batch 16`: Kích thước batch
- `--epochs 50`: Số epoch huấn luyện
- `--data mask_nomask.yaml`: Đường dẫn đến file cấu hình dữ liệu
- `--weights yolov5s.pt`: Khởi tạo mô hình với trọng số của YOLOv5s
- `--device 0`: Sử dụng GPU nếu có

## 5. Kiểm tra mô hình
Sau khi huấn luyện, kiểm tra mô hình với:
```bash
python detect.py --weights runs/train/exp/weights/best.pt --img 640 --conf 0.5 --source MaskNoMaskDataset/test/images/
```

## 6. Xuất mô hình
Nếu muốn sử dụng mô hình trong môi trường khác, xuất nó sang định dạng ONNX hoặc TensorRT:
```bash
python export.py --weights runs/train/exp/weights/best.pt --img 640 --batch 1 --device 0 --include onnx
```

## 7. Kết luận
Dự án này hướng dẫn cách huấn luyện một mô hình YOLOv5s để nhận diện khuôn mặt có và không có khẩu trang. Bạn có thể cải thiện độ chính xác bằng cách:
- Sử dụng nhiều dữ liệu hơn
- Tinh chỉnh các tham số huấn luyện
- Thử nghiệm với các phiên bản khác của YOLO như YOLOv5m, YOLOv5l hoặc YOLOv5x.

