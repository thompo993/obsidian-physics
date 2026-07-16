---
tags:
  - note
  - "#coding"
  - machine-learning
created: 2026-04-11
---


https://www.reddit.com/r/MachineLearning/comments/1ckrzq6/d_is_there_a_more_systematic_way_of_choosing_the/

# CODING CONVENTIONs





- [x] Complete Coding Assignment and report 📅 2026-05-23 ✅ 2026-06-02
### Rough Plan
- get a way to use machine learning hardware using Kaggle
- how do i get GPU hours etc 
-  **Run everything in Kaggle notebook. 
- we need to use a CNN 
-  Transfer learning due to a small dataset. 
- need a method of splitting the data fairly. 
- **consider loss function **
- **Consider optimizer**
- Save and timestamp models to prevent overuse of GPUs

---

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

---

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
 
---

## full plan 
Now we have enough to lay out the full plan. Here it is in order:

### Stage 1 — Data Pipeline

- `ImageFolder` dataset
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
#### Claude: Two Questions To Think About Before You Run It

1. Which class do you expect the model to struggle with most, and why?
	1. meningioma, easy to spot 
	2. glioma, easy for human, but lots of variety, could cause a NN problems
	3. repetitive - easy for a nn to spot. 
2. In a medical context, which error is worse — a **false positive** (predicting tumor when there isn't one) or a **false negative** (missing a tumor)? How does that affect which metric you should priorities?
	1. False positive is better than a false negative, we would rather be on the side of caution regarding this, there are two cases
		1. FP - the patient has further investigation, by  a human, and it is clear they do not have a tumor 
		2. FN - the patient leaves with a tumor that does not get further investigation. 
	2. Therefore the metric that should be prioritized is the True positive metric. 
#### Claude: Two Questions To Think About Before You Run It - feedback
#### On Your Class Difficulty Predictions

Your meningioma prediction was **correct** — it has the lowest F1 (0.93) and lowest recall (0.91). Good intuition.

However, refine your reasoning slightly for the report. The key reason meningioma is harder isn't just "variety" — it's that meningiomas tend to appear at the **brain boundary** and can look similar to normal tissue in some slices. Glioma has the most samples (1426) so the model has seen more examples of it.
#### 🎯 On False Negatives vs False Positives

Spot on. The metric you're describing — minimising false negatives — is **recall** (also called sensitivity):

```
Recall = True Positives / (True Positives + False Negatives)
```

High recall means few tumors are missed. For your report, frame it this way:
*"In a clinical screening context, recall is prioritised over precision, since a false negative — failing to detect a tumour — carries greater risk than a false positive, which would simply trigger further investigation."*
### Stage 2 — Baseline Model

- Small custom CNN (3–4 conv layers)
- No pretrained weights
- Gives you a floor to beat
#### Baseline model results 
```
Epoch 01/20 | Train Loss: 0.7133 Acc: 0.685 | Val Loss: 0.4637 Acc: 0.794
Epoch 02/20 | Train Loss: 0.4374 Acc: 0.826 | Val Loss: 0.3351 Acc: 0.863
Epoch 03/20 | Train Loss: 0.3309 Acc: 0.865 | Val Loss: 0.3324 Acc: 0.829
Epoch 04/20 | Train Loss: 0.2557 Acc: 0.897 | Val Loss: 0.2305 Acc: 0.909
Epoch 05/20 | Train Loss: 0.1949 Acc: 0.920 | Val Loss: 0.2418 Acc: 0.918
Epoch 06/20 | Train Loss: 0.1312 Acc: 0.943 | Val Loss: 0.2111 Acc: 0.939
Epoch 07/20 | Train Loss: 0.1031 Acc: 0.960 | Val Loss: 0.1887 Acc: 0.946
Epoch 08/20 | Train Loss: 0.1025 Acc: 0.965 | Val Loss: 0.1634 Acc: 0.941
Epoch 09/20 | Train Loss: 0.0694 Acc: 0.975 | Val Loss: 0.2076 Acc: 0.939
Epoch 10/20 | Train Loss: 0.0559 Acc: 0.980 | Val Loss: 0.1819 Acc: 0.928
Epoch 11/20 | Train Loss: 0.0548 Acc: 0.980 | Val Loss: 0.1756 Acc: 0.948
Epoch 12/20 | Train Loss: 0.0337 Acc: 0.988 | Val Loss: 0.1521 Acc: 0.961
Epoch 13/20 | Train Loss: 0.0305 Acc: 0.987 | Val Loss: 0.2592 Acc: 0.935
Epoch 14/20 | Train Loss: 0.0244 Acc: 0.993 | Val Loss: 0.1454 Acc: 0.963
Epoch 15/20 | Train Loss: 0.0280 Acc: 0.992 | Val Loss: 0.1625 Acc: 0.950
Epoch 16/20 | Train Loss: 0.0469 Acc: 0.986 | Val Loss: 0.1419 Acc: 0.952
Epoch 17/20 | Train Loss: 0.0321 Acc: 0.989 | Val Loss: 0.1845 Acc: 0.957
Epoch 18/20 | Train Loss: 0.0252 Acc: 0.992 | Val Loss: 0.1477 Acc: 0.954
Epoch 19/20 | Train Loss: 0.0146 Acc: 0.995 | Val Loss: 0.2923 Acc: 0.950
Epoch 20/20 | Train Loss: 0.0386 Acc: 0.986 | Val Loss: 0.2431 Acc: 0.952
```

```
precision    recall  f1-score   support

  meningioma       0.95      0.91      0.93       129
      glioma       0.94      0.97      0.95       200
   pituitary       0.99      0.98      0.99       130

    accuracy                           0.96       459
   macro avg       0.96      0.95      0.96       459
weighted avg       0.96      0.96      0.96       459
```
### Stage 3 — Improved Model

- Pretrained backbone (e.g. ResNet-18)
- Fine-tuning strategy to discuss
- Augmentation added here

### Stage 4 — Evaluation

- Confusion matrix
- Per-class F1, precision, recall
- ROC-AUC (one-vs-rest)
- **Not just accuracy** 
- [x] Ensure comments are updated inside assignment 2 jupyter notebook ⏫ 📅 2026-05-23 ✅ 2026-06-02