# Setup 
- inside of a Kaggle notebook editor
- 30 GPU hours Tesla T4x2 per week
-  scalars and vectors, lowercase 
- matrix and tensors UPPERCASE
### train resnet image datset and normalisation 
When ResNet (and similar models) were trained on ImageNet, the input pixels weren't raw values between 0–255, or even just scaled to 0–1. They were **shifted and scaled** so that each colour channel had a specific mean and standard deviation across the whole ImageNet dataset.
Converting grayscale images to RGB for neural networks (e.g., ResNet, EfficientNet) is standardly done by duplicating the single grayscale channel three times to create an artificial 3-channel (RGB) image, or by using a 
 convolution layer to map 1 channel to 3. This allows 1-channel data to fit 3-channel input requirements without losing information, typically using OpenCV (`cv2.cvtColor`) or NumPy (`np.stack`)
 Convolution Layer (Learned): Add a convolutional layer at the beginning of the model to learn the optimal transformation from 1 channel to 3.
 `ImageFolder` dataset
- Resize to **224×224** (why? — think about what pretrained models expect)
	- because we need to change the image size to what the pre trained models expect resnet and other pre trained models were trained on 224x224. 
- Normalise to ImageNet stats (we'll discuss why)
	- restnet expects RGG, which we have 3064 T1-weighted contrast-inhanced images with three kinds of brain tumor.
	https://docs.pytorch.org/vision/0.9/transforms.html - documentation for pytorch
- 70/15/15 train/val/test random split
- setup data loaders, what are they? find in pytorch documentation. "A **DataLoader** is a utility that prepares your data in batches for training machine learning models. Think of it as a smart conveyor belt that feeds data to your model in organized chunks."
	- **Benefits:**
		- 32 images processed simultaneously
		- GPU can parallelize computation
		- Much faster training
- added a function to get dataset size, so it will work with any size of dataset, more future proof (I think this is one point of the mark scheme that states this.)
## Splitting Data 
- small dataset 
- train and test 80/20 
- non equal 
# Baseline CNN 

# Transfer Learning CNN

# Evaluation and comparison