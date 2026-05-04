### 🏷️ Tags

[[python]] 
[[machine learning]] 
[[pytorch]] 

---

### 🛠️ Phase 1: Project Setup, Hardware, & AI Attribution Strategy

_Since the brief explicitly states you must attribute AI-generated code, set this up right away. Furthermore, you want to maximize the use of your Kaggle Tesla T4 GPUs._

- **AI Attribution Standard:** Decide on a comment format to use throughout your notebook.
    - _Example:_ `# [AI-GEN] Prompt: "Write a PyTorch training loop with validation using tqdm" | Tool: GitHub Copilot`
- **Imports:** Gather all libraries at the top:
    - `torch`, `torchvision`, `torch.nn`, `torch.optim` (Core Deep Learning)
    - `sklearn.metrics`, `sklearn.model_selection` (Evaluation & Splitting)
    - `matplotlib.pyplot`, `seaborn` (Visualisation)
    - `tqdm` (For elegant progress bars during training loops)
    - `time` / `timeit` (To benchmark model training speeds)
- **Device & Hardware Configuration:**
    - You have 30 hours of dual **Tesla T4 GPUs** (15GB VRAM each).
    - Set up agnostic device code: `device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')`
    - _Bonus Marks:_ Since Kaggle provides two T4s, you can wrap your model in `torch.nn.DataParallel(model)` to utilize both GPUs simultaneously, drastically reducing training time.

### 📊 Phase 2: Exploratory Data Analysis (EDA)

_Do not skip this—it shows the marker you understand the data before blindly feeding it to a model._

- **Class Distribution Check:** Plot a bar chart showing the number of slices for each class (Meningioma: 708, Glioma: 1426, Pituitary: 930). _Note: Since Gliomas are roughly double the Meningiomas, you should note this imbalance. You may need to use class weights in your loss function later._
- **Visualise Samples:** Plot a 1x4 grid showing one random sample from each tumor class + healthy.
- **Shape Analysis:** Print the original dimensions of a few MRI images. This visually justifies your resizing step to the marker.

### ⚙️ Phase 3: Data Preprocessing & DataLoaders (Factorised Code)

_Write modular functions here so the code is reusable as requested._

- **Transformations (Matching ResNet expectations):**
    - As per PyTorch documentation, pre-trained models expect 3-channel RGB images loaded in a range of `[0, 1]` and normalized using `mean=[0.485, 0.456, 0.406]` and `std=[0.229, 0.224, 0.225]`.
    - **Train Transforms:** `Resize((224, 224))`, `RandomHorizontalFlip()`, `RandomRotation(10)` (Crucial for small datasets to prevent overfitting), `ToTensor()`, `Normalize(...)`.
    - **Val/Test Transforms:** Only `Resize((224,224))`, `ToTensor()`, and `Normalize(...)`. _(Never augment test data)._
    - **Grayscale to RGB:** Since MRI scans are grayscale, use PyTorch's `Lambda` transform to duplicate the 1 channel to 3, OR let `torchvision.datasets.ImageFolder` automatically load them as RGB.
- **Splitting the Data:** Stick to the **70/15/15** (Train/Val/Test) split using a seeded random split to ensure reproducibility.
- **DataLoaders:** Wrap your splits in PyTorch DataLoaders (`batch_size=32`).

### 🧠 Phase 4: Baseline CNN Architecture

_Create a simple, custom CNN from scratch. This serves as your benchmark to prove that Transfer Learning actually improves performance and justifies the added computational cost._

- **Architecture:** 2 or 3 Convolutional layers + MaxPooling + ReLU activations -> Flatten -> Fully Connected layer.
- **Factorisation:** Write it as a clean Python `nn.Module` class. Keep it lightweight.

### 🚀 Phase 5: Transfer Learning CNN (ResNet)

_Which ResNet should you use? For a dataset of ~3000 images, **ResNet18** is highly recommended. Models like ResNet50 or ResNet152 have too many parameters for this dataset size and will likely suffer from severe overfitting, while also wasting GPU time._

- **Model Selection:** Use PyTorch's modern API:
    
    Python
    
    ```
    from torchvision.models import resnet18, ResNet18_Weights
    model = resnet18(weights=ResNet18_Weights.DEFAULT)
    ```
    
- **Modifying the Classifier:**
    - Freeze the early feature-extraction layers so their pre-trained ImageNet weights aren't destroyed by random initial gradients.
    - Replace the final fully connected layer to output your specific classes: `model.fc = nn.Linear(model.fc.in_features, 4)` (assuming 4 classes: 3 tumors + healthy).
- **Loss Function:** `nn.CrossEntropyLoss()`. _(Pass `weight=` if your EDA showed severe imbalance)._
- **Optimizer:** Adam (`torch.optim.Adam`) with a smaller learning rate (e.g., `1e-4`) since the model is already pre-trained.

### 🔄 Phase 6: The Training Loop (with Timing & Progress)

_Write a single, reusable function: `def train_model(model, train_loader, val_loader, criterion, optimizer, epochs):`_

- **Integrating `tqdm`:** Wrap your train and val loaders in `tqdm` (`for batch in tqdm(train_loader, desc="Training"):`) to give a visual progress bar of batches processed.
- **Integrating `time`:** Record the start time before the epoch loop and the end time after. Print the total time taken for each epoch, and return the total overall training time.
- **Inside the loop:**
    1. **Train phase:** Forward pass, calculate loss, backward pass (`loss.backward()`), optimizer step.
    2. **Validation phase:** Forward pass with `torch.no_grad()`, calculate val loss and accuracy.
- **Model Checkpointing:** Save the model weights (`torch.save()`) whenever the validation loss reaches a new minimum.
- **Plotting:** Return the history of losses/accuracies and plot Train vs. Validation metrics over the epochs.

### 📈 Phase 7: Evaluation and Comparison (Quantitative Assessment)

_Evaluate BOTH models strictly on the 15% unseen **Test** set._

- **Time Efficiency Metrics:** Compare the total training time and inference time (seconds per batch) between your Baseline CNN and ResNet18. This utilizes your `timeit` implementation.
- **Classification Metrics:** Do not rely on Accuracy alone. Medical datasets require:
    - **Precision & Recall:** (Recall is critical for tumours—false negatives could cost a life).
    - **F1-Score:** Balances precision and recall.
    - Use `sklearn.metrics.classification_report` to print these beautifully.
- **Visual Assessment:** Generate a **Confusion Matrix** for both models (`sklearn.metrics.confusion_matrix` + `seaborn.heatmap`). This visually proves to the examiner exactly which tumors the models are confusing with each other.

---

### 📝 Plan for the Report (Max 2500 words)

**1. Introduction & Methodology**

- Briefly introduce the problem (brain tumour classification from MRIs).
- Discuss the dataset, referencing your EDA (e.g., class imbalances).
- Detail your data preprocessing: Why 224x224? Why ImageNet normalization? Why data augmentation (rotation/flips)?

**2. Model Architectures & Approach**

- **Baseline CNN:** Explain the architecture as your control variable.
- **Transfer Learning:** Explain why you chose ResNet18. Mention that while Kaggle provided dual Tesla T4s capable of running ResNet152, you chose ResNet18 to avoid overfitting on a small (~3000 image) dataset. Explain how you adapted the final fully connected layer.
- **Training choices:** Justify your loss function (CrossEntropy) and hyperparameters (batch size 32, Adam optimizer).

**3. Results & Evaluation**

- Present your Training/Validation Loss graphs and Confusion Matrices.
- **Evaluation Metrics:** Explain why you used Recall/F1-Score in a medical context instead of just raw accuracy.
- **Comparative Analysis:** Compare the Baseline vs ResNet results, not just in accuracy, but in **computational cost** (referencing your `time` metrics—e.g., "ResNet took 4x longer to train but yielded a 20% increase in Recall").

**4. Code Reusability & Validation**

- Briefly mention how you structured your code (modular DataLoaders, generic training loops, use of `tqdm` for UI) to make it reusable. Emphasize that hyperparameters were tuned on a Validation set, and final metrics were derived from an unseen Test set.

**5. Use of Generative AI (Mandatory Section)**

- List tools (e.g., ChatGPT, GitHub Copilot).
- Describe the approach: "AI was used to generate boilerplate PyTorch code for the training loop (including `tqdm` integration), matplotlib visualization, and hardware device assignment. All AI-generated code blocks are explicitly tagged in the notebook comments with the prompts used. The overall architecture choices, data-splits, and comparative analysis logic were designed manually."