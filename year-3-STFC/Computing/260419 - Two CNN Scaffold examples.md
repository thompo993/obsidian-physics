
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

        print('predicted digit:', int(torch.argmax(probs)))```
```

### Claude Basic Scaffold Example
