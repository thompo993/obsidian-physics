---
tags:
  - note
  - coding
  - machine-learning
created: 2026-05-14
---

## 1) High-level goal and transfer-learning strategy

You are taking a **pretrained ResNet-18** (trained on ImageNet) and adapting it to a **3‑class classification** task (“tumour classifier”). The key idea is **transfer learning**:

- **Early layers** of CNNs learn generic visual features (edges, textures).
- **Later layers** learn more task-specific combinations of features.
- Instead of training from scratch, you:
    1. Start from pretrained weights.
    2. Replace the final classification layer to output 3 classes.
    3. Train only a small subset of parameters (your new head + optionally some late backbone layers).

This typically converges faster, needs less data, and reduces overfitting risk.

---

## 2) Base architecture: ResNet-18 (what it is and why it’s a good choice)

### 2.1 What is ResNet?

**ResNet (Residual Network)** introduces **residual/skip connections** to make deep networks easier to optimize.

A residual block learns a function (F(x)), and the block output is:

y=F(x)+x

This helps because:

- Gradients can flow through the skip path even if (F(x)) becomes hard to optimize.
- It reduces vanishing gradient problems.
- It allows deeper networks to train reliably.

### 2.2 ResNet-18 specifically

ResNet-18 is a relatively small ResNet variant:

- It uses **basic residual blocks** (not bottleneck blocks).
- It’s much lighter than ResNet-50/101:
    - Fewer parameters
    - Faster training/inference
    - Often a good baseline for medical imaging when dataset sizes are modest.

### 2.3 Input and feature dimensions (important for understanding `.fc`)

For standard torchvision ResNet-18:

- Input image: **3 × 224 × 224**
- After convolutional stages, it produces a final feature tensor with **512 channels**
- Then it applies **global average pooling**, turning that into a **512‑dim feature vector per image**
- Finally `.fc` maps **512 → 1000** (ImageNet classes) in the original model

That’s why your new head begins with `nn.Linear(512, ...)`: the backbone outputs 512 features.

---

## 3) Loading pretrained weights and freezing (architecture + training choices)

Python

```
resnet18 = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)
for param in resnet18.parameters():
    param.requires_grad = False
```

### 3.1 Why use pretrained weights?

- They encode general-purpose features learned from large-scale ImageNet.
- In limited-data domains (common in medical datasets), this improves generalization.

### 3.2 Why freeze all layers initially?

Setting `requires_grad=False` means:

- No gradients are computed/stored for those parameters.
- They will not update during optimizer steps.
- This:
    - Speeds up training (less backprop work)
    - Reduces overfitting risk
    - Makes training more stable early on

This is the classic **feature extractor** setup: “use CNN as a fixed feature extractor, train a new classifier on top”.

---

## 4) Replacing the classifier head (`.fc`) and why it’s done this way

### 4.1 Original classifier

You print:

Python

```
print(resnet18.fc)
```

In ResNet-18, this is typically:

- `Linear(in_features=512, out_features=1000)` for ImageNet

### 4.2 Deep copy: why `copy.deepcopy(resnet18)`?

Python

```
resnet18_tc = copy.deepcopy(resnet18)
```

You do this to:

- Keep the original pretrained model object intact (for comparison or reuse).
- Avoid accidental in-place modifications.
- Make it semantically clear: **resnet18_tc** is your task-specific model.

### 4.3 Unfreezing the last residual layer (`layer4`)

Python

```
for param in resnet18_tc.layer4.parameters():
    param.requires_grad = True
```

This is **partial fine-tuning**:

- Earlier layers remain frozen (generic features).
- `layer4` is the last stage and is most task-specific in feature hierarchy.
- Fine-tuning `layer4` lets the network adapt high-level features to your dataset (e.g., tumour textures/shapes) without retraining the whole model.

Why “layer4” specifically?

- It’s close to the output and therefore most relevant to adapt to the new classification boundary.
- It usually gives a good tradeoff:
    - Better accuracy than training only the head
    - Less overfitting and compute than unfreezing all layers

### 4.4 Your new classifier head (architecture rationale)

Python

```
resnet18_tc.fc = nn.Sequential(
    nn.Linear(512, 256),
    nn.ReLU(),
    nn.Dropout(0.5),
    nn.Linear(256, 3)
)
```

Breakdown:

1. **`Linear(512 → 256)`**
    
    - Reduces the 512-d feature vector into a smaller hidden representation.
    - Adds capacity to learn task-specific combinations of features.
2. **`ReLU()`**
    
    - Introduces non-linearity.
    - Without this, stacked linear layers collapse into a single linear transform.
3. **`Dropout(0.5)`**
    
    - Regularization: randomly zeroes 50% of hidden units during training.
    - Especially useful when the head is trained on smaller datasets, reducing co-adaptation.
    - Helps prevent overfitting in the new classifier.
4. **`Linear(256 → 3)`**
    
    - Produces **logits** for your 3 classes.
    - Logits are unnormalized scores (not probabilities yet).

Important note for explanation:

- You do **not** apply `Softmax` in the model because **`nn.CrossEntropyLoss` expects raw logits** and internally applies `log_softmax` + `NLLLoss` in a numerically stable way.

### 4.5 Why save an “empty instance”?

Python

```
resenet_18_emtpy = copy.deepcopy(resnet18_tc)
```

This can be used as a clean template:

- to reload weights later without reconstructing architecture code
- to keep a reference to the architecture before training modifies weights

(You might later do `resenet_18_emtpy.load_state_dict(best_state)`.)

---

## 5) Loss function choice: weighted Cross Entropy (and why)

You compute class weights from `class_counts`:

Python

```
total = sum(class_counts)
n_classes = len(class_counts)
class_weights = torch.tensor([total / (n_classes * c) for c in class_counts])
loss_func = nn.CrossEntropyLoss(weight=class_weights)
```

### 5.1 Why CrossEntropyLoss?

For a multi-class single-label classification problem (3 classes), the standard loss is:

- Model outputs logits (z \in \mathbb{R}^3)
- Softmax gives probabilities
- Cross-entropy penalizes incorrect class probability

It’s the most common, well-behaved objective for this setting.

### 5.2 Why class weighting?

Medical datasets often have **class imbalance** (e.g., fewer malignant samples than normal). Without weighting:

- The model can achieve high accuracy by mostly predicting the majority class.
- Minority classes get under-learned.

Weighted cross entropy increases the penalty for misclassifying rare classes. Your formula:

wi=NK⋅ni

Where:

- (N) = total samples
- (K) = number of classes
- (n_i) = samples in class (i)

Interpretation:

- If (n_i) is small → (w_i) becomes larger → mistakes on that class cost more.

This encourages balanced performance (better recall on rare classes).

---

## 6) Optimizer choice: AdamW + parameter groups (and why)

Python

```
optimizer = torch.optim.AdamW([
    {'params': resnet18_tc.layer4.parameters(), 'lr': 1e-4},
    {'params': resnet18_tc.fc.parameters(),     'lr': 5e-4},
], weight_decay=0.005)
```

### 6.1 Why use different learning rates for backbone vs head?

Because the two parts are in very different situations:

- **`layer4` (pretrained)**: already good weights; you want _gentle_ adaptation  
    → smaller LR (1e‑4) to avoid destroying pretrained features (“catastrophic forgetting”).
    
- **`fc` head (newly initialized)**: random weights; needs to learn from scratch  
    → larger LR (5e‑4) to learn quickly.
    

This is a standard and strong fine-tuning practice.

### 6.2 Why AdamW?

AdamW is Adam with **decoupled weight decay**.

- In classic Adam, “L2 regularization” interacts with adaptive moments in a way that is not equivalent to true weight decay.
- AdamW applies weight decay more properly, often improving generalization.

This is especially useful in transfer learning where:

- You want stable optimization
- You want regularization (weight decay) without harming Adam’s adaptive behavior

### 6.3 Why weight decay?

`weight_decay=0.005` helps prevent overfitting by discouraging large weights.

- Particularly helpful for the new head (and any unfrozen layers) which might overfit quickly on small datasets.

_(Note: dropout + weight decay together give you two complementary regularizers.)_

---

## 7) Scheduler choice: ReduceLROnPlateau (and why it fits your loop)

Python

```
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(
    optimizer, patience=5, factor=0.5
)
...
scheduler.step(v_loss)
```

### 7.1 What does ReduceLROnPlateau do?

It monitors a metric (you use **validation loss**) and reduces LR when improvement stalls.

Mechanism:

- Track best `v_loss` seen so far
- If `v_loss` doesn’t improve for `patience=5` epochs:
    - multiply learning rate by `factor=0.5`

So, LR schedule is **performance-driven**, not time-driven.

### 7.2 Why is it a good choice here?

This scheduler is very practical when:

- You don’t know the optimal training length
- Validation performance plateaus unpredictably
- You want LR to drop when the model stops improving, which often helps it find a better minimum

It’s especially common in transfer learning setups.

### 7.3 Why step with validation loss (not training loss)?

Because you want LR changes based on **generalization**, not just fitting the training set.

- Training loss can keep decreasing even when val loss stagnates or worsens (overfitting).
- Using val loss makes LR reduction more aligned with improving real performance.

---

## 8) Multi-GPU training with DataParallel (what it does and caveats)

Python

```
if torch.cuda.device_count() > 1:
    resnet18_tc = nn.DataParallel(resnet18_tc)
    resnet18_tc = resnet18_tc.to(device)
```

### 8.1 What DataParallel does

`nn.DataParallel`:

- Splits each batch across GPUs (batch dimension)
- Replicates the model onto each GPU
- Computes forward/backward on each GPU
- Gathers gradients on the main GPU and updates parameters

This can speed up training when batches are large enough.

### 8.2 Caveat when saving/loading

When a model is wrapped in `DataParallel`, keys in `state_dict()` are prefixed with `"module."`.

So:

- Saving `resnet18_tc.state_dict()` while DataParallel-wrapped will include `"module."`
- Loading into a non-parallel model may require stripping that prefix.

(Your code saves `best_resnet_model_state = copy.deepcopy(resnet18_tc.state_dict())`; that’s fine, just be consistent when reloading.)

---

## 9) Training loop design (what’s happening each epoch)

Each epoch:

### 9.1 Training step

Python

```
t_loss, t_acc = step(... mode="train", optimizer=optimizer, freeze_bn_true_false=True)
```

Typical actions inside a train step:

- `model.train()`
- forward pass → logits
- compute loss (weighted cross entropy)
- backprop (`loss.backward()`)
- optimizer step (`optimizer.step()`)
- zero gradients (`optimizer.zero_grad()`)

You also pass `freeze_bn_true_false=True`. Likely meaning:

- BatchNorm layers are kept in eval mode or their running stats are frozen. Why that can matter:
- With small batches (or medical data with distribution shift), updating BN running stats can destabilize training.
- When fine-tuning only some layers, many people freeze BN to preserve pretrained normalization behavior.

### 9.2 Validation step

Python

```
v_loss, v_acc = step(... mode="eval")
```

Typical validation:

- `model.eval()`
- `torch.no_grad()`
- forward pass only
- compute metrics

### 9.3 Scheduler update

Python

```
scheduler.step(v_loss)
```

LR drops if validation loss stalls.

### 9.4 Best model tracking

Python

```
if v_acc > best_val_acc:
    best_val_acc = v_acc
    best_resnet_model_state = copy.deepcopy(resnet18_tc.state_dict())
```

This is model checkpointing-by-metric:

- You keep the parameters that achieved the best validation accuracy across training.
- Prevents you from ending up with the “last epoch” model if it overfits later.

---

## 10) Why these choices make sense together (a coherent “story”)

You can describe your approach as:

1. **Architecture choice (ResNet-18 pretrained)**  
    Selected for strong baseline performance, efficient compute, and proven transfer learning behavior.
    
2. **Transfer learning plan**
    
    - Freeze whole network first to preserve pretrained features.
    - Replace the final layer to match 3 classes.
    - Unfreeze only `layer4` to adapt high-level features without overfitting the entire backbone.
3. **Regularization choices (Dropout + weight decay + partial unfreezing)**
    
    - Dropout in head prevents co-adaptation.
    - Weight decay reduces overly large weights.
    - Only unfreezing `layer4` controls capacity and reduces risk on limited medical data.
4. **Loss choice (weighted cross entropy)**  
    Handles multi-class classification and combats class imbalance.
    
5. **Optimization (AdamW + differential learning rates)**
    
    - Head learns fast (higher LR)
    - Pretrained backbone adapts slowly (lower LR)
    - AdamW provides stable fine-tuning with correct weight decay handling.
6. **Scheduler (ReduceLROnPlateau)**  
    Automatically reduces LR when validation improvement stalls—often unlocking additional performance without manual tuning.
    

---

## 11) Suggested “talk track” (how to explain it verbally)

If you need a concise narrative:

- “We use ImageNet-pretrained ResNet-18 as the backbone because it provides strong general visual features and is lightweight enough for fast iteration. We replace the final fully connected layer with a small MLP head (512→256→3) with ReLU and dropout to learn task-specific decision boundaries for three tumour classes. To reduce overfitting and training time, we freeze the whole network, then selectively unfreeze only `layer4` so the highest-level features can adapt to our dataset. Because our dataset is imbalanced, we use weighted cross-entropy so minority classes contribute more to the loss. We optimize with AdamW using parameter groups: a smaller learning rate for the pretrained `layer4` and a higher learning rate for the new head. Finally, ReduceLROnPlateau monitors validation loss and halves the learning rate when improvement stalls, improving convergence without manual scheduling.”