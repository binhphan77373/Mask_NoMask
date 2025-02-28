# Mask and No-mask Detection using YOLOv5s  

## 1. Overview  
In this project, we will train a model to detect faces with and without masks using YOLOv5s. YOLOv5s is one of the lightest and fastest versions of YOLO, making it ideal for deployment on resource-constrained devices.  

The dataset consists of two classes:  
- **Class 0**: Faces with masks  
- **Class 1**: Faces without masks  

## 2. Environment Setup  

### 2.1. Create a Virtual Environment in Anaconda  
If you are using Anaconda, you can create a new environment as follows:  
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

## 3. Prepare the Dataset  
The dataset should be organized in the YOLO format:  
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

Then, create a `mask_nomask.yaml` file with the following content:  
```yaml
train: /path/to/MaskNoMaskDataset/train/images
val: /path/to/MaskNoMaskDataset/valid/images

test: /path/to/MaskNoMaskDataset/test/images

nc: 2
names: ['Mask', 'No-Mask']
```  

## 4. Train the Model  
Run the following command to start training:  
```bash
python train.py --img 640 --batch 16 --epochs 50 --data mask_nomask.yaml --weights yolov5s.pt --device 0
```  

- `--img 640`: Input image size  
- `--batch 16`: Batch size  
- `--epochs 50`: Number of training epochs  
- `--data mask_nomask.yaml`: Path to the dataset configuration file  
- `--weights yolov5s.pt`: Initialize the model with YOLOv5s weights  
- `--device 0`: Use GPU if available  

## 5. Test the Model  
After training, test the model using:  
```bash
python detect.py --weights runs/train/exp/weights/best.pt --img 640 --conf 0.5 --source MaskNoMaskDataset/test/images/
```  

## 6. Export the Model  
If you want to use the model in another environment, export it to ONNX or TensorRT format:  
```bash
python export.py --weights runs/train/exp/weights/best.pt --img 640 --batch 1 --device 0 --include onnx
```  

## 7. Conclusion  
This project provides a step-by-step guide to training a YOLOv5s model for mask and no-mask detection. You can improve the model's accuracy by:  
- Using a larger dataset  
- Fine-tuning training parameters  
- Experimenting with other YOLO versions like YOLOv5m, YOLOv5l, or YOLOv5x.  
