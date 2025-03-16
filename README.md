# Weed Detection using InceptionV3 and YOLO Data Processing

## Overview
This project implements a weed detection model using **InceptionV3** for binary classification. The dataset consists of weed images and their corresponding YOLO annotation files. A custom **YOLODataGenerator** is used to preprocess the images and labels before training the model.

## Dataset
The dataset is organized as follows:
```
WeedCrop Image Dataset/
    ├── train/
    │   ├── images/  # Training images
    │   ├── labels/  # Corresponding YOLO labels (.txt)
    ├── valid/
    │   ├── images/  # Validation images
    │   ├── labels/  # Validation YOLO labels
    ├── test/
    │   ├── images/  # Test images
    │   ├── labels/  # Test YOLO labels
```

## Dependencies
Ensure you have the following dependencies installed:
```bash
pip install tensorflow opencv-python numpy scikit-learn matplotlib
```

## Data Preprocessing
- **Custom Data Generator**: `YOLODataGenerator` loads images and corresponding labels.
- **Augmentation**: Includes random transformations like rotation, zooming, and flipping.
- **Label Handling**: Extracts class ID from YOLO `.txt` files.

## Model Architecture
- **Base Model**: InceptionV3 (pretrained on ImageNet, used for feature extraction).
- **Added Layers**:
  - Global Average Pooling
  - Fully Connected Dense Layer (1024 units, ReLU activation)
  - Dropout (0.5)
  - Output Layer (1 neuron, Sigmoid activation for binary classification)

## Training
### Hyperparameters
- **Input Image Size**: `(139, 139, 3)`
- **Batch Size**: `16`
- **Optimizer**: Adam (`learning_rate=0.001` with decay)
- **Loss Function**: Binary Cross-Entropy
- **Callbacks**:
  - EarlyStopping (patience=5)
  - ReduceLROnPlateau (factor=0.2, patience=3)
  - ModelCheckpoint (saving best model)
  - LearningRateScheduler (custom step decay)

### Command to Train the Model
Run the following script:
```python
python train.py
```

## Model Evaluation
The trained model is evaluated on test images:
```python
evaluate_model(model, test_images_dir, test_labels_dir, (139, 139))
```
It outputs a classification report including precision, recall, and F1-score.

## Results & Visualization
Training and validation accuracy/loss plots are generated using:
```python
plot_metrics(history)
```

## Model Saving & Deployment
- The best model is saved as `plant_disease_model_inception.keras`.
- Can be loaded for inference using:
```python
from tensorflow.keras.models import load_model
model = load_model('plant_disease_model_inception.keras')
```

## Future Improvements
- Extend model for **multi-class classification**.
- Improve dataset labeling **(semi-supervised learning)**.
- Optimize model performance for **real-time inference on mobile devices**.

