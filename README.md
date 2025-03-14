# Deep Learning Project 1 (Spring 2025)
Custom ResNet on CIFAR-10

Authors: Rujuta Amit Joshi, Shashank Dugad, Anshi Shah,
Affiliations: New York University, 
Emails: rj2719@nyu.edu, sd5957@nyu.edu, ans10020@nyu.edu

This repository contains **Deep Learning Project 1**, focusing on a **custom ResNet** architecture (under 5M parameters) to classify **CIFAR-10** images. We augment the standard training set using AutoAugment, random cropping, and horizontal flipping, and then validate on either a reserved split of CIFAR-10 or the official test set. Finally, we perform inference on a **hidden (unlabeled) test set** and generate a Kaggle-style submission CSV.

## Key Features
1. **Lightweight ResNet**  
   - Residual blocks carefully sized to stay **below 5M parameters**.  
   - Full skip connections ensure stable, deep training on 32×32 images.

2. **Data Augmentation**  
   - **AutoAugment** (CIFAR-10 policy), random cropping, horizontal flips, and per-channel normalization.  
   - Empirically boosts validation accuracy by ~5–6% compared to basic augmentations.

3. **Training Strategy**  
   - Optimizer: **SGD** with momentum=0.9, weight decay=1e-4.  
   - **Multi-Step LR** schedule: decays at epochs 30, 60, 80, 90 (initial LR=0.1).  
   - Trains for **100 epochs**, batch size=256 (configurable in the script).

4. **Results**  
   - **Validation Accuracy**: ~90+% by epoch 100.  
   - **Hidden Test Accuracy**: ~83–84%.  
   - See included **screenshots** or logs for training/validation curves (loss & accuracy).

## Architecture & Methodology
![image](https://github.com/user-attachments/assets/7fe36416-61ce-482a-9c35-b89495f58b13)

## File Structure
- `custResnet.ipynb`  
  - Main script loading the dataset, defining the model, and running training & inference.  
- `submission.csv`  
  - Output predictions on the hidden/unlabeled test set.  
- `README.md`  
  - This file, summarizing the project.  

## Results
1. Training & Validation Loss:
![image](https://github.com/user-attachments/assets/6db4e0bb-32be-4477-90d2-d09f123f2d12)

2. Training & Validation Accuracy:
![image](https://github.com/user-attachments/assets/55e3bdc3-14b1-4ba5-aa62-b414bc6f392f)

## Model Architecture Details
A custom ResNet is defined with smaller channel sizes (keeping total parameters under 5M):

1. ResidualBlock class:
Two 3×3 convolutions
BatchNorm + ReLU
Optional 1×1 convolution for skip connection if shape changes
2. CustomResNet class:
Initial 3×3 convolution (3 → 39 channels)
Four main layers of residual blocks:
  a. 39 → 39 (4 blocks, stride=1)
  b. 39 → 78 (4 blocks, stride=2)
  c. 78 → 156 (3 blocks, stride=2)
  d. 156 → 305 (2 blocks, stride=2)
Adaptive average pooling
Fully connected layer (305 → 10)

## Model Summary (from torchsummary):
``` bash
Total params: 4,738,952
Trainable params: 4,738,952
Non-trainable params: 0
```

## Usage
1. **Install** dependencies: PyTorch, torchvision, numpy, matplotlib, pandas.  
2. **Download** CIFAR-10 and place batches in `cifar-10-python/`. Update file paths in the script if needed.  
3. **Run** the script:
   ```bash
   python custResnet.py
  ```
