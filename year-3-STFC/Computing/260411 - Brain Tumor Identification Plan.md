### Tags 
[[python]]
[[machine learning]]
[[pytorch]]

### Rough Plan
- get a way to use machine learning hardware using Kaggle
- how do i get GPU hours etc 
-  **Run everything in Kaggle notebook. 
- we need to use a CNN 
-  Transfer learning due to a small dataset. 
- need a method of splitting the data fairly. 

### NN Structure 
```
Input: Brain MRI (512×512×3 or 256×256×3)
        ↓
[Pre-trained ResNet-50 backbone]
- Layer 1: Conv + ReLU + MaxPool (stride 2)
- Layer 2-4: Residual blocks (64→256 channels)
- Layer 5-8: Residual blocks (256→512 channels)
        ↓
Global Average Pooling (512,) → (512)
        ↓
Dropout (0.4-0.5) → (512)  ← Prevent overfitting
        ↓
Dense Layer: 256 units, ReLU
        ↓
Dropout (0.3) → (256)
        ↓
Dense Layer: 128 units, ReLU
        ↓
Dropout (0.2) → (128)
        ↓
Output Layer: 4 units, Softmax
        ↓
4-class probabilities (Glioma, Meningioma, Pituitary, No Tumor)
```

• How to validate your approach, including intermediate steps
• How to present your results appropriately
• How to quantitatively assess your results
• How to factories your code (eg. what functions to use)
• Whether and how you can make your code re-usable


Random image-level split: leakage risk in your report. For a physics coursework (not a medical deployment!), this is a reasonable and honest tradeoff. Your markers will respect you more for flagging it than ignoring it.

### Article 
https://cs231n.github.io/transfer-learning/ 
using a pretrained model as a feature extractor
	- **Pretrained models**. Since modern ConvNets take 2-3 weeks to train across multiple GPUs on ImageNet, it is common to see people release their final ConvNet checkpoints for the benefit of others who can use the networks for fine-tuning. For example, the Caffe library has a [Model Zoo](https://github.com/BVLC/caffe/wiki/Model-Zoo) where people share their network weights.

- resize to 224x224 - because this is what resnet is trained on
- normalize to ImageNet stats
- 70/15/15 split 

### train resnet image datset and normalisation 
When ResNet (and similar models) were trained on ImageNet, the input pixels weren't raw values between 0–255, or even just scaled to 0–1. They were **shifted and scaled** so that each colour channel had a specific mean and standard deviation across the whole ImageNet dataset.

### normalise to resnet 
Converting grayscale images to RGB for neural networks (e.g., ResNet, EfficientNet) is standardly done by duplicating the single grayscale channel three times to create an artificial 3-channel (RGB) image, or by using a 
 convolution layer to map 1 channel to 3. This allows 1-channel data to fit 3-channel input requirements without losing information, typically using OpenCV (`cv2.cvtColor`) or NumPy (`np.stack`)
 Convolution Layer (Learned): Add a convolutional layer at the beginning of the model to learn the optimal transformation from 1 channel to 3.

## full plan 
Now we have enough to lay out the full plan. Here it is in order:

### Stage 1 — Data Pipeline

- `ImageFolder` dataset
- Resize to **224×224** (why? — think about what pretrained models expect)
	- because we need to change the image size to what the pre trained models expect resnet and other pre trained models were trained on 224x224. 
- Normalise to ImageNet stats (we'll discuss why)
	- restnet expects RGG, which we have greyscale
- 70/15/15 train/val/test random split

### Stage 2 — Baseline Model

- Small custom CNN (3–4 conv layers)
- No pretrained weights
- Gives you a floor to beat

### Stage 3 — Improved Model

- Pretrained backbone (e.g. ResNet-18)
- Fine-tuning strategy to discuss
- Augmentation added here

### Stage 4 — Evaluation

- Confusion matrix
- Per-class F1, precision, recall
- ROC-AUC (one-vs-rest)
- **Not just accuracy**