---
tags:
  - note
  - coding
  - machine-learning
created: 2026-04-19
---
[[260411 - PyTorch Course Tracker]]
### Week 13 CNN example
class MNIST_CNN(nn.Module):

    def __init__(self):

        super().__init__()

        self.conv1 = nn.Conv2d(1, 32, kernel_size=3, padding=1)

        self.conv2 = nn.Conv2d(32, 64, kernel_size=3, padding=1)

        self.pool = nn.MaxPool2d(2, 2)

        self.relu = nn.ReLU()

        # Fix flattened size

        self.fc1 = nn.Linear(64 * 14 * 14, 128)

        self.fc2 = nn.Linear(128, 32)

        self.fc3 = nn.Linear(32, 10)

        self.softmax = nn.Softmax(dim=1)

        self.loss = nn.CrossEntropyLoss()

    def forward(self, xb):

        xb = self.relu(self.conv1(xb))

        xb = self.pool(self.relu(self.conv2(xb)))

        xb = xb.view(xb.size(0), -1)

        out = self.relu(self.fc1(xb))

        out = self.relu(self.fc2(out))

        out = self.fc3(out)

        return out

  

    def training_step(self, batch, device=None):

        x, y = batch

        if device is not None:

            x = x.to(device)

            y = y.to(device)

        y_hat = self(x)

        loss = self.loss(y_hat, y)

        return loss

  

    def predict_single(self, input, target, device=None):

        self.eval()

        with torch.no_grad():

            inputs = input.unsqueeze(0)

            if device is not None:

                inputs = inputs.to(device)

            predictions = self(inputs)

            probs = torch.softmax(predictions, dim=1)[0].detach().cpu()

        print('y:', int(target))

        print('y_hat (probs):', probs)

        print('predicted digit:', int(torch.argmax(probs)))
        ```

### Claude Basic Scaffold Example
```
import torch.nn as nn
import torch.nn.functional as F

class BaselineCNN(nn.Module):
    def __init__(self, num_classes=3):
        super().__init__()
        
        # Feature extractor — 3 conv blocks
        # Each block: Conv → ReLU → MaxPool
        self.features = nn.Sequential(
            # Block 1: 3 → 32 channels
            nn.Conv2d(3, 32, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),          # 224 → 112

            # Block 2: 32 → 64 channels
            nn.Conv2d(32, 64, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),          # 112 → 56

            # Block 3: 64 → 128 channels
            nn.Conv2d(128, 128, kernel_size=3, padding=1),  # ← spot the bug?
            nn.ReLU(),
            nn.MaxPool2d(2),          # 56 → 28
        )
        
        # Classifier head
        self.classifier = nn.Sequential(
            nn.Flatten(),
            nn.Linear(128 * 28 * 28, 256),
            nn.ReLU(),
            nn.Dropout(0.5),
            nn.Linear(256, num_classes)
        )
    
    def forward(self, x):
        x = self.features(x)
        x = self.classifier(x)
        return x

# Instantiate and move to device
model = BaselineCNN(num_classes=3).to(device)
print(model)

# Quick parameter count
total_params = sum(p.numel() for p in model.parameters() if p.requires_grad)
print(f"\nTrainable parameters: {total_params:,}")
```