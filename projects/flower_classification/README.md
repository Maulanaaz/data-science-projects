# 🌸 Flower Classification

## 🧠 Goal

Image classification of flowers using a Convolutional Neural Network (CNN) with transfer learning based on the MobileNetV2 architecture.

## 📦 Dataset

- Source: [Kaggle - Flower Classification](https://www.kaggle.com/datasets/marquis03/flower-classification)
- 14 distinct flower categories

## ⚙️ Workflow

1. Image preprocessing and resizing  
2. Load MobileNetV2 with pre-trained weights from ImageNet  
3. Add custom classification head (fine-tuning)  
4. Compile and train the model with early stopping and model checkpointing  

## 📊 Model Performance

- **Training Accuracy**: 98.92%  
- **Validation Accuracy**: 90.78%  
- **Validation Loss**: 0.5068  

📌 The best model was automatically saved using `ModelCheckpoint`, based on validation accuracy.

### 🔍 Observations

- Training and validation accuracy converged smoothly, indicating no severe overfitting.  
- Validation accuracy stabilized around ~91%, suggesting that MobileNetV2 performed well for this 14-class classification task.

## 📁 File Structure

- `flower.ipynb` – Complete training pipeline and evaluation  
- `my_model.h5` – Saved model weights (optional for inference)

## 🧠 Model Architecture

- **Base Model**: MobileNetV2 (pre-trained on ImageNet, partially fine-tuned)
- **Classification Head**:

| Layer                          | Output Shape |
|--------------------------------|--------------|
| Conv2D(64, 3×3, ReLU) + MaxPooling2D | 64 × 64 × 64 |
| GlobalAveragePooling2D         | 64           |
| BatchNormalization             | 64           |
| Dense(512, ReLU)               | 512          |
| Dense(256, ReLU)               | 256          |
| Dense(128, ReLU)               | 128          |
| **Dense(14, Softmax)**         | 14 classes   |

> `Dropout` layers (0.2–0.4) are applied throughout the fully-connected layers to reduce overfitting.

---

