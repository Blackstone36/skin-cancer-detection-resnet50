Skin Cancer Detection using Deep Learning

A deep learning-based computer vision project for classifying skin lesion images as benign or malignant. The implementation uses Transfer Learning with ResNet50, along with image preprocessing, data generators, regularization, early stopping, and model evaluation.

Medical disclaimer: This project is intended for educational and research purposes only. It is not a medical diagnostic tool and should not be used as a substitute for examination or advice from a qualified healthcare professional.

Overview

Early identification of suspicious skin lesions can support timely medical evaluation. This project explores automated image classification using deep learning.

The project workflow includes:

Loading and organizing skin lesion images.

Creating image file paths and labels.

Splitting the data into training and testing sets.

Preprocessing images using the ResNet50 preprocessing function.

Resizing images to 100 × 100 × 3 for the implemented ResNet50 pipeline.

Applying a pretrained ResNet50 model with ImageNet weights.

Adding fully connected classification layers.

Using dropout and L2 regularization to reduce overfitting.

Training with early stopping.

Evaluating the model using accuracy, loss, predictions, and a classification report.

The project documentation also describes a broader skin-lesion detection workflow involving preprocessing, segmentation, feature extraction, and classification. fileciteturn1file7

Classification

The current notebook implementation contains two classes:

Benign

Malignant

The notebook explicitly loads these two class folders from the dataset. fileciteturn2file0

The accompanying project documentation discusses a seven-class skin-lesion classification concept, including melanocytic nevi, melanoma, benign keratosis, basal cell carcinoma, actinic keratoses, vascular lesions, and dermatofibroma. However, the supplied notebook currently implements a binary benign/malignant classifier, so this README describes the implemented notebook rather than treating the seven-class design as the current model output. fileciteturn1file7

Model Architecture

The implemented model uses ResNet50 as a pretrained feature extractor:

Input Image
     ↓
ResNet50 (ImageNet pretrained weights)
     ↓
Global Average Pooling
     ↓
Dense Layer (128 / 64 units depending on experiment)
     ↓
Batch Normalization / Dropout / L2 Regularization
     ↓
Dense Layer
     ↓
Softmax Output
     ↓
Benign / Malignant

The notebook first creates a frozen ResNet50 backbone and adds dense classification layers. Later experiments introduce dropout, L2 regularization, batch normalization, and early stopping. fileciteturn1file3

Dataset

The project documentation states that the skin-cancer dataset was obtained from Kaggle and describes a dataset of approximately 10,000 images. fileciteturn1file0

For the supplied notebook, the data is loaded from directory-based class folders:

train/
├── benign/
└── malignant/

The notebook uses train_test_split with test_size=0.25 and random_state=42, followed by Keras dataframe-based image generators. fileciteturn1file4

Important: The original documentation describes an 8,000/2,000 training/testing split, while the supplied notebook code creates a 75%/25% split. Use the actual dataset and notebook configuration when reproducing the experiment. fileciteturn1file0 fileciteturn1file4

Technologies Used

Python 3

TensorFlow

Keras

NumPy

Pandas

OpenCV

Matplotlib

Seaborn

Scikit-learn

Google Colab / Google Drive

ResNet50

The project documentation lists TensorFlow, Pandas, NumPy, OpenCV, and Keras among the main libraries. fileciteturn1file2

Image Processing

The project uses image-processing techniques as part of the overall workflow. The documentation describes:

Image preprocessing

Grayscale conversion in the described preprocessing workflow

Image resizing

Image segmentation

Otsu thresholding

Texture-based feature extraction

GLCM-based texture statistics

The documented GLCM features include correlation, contrast, energy, and homogeneity. fileciteturn1file7

For the supplied ResNet50 notebook, images are passed through preprocess_input and generated at 100 × 100 resolution for the main training pipeline. fileciteturn1file4

Training

The notebook experiments with several model configurations. The main improvements include:

Transfer learning using ResNet50.

Frozen pretrained backbone.

Dense classification layers.

L2 kernel regularization.

Dropout with a rate of 0.5.

Batch normalization.

Early stopping.

Restoring the best model weights.

Mixed-precision training to improve computational efficiency when supported by the hardware.

The notebook also checks GPU availability and configures TensorFlow mixed precision. fileciteturn1file8

Evaluation

The project evaluates the trained model using:

Test loss

Test accuracy

Classification report

Predicted class labels

Accuracy curves

Loss curves

Visual comparison of true and predicted labels

The supplied project report records a test loss of 0.61602 and a test accuracy of 82.73% for the reported experiment. fileciteturn1file9

Results may differ when the notebook is rerun because of dataset versions, hardware, preprocessing, training duration, and model configuration.

Installation

Create a Python environment and install the required packages:

pip install tensorflow keras numpy pandas opencv-python matplotlib seaborn scikit-learn

If you are using Google Colab, most dependencies can be installed directly in the notebook.

Running the Project

Option 1 — Google Colab

Open skin cancer.ipynb in Google Colab.

Upload or connect the dataset to Google Drive.

Update the dataset path in the notebook:

file_path = "/content/drive/MyDrive/Cancer detection/archive (2)/train"

Make sure the directory contains:

train/
├── benign/
└── malignant/

Run the notebook cells in order.

Train the model.

Evaluate the model on the test data.

Use the prediction section to classify a new image.

The notebook uses Google Drive mounting and the path above for loading the training images. fileciteturn2file0

Option 2 — Local Python Environment

For local execution, replace the Google Drive dataset path with a local path, for example:

file_path = "./dataset/train"

Then keep the same directory structure:

dataset/
└── train/
    ├── benign/
    └── malignant/

Predicting a New Image

The notebook demonstrates loading an image with OpenCV, resizing it, applying ResNet preprocessing, and passing it to the trained model.

Example workflow:

img = cv2.imread(img_path)
img = cv2.resize(img, (100, 100))
x = np.expand_dims(img, axis=0)
x = preprocess_input(x)

result = model.predict(x)
print(result)

The notebook also saves the trained model as:

model_resnet50.h5

and demonstrates loading the saved model for later prediction. fileciteturn1file3

Project Structure

A recommended GitHub repository structure is:

skin-cancer-detection/
│
├── skin cancer.ipynb
├── model_resnet50.h5
├── README.md
├── requirements.txt
│
├── dataset/
│   └── train/
│       ├── benign/
│       └── malignant/
│
└── results/
    ├── accuracy.png
    ├── loss.png
    └── classification_report.txt

 

Results

Metric

Reported Result

Test Loss

0.61602

Test Accuracy

82.73%

These values are taken from the supplied project documentation. fileciteturn1file9

Key Features

Skin lesion image classification

Benign vs. malignant prediction

Transfer learning with ResNet50

Image preprocessing

Keras ImageDataGenerator pipeline

Regularization using dropout and L2

Batch normalization

Early stopping

Model saving and loading

Classification report

Accuracy and loss visualization

GPU and mixed-precision support

Limitations

The current notebook implements binary classification rather than the seven-class design described in parts of the project documentation.

The model is trained on a specific dataset and may not generalize to all skin types, imaging devices, or clinical environments.

Reported accuracy alone is not sufficient to establish clinical reliability.

The project should be considered a research/educational prototype rather than a clinical diagnostic system.

A medical professional should make any real-world diagnosis.

Future Improvements

Possible future work includes:

Extending the model to the seven lesion categories described in the project documentation.

Using stronger data augmentation.

Addressing class imbalance.

Fine-tuning selected ResNet50 layers.

Reporting precision, recall, F1-score, ROC-AUC, and confusion matrix.

Using explainability techniques such as Grad-CAM.

Improving image quality and lesion segmentation.

Building a web interface for image upload and prediction.

Validating the model on an independent clinical dataset.

The project documentation also proposes extending the system to different skin-related diseases and includes a doctor-reference/feedback component. fileciteturn1file9

References

The supplied project documentation references work on skin-cancer detection, deep neural networks, and skin-lesion classification, including:

University of Waterloo — Skin Cancer Detection research/demo.

World Cancer Research Fund — Skin cancer statistics.

Hinton, Krizhevsky & Sutskever — ImageNet Classification with Deep Convolutional Neural Networks.

Yao — Evolving Artificial Neural Networks.

Mahbod et al. — Skin Lesion Classification Using Hybrid Deep Neural Networks.

Harangi, Baran & Hajdu — Classification of Skin Lesions Using an Ensemble of Deep Neural Networks.

The references are listed in the supplied project documentation. fileciteturn1file3

License
University of Management and Technology
corse:Programming for Artificial Intelligence
Team members:
Hafsa Rehman
Sara Akmal

Disclaimer

This repository is an academic/research project demonstrating deep-learning-based image classification. Predictions from this model should not be interpreted as a medical diagnosis. Always consult a qualified dermatologist or other healthcare professional for medical evaluation.
