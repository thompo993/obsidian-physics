### Tags 
[[python]]
[[machine learning]]
[[pytorch]]
### Phase 1: Project Setup & AI Attribution Strategy

_Since the brief explicitly states you must attribute AI generated code, set this up right away._

- **AI Attribution Standard:** Decide on a comment format to use throughout your notebook.
    - _Example:_ `# [AI-GEN] Prompt: "Write a PyTorch training loop with validation" | Tool: GitHub Copilot`
- **Imports:** Gather all libraries at the top (`torch`, `torchvision`, `sklearn`, `matplotlib`, `numpy`, `pandas`).
- **Device Configuration:** Set up agnostic device code (`device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')`) to utilize your Kaggle Tesla T4s.
- **TQDM** for progress bar 
- **Timeit** for timing the training of the models

### Phase 2: Exploratory Data Analysis (EDA)

_Do not skip this—it shows the marker you understand the data before blindly feeding it to a model._

- **Class Distribution Check:** Plot a bar chart showing the number of slices for each class (Meningioma: 708, Glioma: 1426, Pituitary: 930, Healthy: X). _Note: Since Gliomas are roughly double the Meningiomas, you might need class weights later._
- **Visualise Samples:** Plot a 1x4 grid showing one random sample from each class.
- **Shape Analysis:** Print the original dimensions of a few images to justify your resizing step.

###  Phase 3: Data Preprocessing & DataLoaders (Factorised Code)

_Write modular functions here so the code is reusable as requested._

- **Transformations:**
    - **Train Transforms:** `Resize((224, 224))`, `RandomHorizontalFlip()`, `RandomRotation(10)` (Data augmentation is crucial for small datasets like this to prevent overfitting), `ToTensor()`, `Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])`.
    - **Val/Test Transforms:** Only `Resize((224,224))`, `ToTensor()`, and `Normalize`. (Never augment test data).
    - **Grayscale to RGB:** Use PyTorch's `Lambda` transform to duplicate channels if reading as grayscale, or simply use `ImageFolder` which can automatically load images as RGB.
- **Splitting the Data:**
    - Stick to the **70/15/15** (Train/Val/Test) split. A separate validation set is vital for tuning hyperparameters (like learning rate) without leaking data from the final test set.
- **DataLoaders:** Wrap your splits in PyTorch DataLoaders (batch_size=32).

###  Phase 4: Baseline CNN Architecture
Mon May  4 11:16:32 2026       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.105.08             Driver Version: 580.105.08     CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla T4                       Off |   00000000:00:04.0 Off |                    0 |
| N/A   60C    P0             28W /   70W |     189MiB /  15360MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
|   1  Tesla T4                       Off |   00000000:00:05.0 Off |                    0 |
| N/A   32C    P8             10W /   70W |       3MiB /  15360MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A              57      C   /usr/bin/python3                        186MiB |
+-----------------------------------------------------------------------------------------+
_Create a simple, custom CNN from scratch. This serves as your benchmark to prove that Transfer Learning actually helps._

- **Architecture:** 2 or 3 Convolutional layers + MaxPooling + ReLU activations -> Flatten -> Fully Connected layer.
- **Factorisation:** Write it as a clean Python `nn.Module` class.

###  Phase 5: Transfer Learning CNN (ResNet)

- **Model Selection:** `torchvision.models.resnet18(pretrained=True)` or `resnet50`. ResNet18 is faster and usually sufficient for 224x224 images.
- **Modifying the Classifier:** Freeze the base layers (optional, but good practice for the first few epochs), and replace the final fully connected layer (`model.fc = nn.Linear(model.fc.in_features, 4)` for your 4 classes).
- **Loss Function:** `nn.CrossEntropyLoss()`. _If your EDA showed severe imbalance, pass `weight=` to this loss function to penalize misclassifications of minority classes._
- **Optimizer:** Adam (`torch.optim.Adam`) with a learning rate of ~0.001.

###  Phase 6: The Training Loop

_Write a single, reusable function: `def train_model(model, train_loader, val_loader, criterion, optimizer, epochs):`_

- **Inside the loop:**
    1. Train phase (forward pass, loss, backward pass, step).
    2. Validation phase (forward pass with `torch.no_grad()`, calculate val loss and accuracy).
- **Intermediate Validation:** Save the model weights (`torch.save()`) when the validation loss reaches a new minimum (Model Checkpointing). This prevents overfitting.
- **Plotting:** Output a graph of Train Loss vs. Validation Loss over the epochs.

###  Phase 7: Evaluation and Comparison (Quantitative Assessment)

_Evaluate BOTH models strictly on the 15% unseen **Test** set._

- **Metrics:** Do not rely on Accuracy alone. Medical datasets require:
    - **Precision & Recall** (Recall is critical for tumours—false negatives are dangerous).
    - **F1-Score**.
    - Use `sklearn.metrics.classification_report`.
- **Visual Assessment:** Generate a **Confusion Matrix** for both models (`sklearn.metrics.confusion_matrix` + `seaborn.heatmap`). This visually proves to the examiner exactly which tumors the model is confusing with each other.

---

### Plan for the Report (Max 2500 words)

Based on the assignment prompt, here is an outline for your report:

**1. Introduction & Methodology**

- Briefly introduce the problem (brain tumour classification).
- Discuss the dataset and any class imbalances.
- Detail your data preprocessing: Why 224x224? Why ImageNet normalization? Why data augmentation?

**2. Model Architectures & Approach**

- **Baseline CNN:** Explain the architecture. "What class of ML model and why?" -> CNNs capture spatial hierarchies in image data.
- **Transfer Learning:** Explain why you chose ResNet. Explain that it leverages feature extraction learned from millions of images, which helps combat the small size of the medical dataset.
- **Training choices:** "What loss function and why?" -> CrossEntropy because it's a mutually exclusive multi-class problem. "Hyperparameters?" -> Justify batch size 32 (memory vs gradient stability) and learning rate.

**3. Results & Evaluation**

- Present your graphs (Loss curves) and Confusion Matrices.
- "What methods did you use to evaluate and why?" -> Explain why you used Recall/F1-Score in a medical context instead of just raw accuracy.
- Compare the Baseline vs ResNet results.

**4. Code Reusability & Validation**

- Briefly mention how you structured your code (e.g., modular DataLoaders, generic training loops) to make it reusable. Mention using a held-out test set to validate unseen performance.

**5. Use of Generative AI (Mandatory Section)**

- List tools (e.g., ChatGPT, GitHub Copilot).
- Describe the approach: "AI was used to generate boilerplate PyTorch code for the training loop and matplotlib visualization. AI prompts are documented in the notebook comments. The overall architecture choices and data-splits were designed manually."