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