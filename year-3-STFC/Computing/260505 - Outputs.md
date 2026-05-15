# tags
[[260504 - Plan For Assignment 2 FINAL]]
[[pytorch]]

# final report f1 score etc 
```
----------------------------------------
Base Line CNN
----------------------------------------
average time per batch: 2.07766 micro seconds | std deviation of 0.40724 micro seconds
----------------------------------------
              precision    recall  f1-score   support

  Meningioma     0.6124    0.8316    0.7054        95
      Glioma     0.9198    0.7890    0.8494       218
   Pituitary     0.9441    0.9247    0.9343       146

    accuracy                         0.8410       459
   macro avg     0.8254    0.8484    0.8297       459
weighted avg     0.8639    0.8410    0.8466       459
```





















# CODE CHANGES
- need to rename the loss and validation code
### Optimizer choice 
I have been experimenting with Adam optimizer of SGD, this is because

"As a rule of thumb ADAM is more robust to bad hyperparameters initialization and will often achieve convergence fast enough, but SGD can be much faster if you understand what you are doing." - bloke on reddit 

 as a result i have decided to start with ADAM optimizer as i am not an expert 

- as i get a feel for the correct lr, i swap back to SGD. 
	- need to look into `momentum` and `lr` 


### Variable Learning rate
used this to help lower learning rate over time.

### model is getting stuck 
- we got stuck with a 46% accuracy, which was noted to be predicting glioma every time so we need to have some changes
- the solution is to weight the class counts.
```
0%|          | 0/30 [00:00<?, ?it/s]

 train loss: 0.965 | train accuracy: 47.341%

  3%|▎         | 1/30 [00:20<10:07, 20.94s/it]

 eval loss: 0.883 | eval accuracy: 60.994%
 train loss: 0.863 | train accuracy: 55.737%

  7%|▋         | 2/30 [00:42<09:50, 21.08s/it]

 eval loss: 0.864 | eval accuracy: 57.644%
 train loss: 0.835 | train accuracy: 55.504%

 10%|█         | 3/30 [01:03<09:28, 21.04s/it]

 eval loss: 0.884 | eval accuracy: 61.619%
 train loss: 0.802 | train accuracy: 56.996%

 13%|█▎        | 4/30 [01:24<09:07, 21.06s/it]

 eval loss: 0.843 | eval accuracy: 62.869%
 train loss: 0.735 | train accuracy: 66.978%

 17%|█▋        | 5/30 [01:45<08:48, 21.14s/it]

 eval loss: 0.849 | eval accuracy: 66.827%
 train loss: 0.698 | train accuracy: 71.129%

 20%|██        | 6/30 [02:06<08:27, 21.14s/it]

 eval loss: 0.758 | eval accuracy: 67.853%
 train loss: 0.689 | train accuracy: 70.103%

 23%|██▎       | 7/30 [02:29<08:19, 21.73s/it]

 eval loss: 0.726 | eval accuracy: 70.353%
 train loss: 0.649 | train accuracy: 72.062%

 27%|██▋       | 8/30 [02:51<08:00, 21.86s/it]

 eval loss: 0.655 | eval accuracy: 68.462%
 train loss: 0.569 | train accuracy: 74.580%

 30%|███       | 9/30 [03:13<07:39, 21.89s/it]

 eval loss: 0.589 | eval accuracy: 75.657%
 train loss: 0.557 | train accuracy: 76.166%

 33%|███▎      | 10/30 [03:35<07:19, 21.96s/it]

 eval loss: 0.584 | eval accuracy: 75.449%
 train loss: 0.492 | train accuracy: 77.845%

 37%|███▋      | 11/30 [03:57<06:58, 22.02s/it]

 eval loss: 0.575 | eval accuracy: 75.032%
 train loss: 0.484 | train accuracy: 77.985%

 40%|████      | 12/30 [04:20<06:37, 22.10s/it]

 eval loss: 0.570 | eval accuracy: 75.657%
 train loss: 0.470 | train accuracy: 78.405%

 43%|████▎     | 13/30 [04:42<06:16, 22.14s/it]

 eval loss: 0.578 | eval accuracy: 75.128%
 train loss: 0.468 | train accuracy: 78.172%

 47%|████▋     | 14/30 [05:04<05:51, 22.00s/it]

 eval loss: 0.583 | eval accuracy: 75.449%
 train loss: 0.466 | train accuracy: 79.011%

 50%|█████     | 15/30 [05:26<05:30, 22.02s/it]

 eval loss: 0.571 | eval accuracy: 75.657%
 train loss: 0.462 | train accuracy: 79.944%

 53%|█████▎    | 16/30 [05:48<05:09, 22.09s/it]

 eval loss: 0.565 | eval accuracy: 76.074%
 train loss: 0.464 | train accuracy: 79.011%

 57%|█████▋    | 17/30 [06:10<04:48, 22.17s/it]

 eval loss: 0.571 | eval accuracy: 76.490%
 train loss: 0.462 | train accuracy: 78.825%

 60%|██████    | 18/30 [06:32<04:25, 22.16s/it]

 eval loss: 0.575 | eval accuracy: 75.449%
 train loss: 0.454 | train accuracy: 79.384%

 63%|██████▎   | 19/30 [06:54<04:03, 22.11s/it]

 eval loss: 0.571 | eval accuracy: 76.282%
 train loss: 0.460 | train accuracy: 79.431%

 67%|██████▋   | 20/30 [07:16<03:40, 22.08s/it]

 eval loss: 0.567 | eval accuracy: 76.282%
 train loss: 0.453 | train accuracy: 79.897%

 70%|███████   | 21/30 [07:38<03:17, 22.00s/it]

 eval loss: 0.565 | eval accuracy: 75.865%
 train loss: 0.447 | train accuracy: 80.037%

 73%|███████▎  | 22/30 [08:00<02:55, 21.97s/it]

 eval loss: 0.570 | eval accuracy: 75.657%
 train loss: 0.448 | train accuracy: 79.618%

 77%|███████▋  | 23/30 [08:22<02:33, 21.93s/it]

 eval loss: 0.569 | eval accuracy: 75.865%
 train loss: 0.451 | train accuracy: 79.338%

 80%|████████  | 24/30 [08:44<02:10, 21.81s/it]

 eval loss: 0.571 | eval accuracy: 75.657%
 train loss: 0.445 | train accuracy: 79.524%
```

### Augmentations 
- when looking for ways to optimize the pre trained model we decided to use augmentations,
- initally decided not too, this is because we were worried about loosing the ability to diagnose
	- doctor never sees the inverted and flipped images, just the labels 
	- it helps prevent overfitting
- flips and rotations were deemed valid 
- brightness and contrast augmentations were not outside of transferring  to resent, as adjusting brightness contrast can erase important tumor/brain boundaries that are crucial for diagnosis. 
- **FOR REPORT:** acknowledge these factors, but say why its fine and I chose these specific transforms. if we were doing a "tumor area" classification, then this would be very different. 
- can also help learn better from the same patient 
- Swapped too `scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, patience=5, factor=0.5)` **learn more about this** 

### baselinecnn training performance
[[fig-260506-base-line-cnn-training-performance.png]]
![[fig-260506-base-line-cnn-training-performance.png]]


# temporary notes to be put in the file stored on source room laptop on Monday 

### why choose resnet 18?
please help me get started on the resnet 18 model, why this transfer learning model, what is a baseline architecture for such a transfer learning model

**The core problem with deeper ResNets on small datasets** is the parameter count:

|Model|Parameters|Good for|
|---|---|---|
|ResNet18|11.7M|~1k–50k images|
|ResNet50|25.6M|~50k–500k images|
|ResNet152|60.2M|millions of images|

Your dataset has ~3,064 images. ResNet50+ will memorise the training set rather than generalise — you'd see training accuracy hit ~99% while validation plateaus at 60–70%. ResNet18 has enough capacity to learn your 3-class problem without that risk.

**The other reason** is that the ImageNet pretraining is already doing the heavy lifting. The early layers of any ResNet already know how to detect edges, textures, and shapes from 1.2 million images. For MRI scans, those low-level features transfer surprisingly well — tumour boundaries, tissue texture gradients, and structural asymmetries are all edge/texture phenomena at the feature level.

---

### What ResNet18 actually is — the architecture

ResNet introduced **residual connections** (skip connections) to solve the vanishing gradient problem that made deep networks hard to train. The key idea:

```
Normal block:    x → [Conv → BN → ReLU → Conv → BN] → y

Residual block:  x → [Conv → BN → ReLU → Conv → BN] → + x → ReLU → y
                                                        ↑
                                                   identity skip
```

Because the gradient can flow directly through the skip connection, you can stack many layers without the gradient vanishing to zero before it reaches the early layers.

ResNet18's full structure:

```
Input (3×224×224)
    ↓
Conv1 (7×7, 64 filters, stride 2)  →  112×112×64
MaxPool (3×3, stride 2)             →   56×56×64
    ↓
Layer1: 2× BasicBlock (64 filters)  →   56×56×64
Layer2: 2× BasicBlock (128 filters) →   28×28×128   ← stride 2 downsamples
Layer3: 2× BasicBlock (256 filters) →   14×14×256   ← stride 2 downsamples
Layer4: 2× BasicBlock (512 filters) →    7×7×512    ← stride 2 downsamples
    ↓
AdaptiveAvgPool → 1×1×512 (global average pool)
Flatten → 512
FC → 1000  (ImageNet classes — THIS IS WHAT YOU REPLACE)
```

Each `BasicBlock` is:

```
Conv 3×3 → BatchNorm → ReLU → Conv 3×3 → BatchNorm → (+skip) → ReLU
```

---

### Your fine-tuning strategy

You have two valid approaches:

**Option A — Freeze all, train only the head** (what the new cells do)

- Fast, low overfitting risk, good if ImageNet features transfer well
- Trains in a few minutes per epoch

**Option B — Freeze early layers, unfreeze later layers** (more powerful)

python

```python
# unfreeze layer4 and the FC head for fine-tuning
for name, param in resnet_model.named_parameters():
    param.requires_grad = name.startswith("layer4") or name.startswith("fc")
```

This lets the model adapt the high-level features (layer4 learns tumour-specific patterns) while keeping the stable low-level filters frozen. Often gives 2–5% better accuracy on medical imaging tasks.

For your report, **Option A is perfectly justifiable** given the small dataset. You could mention Option B as a direction for future work.

---

### The FC replacement explained

python

```python
# original: nn.Linear(512, 1000)  — ImageNet's 1000 classes
# yours:
resnet_model.fc = nn.Linear(512, 3)  # 512 is resnet18's penultimate feature size
```

When you replace the FC layer, PyTorch initialises it with random weights — so it's the only part that needs training from scratch. Everything before it is frozen pretrained weights. This is why you use `lr=1e-4` (ten times smaller than your baseline's `1e-3`) — you're nudging a mostly-trained network, not training from random initialisation.

---

For your **report**, the justification chain is:

1. Dataset is small (~3k images) → deeper models overfit
2. ResNet18's residual connections enable stable gradient flow across 18 layers
3. ImageNet pretraining provides transferable low-level features
4. Only the FC head requires training → fast convergence, low overfitting risk
5. `lr=1e-4` chosen to avoid destroying pretrained weights with large gradient updates

### example acrhitecture of the baseline cnn
![[fig-260509-baselinecnn-architecture.png]]

### resnet architecture 
![[fig-260509-resnet18_flowchart.png]]
### baseline architecture 
![[fig-260509-baseline_cnn_20260509-093203_acc77.9.png]]