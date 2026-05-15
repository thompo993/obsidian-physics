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


# resnet18 run @ 15/05/26 23:05
```
 0%|          | 0/30 [00:00<?, ?it/s]

 train loss: 0.617 | train accuracy: 73.041%

  3%|▎         | 1/30 [00:24<11:43, 24.27s/it]

 eval loss: 0.499 | eval accuracy: 83.462%
lr: 0.0001
 train loss: 0.321 | train accuracy: 88.060%

  7%|▋         | 2/30 [00:48<11:22, 24.37s/it]

 eval loss: 0.318 | eval accuracy: 88.349%
lr: 0.0001
 train loss: 0.262 | train accuracy: 89.879%

 10%|█         | 3/30 [01:13<11:07, 24.73s/it]

 eval loss: 0.386 | eval accuracy: 83.141%
lr: 0.0001
 train loss: 0.226 | train accuracy: 91.604%

 13%|█▎        | 4/30 [01:38<10:43, 24.74s/it]

 eval loss: 0.332 | eval accuracy: 90.112%
lr: 0.0001
 train loss: 0.173 | train accuracy: 93.237%

 17%|█▋        | 5/30 [02:02<10:07, 24.28s/it]

 eval loss: 0.360 | eval accuracy: 90.224%
lr: 0.0001
 train loss: 0.153 | train accuracy: 94.496%

 20%|██        | 6/30 [02:25<09:32, 23.87s/it]

 eval loss: 0.262 | eval accuracy: 92.500%
lr: 0.0001
 train loss: 0.133 | train accuracy: 95.056%

 23%|██▎       | 7/30 [02:48<09:08, 23.85s/it]

 eval loss: 0.347 | eval accuracy: 90.321%
lr: 0.0001
 train loss: 0.116 | train accuracy: 95.522%

 27%|██▋       | 8/30 [03:13<08:48, 24.01s/it]

 eval loss: 0.196 | eval accuracy: 91.875%
lr: 0.0001
 train loss: 0.127 | train accuracy: 95.382%

 30%|███       | 9/30 [03:37<08:23, 23.99s/it]

 eval loss: 0.246 | eval accuracy: 92.404%
lr: 0.0001
 train loss: 0.097 | train accuracy: 96.409%

 33%|███▎      | 10/30 [04:01<07:58, 23.95s/it]

 eval loss: 0.207 | eval accuracy: 92.724%
lr: 0.0001
 train loss: 0.087 | train accuracy: 97.155%

 37%|███▋      | 11/30 [04:25<07:34, 23.93s/it]

 eval loss: 0.212 | eval accuracy: 93.542%
lr: 0.0001
 train loss: 0.070 | train accuracy: 97.341%

 40%|████      | 12/30 [04:49<07:11, 23.99s/it]

 eval loss: 0.210 | eval accuracy: 93.542%
lr: 0.0001
 train loss: 0.078 | train accuracy: 97.295%

 43%|████▎     | 13/30 [05:13<06:49, 24.11s/it]

 eval loss: 0.315 | eval accuracy: 93.558%
lr: 0.0001
 train loss: 0.060 | train accuracy: 97.668%

 47%|████▋     | 14/30 [05:37<06:23, 23.97s/it]

 eval loss: 0.265 | eval accuracy: 91.250%
lr: 5e-05
 train loss: 0.045 | train accuracy: 98.507%

 50%|█████     | 15/30 [06:00<05:56, 23.75s/it]

 eval loss: 0.278 | eval accuracy: 90.641%
lr: 5e-05
 train loss: 0.035 | train accuracy: 98.834%

 53%|█████▎    | 16/30 [06:24<05:32, 23.72s/it]

 eval loss: 0.221 | eval accuracy: 94.167%
lr: 5e-05
 train loss: 0.023 | train accuracy: 99.067%

 57%|█████▋    | 17/30 [06:48<05:09, 23.82s/it]

 eval loss: 0.329 | eval accuracy: 93.862%
lr: 5e-05
 train loss: 0.057 | train accuracy: 97.528%

 60%|██████    | 18/30 [07:11<04:45, 23.76s/it]

 eval loss: 0.204 | eval accuracy: 94.696%
lr: 5e-05
 train loss: 0.031 | train accuracy: 98.974%

 63%|██████▎   | 19/30 [07:34<04:19, 23.60s/it]

 eval loss: 0.221 | eval accuracy: 95.433%
lr: 5e-05
 train loss: 0.028 | train accuracy: 99.021%

 67%|██████▋   | 20/30 [07:59<03:57, 23.77s/it]

 eval loss: 0.169 | eval accuracy: 95.833%
lr: 5e-05
 train loss: 0.021 | train accuracy: 99.487%

 70%|███████   | 21/30 [08:23<03:35, 23.93s/it]

 eval loss: 0.598 | eval accuracy: 91.058%
lr: 5e-05
 train loss: 0.033 | train accuracy: 98.741%

 73%|███████▎  | 22/30 [08:47<03:11, 23.88s/it]

 eval loss: 0.160 | eval accuracy: 95.737%
lr: 5e-05
 train loss: 0.018 | train accuracy: 99.347%

 77%|███████▋  | 23/30 [09:11<02:48, 24.11s/it]

 eval loss: 0.183 | eval accuracy: 94.696%
lr: 5e-05
 train loss: 0.021 | train accuracy: 99.347%

 80%|████████  | 24/30 [09:36<02:25, 24.27s/it]

 eval loss: 0.543 | eval accuracy: 91.571%
lr: 5e-05
 train loss: 0.023 | train accuracy: 99.394%

 83%|████████▎ | 25/30 [09:59<02:00, 24.00s/it]

 eval loss: 0.163 | eval accuracy: 95.833%
lr: 5e-05
 train loss: 0.016 | train accuracy: 99.394%

 87%|████████▋ | 26/30 [10:23<01:35, 23.90s/it]

 eval loss: 0.247 | eval accuracy: 94.904%
lr: 5e-05
 train loss: 0.007 | train accuracy: 99.813%

 90%|█████████ | 27/30 [10:47<01:11, 23.92s/it]

 eval loss: 0.272 | eval accuracy: 95.737%
lr: 5e-05
 train loss: 0.041 | train accuracy: 98.554%

 93%|█████████▎| 28/30 [11:11<00:47, 23.82s/it]

 eval loss: 0.230 | eval accuracy: 94.487%
lr: 2.5e-05
 train loss: 0.014 | train accuracy: 99.627%

 97%|█████████▋| 29/30 [11:34<00:23, 23.75s/it]

 eval loss: 0.220 | eval accuracy: 94.904%
lr: 2.5e-05
 train loss: 0.015 | train accuracy: 99.580%

100%|██████████| 30/30 [11:58<00:00, 23.94s/it]

 eval loss: 0.186 | eval accuracy: 95.112%
lr: 2.5e-05
718.2316377609995
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