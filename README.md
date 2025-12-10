 Desert Off-Road Scene Segmentation  
 Using DINOv2 + Custom ConvNeXt-Style Segmentation Head

This project performs semantic segmentation on desert/off-road environments using a DINOv2 transformer backbone combined with a ConvNeXt-style segmentation head.  
It predicts 10 semantic classes such as trees, rocks, bushes, sand, sky, and ground clutter.

Features
- High-accuracy segmentation using the powerful DINOv2 ViT-S/14 backbone.
- Custom ConvNeXt-inspired lightweight segmentation head.
- Supports 10 desert/off-road terrain classes.
- Detailed evaluation metrics:
  - IoU
  - Dice Score
  - Pixel Accuracy
  - Train/Val Loss curves
- Fully reproducible Kaggle-ready training script.
- Automatic saving of:
  - Training curves
  - Metrics report
  - Model checkpoint (`segmentation_head.pth`)

Dataset Structure
The project uses the Offroad Segmentation Training Dataset provided on Kaggle.

Offroad_Segmentation_Training_Dataset/
│
├── train/
│   ├── Color_Images/
│   ├── Segmentation/
│
└── val/
    ├── Color_Images/
    ├── Segmentation/

Inside Kaggle, the dataset is mounted as
/kaggle/input/duality-desert-segmentation/duality-desert-segmentation/

Classes
Raw mask values are mapped to class IDs:
| Raw Value | Class |
|----------:|-------|
| 0 | Background |
| 100 | Trees |
| 200 | Lush Bushes |
| 300 | Dry Grass |
| 500 | Dry Bushes |
| 550 | Ground Clutter |
| 700 | Logs |
| 800 | Rocks |
| 7100 | Landscape |
| 10000 | Sky |
Total classes: 10

Model Architecture
 1. Backbone  
The model uses Facebook’s DINOv2 ViT-S/14 transformer for feature extraction.
 1. Segmentation Head  
A custom ConvNeXt-style head:
- Depthwise convs  
- GELU activations  
- 1×1 conv projection  
- Final classifier → 10 segmentation classes  

Final Output  
Upsampling using Bilinear Interpolation to match input resolution.

Training
Training consists of:
- 10 epochs
- Batch size: 2
- Optimizer: SGD (momentum 0.9)
- LR = 1e-4
- Image size reduced to h × w multiple of 14 (ViT patch size)

During each epoch:
1. Train loop  
2. Validation loop  
3. IoU, Dice, Pixel Accuracy calculated  
4. Curves saved  

Output Files
After training, the following are saved:
/kaggle/working/
│
├── segmentation_head.pth
└── train_stats/
     ├── training_curves.png
     ├── iou_curves.png
     ├── dice_curves.png
     ├── all_metrics_curves.png
     └── evaluation_metrics.txt

How to Run (Kaggle)
 1. Copy & Patch the training script  
Place the corrected file in
/kaggle/working/train_segmentation.py

 2. Run training:
!python train_segmentation.py

Evaluation Metric
During training, the script computes:
- Mean IoU
- Mean Dice Score
- Pixel Accuracy
- Train & Val Los
These are logged and plotted across epochs.

Future Improvements
- Replace ConvNeXt head with U-Net style decoder  
- Use DeepLabV3+ or SegFormer head  
- Add mixed-precision training (AMP) for 2× faster training  
- Export model to ONNX for real-time deployment  
- Add test-time augmentation  


Author:
Yash Gupta  
Backend + AI Developer  
2025  
--

If you use this project  
Feel free to star the repo or share your results!

