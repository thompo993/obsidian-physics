---
tags:
  - note
  - coding
  - machine-learning
created: 2026-05-16
---

this is to be used in [[260504 - Plan For Assignment 2 FINAL]]
**ALL FIGURES STORED TOO:** "C:\Physics\y3\ml_assignment_2"

# Baseline CNN All outputs:
### Training and Eval Loop
```
----------------------------------------
Base Line CNN
----------------------------------------
average time per batch: 1.97659 micro seconds | std deviation of 0.36720 micro seconds
----------------------------------------
              precision    recall  f1-score   support

  Meningioma     0.5263    0.7368    0.6140        95
      Glioma     0.8736    0.7294    0.7950       218
   Pituitary     0.9097    0.8973    0.9034       146

    accuracy                         0.7843       459
   macro avg     0.7699    0.7878    0.7708       459
weighted avg     0.8132    0.7843    0.7920       459
```
### Stats Report 



# ResNET All outputs:

### Training and Eval Loop
```
0%|          | 0/30 [00:00<?, ?it/s]

 train loss: 0.574 | train accuracy: 76.026%

  3%|▎         | 1/30 [00:21<10:16, 21.28s/it]

 eval loss: 0.350 | eval accuracy: 85.224%
lr: 0.0001
 train loss: 0.339 | train accuracy: 86.987%

  7%|▋         | 2/30 [00:42<09:58, 21.37s/it]

 eval loss: 0.365 | eval accuracy: 87.420%
lr: 0.0001
 train loss: 0.260 | train accuracy: 89.925%

 10%|█         | 3/30 [01:04<09:40, 21.49s/it]

 eval loss: 0.295 | eval accuracy: 89.167%
lr: 0.0001
 train loss: 0.242 | train accuracy: 91.185%

 13%|█▎        | 4/30 [01:26<09:22, 21.65s/it]

 eval loss: 0.311 | eval accuracy: 90.753%
lr: 0.0001
 train loss: 0.179 | train accuracy: 93.517%

 17%|█▋        | 5/30 [01:48<09:02, 21.71s/it]

 eval loss: 0.474 | eval accuracy: 89.087%
lr: 0.0001
 train loss: 0.216 | train accuracy: 91.931%

 20%|██        | 6/30 [02:10<08:43, 21.80s/it]

 eval loss: 0.280 | eval accuracy: 91.362%
lr: 0.0001
 train loss: 0.141 | train accuracy: 94.356%

 23%|██▎       | 7/30 [02:31<08:20, 21.76s/it]

 eval loss: 0.216 | eval accuracy: 91.058%
lr: 0.0001
 train loss: 0.131 | train accuracy: 94.823%

 27%|██▋       | 8/30 [02:53<07:59, 21.80s/it]

 eval loss: 0.244 | eval accuracy: 93.766%
lr: 0.0001
 train loss: 0.087 | train accuracy: 96.642%

 30%|███       | 9/30 [03:15<07:39, 21.87s/it]

 eval loss: 0.321 | eval accuracy: 91.875%
lr: 0.0001
 train loss: 0.113 | train accuracy: 95.336%

 33%|███▎      | 10/30 [03:37<07:17, 21.87s/it]

 eval loss: 0.211 | eval accuracy: 92.212%
lr: 0.0001
 train loss: 0.096 | train accuracy: 96.362%

 37%|███▋      | 11/30 [03:58<06:52, 21.69s/it]

 eval loss: 0.222 | eval accuracy: 94.808%
lr: 0.0001
 train loss: 0.062 | train accuracy: 97.481%

 40%|████      | 12/30 [04:20<06:30, 21.68s/it]

 eval loss: 0.197 | eval accuracy: 95.529%
lr: 0.0001
 train loss: 0.057 | train accuracy: 97.948%

 43%|████▎     | 13/30 [04:42<06:08, 21.68s/it]

 eval loss: 0.139 | eval accuracy: 95.224%
lr: 0.0001
 train loss: 0.070 | train accuracy: 97.295%

 47%|████▋     | 14/30 [05:03<05:46, 21.64s/it]

 eval loss: 0.263 | eval accuracy: 93.349%
lr: 0.0001
 train loss: 0.059 | train accuracy: 97.575%

 50%|█████     | 15/30 [05:24<05:22, 21.50s/it]

 eval loss: 0.182 | eval accuracy: 94.183%
lr: 0.0001
 train loss: 0.039 | train accuracy: 98.881%

 53%|█████▎    | 16/30 [05:45<04:59, 21.40s/it]

 eval loss: 0.226 | eval accuracy: 93.654%
lr: 0.0001
 train loss: 0.044 | train accuracy: 98.368%

 57%|█████▋    | 17/30 [06:07<04:38, 21.39s/it]

 eval loss: 0.393 | eval accuracy: 92.003%
lr: 0.0001
 train loss: 0.032 | train accuracy: 98.834%

 60%|██████    | 18/30 [06:29<04:18, 21.57s/it]

 eval loss: 0.412 | eval accuracy: 93.045%
lr: 0.0001
 train loss: 0.126 | train accuracy: 95.709%

 63%|██████▎   | 19/30 [06:51<03:58, 21.68s/it]

 eval loss: 0.152 | eval accuracy: 94.279%
lr: 5e-05
 train loss: 0.028 | train accuracy: 99.067%

 67%|██████▋   | 20/30 [07:12<03:36, 21.68s/it]

 eval loss: 0.178 | eval accuracy: 95.833%
lr: 5e-05
 train loss: 0.021 | train accuracy: 99.534%

 70%|███████   | 21/30 [07:34<03:14, 21.66s/it]

 eval loss: 0.207 | eval accuracy: 95.016%
lr: 5e-05
 train loss: 0.018 | train accuracy: 99.207%

 73%|███████▎  | 22/30 [07:56<02:54, 21.81s/it]

 eval loss: 0.206 | eval accuracy: 95.224%
lr: 5e-05
 train loss: 0.024 | train accuracy: 99.160%

 77%|███████▋  | 23/30 [08:18<02:32, 21.85s/it]

 eval loss: 0.154 | eval accuracy: 95.737%
lr: 5e-05
 train loss: 0.027 | train accuracy: 99.067%

 80%|████████  | 24/30 [08:40<02:11, 21.93s/it]

 eval loss: 0.157 | eval accuracy: 96.362%
lr: 5e-05
 train loss: 0.017 | train accuracy: 99.347%

 83%|████████▎ | 25/30 [09:02<01:49, 21.98s/it]

 eval loss: 0.189 | eval accuracy: 96.154%
lr: 2.5e-05
 train loss: 0.018 | train accuracy: 99.300%

 87%|████████▋ | 26/30 [09:24<01:27, 21.88s/it]

 eval loss: 0.176 | eval accuracy: 96.779%
lr: 2.5e-05
 train loss: 0.009 | train accuracy: 99.767%

 90%|█████████ | 27/30 [09:46<01:05, 21.84s/it]

 eval loss: 0.178 | eval accuracy: 95.849%
lr: 2.5e-05
 train loss: 0.015 | train accuracy: 99.674%

 93%|█████████▎| 28/30 [10:07<00:43, 21.78s/it]

 eval loss: 0.165 | eval accuracy: 96.042%
lr: 2.5e-05
 train loss: 0.010 | train accuracy: 99.580%

 97%|█████████▋| 29/30 [10:29<00:21, 21.70s/it]

 eval loss: 0.194 | eval accuracy: 95.946%
lr: 2.5e-05
 train loss: 0.007 | train accuracy: 99.627%

100%|██████████| 30/30 [10:51<00:00, 21.70s/it]

 eval loss: 0.216 | eval accuracy: 95.529%
lr: 2.5e-05
651.0740678530001
```
### Stats Report 
```
----------------------------------------
ResNet 18
----------------------------------------
average time per batch: 1.04249 micro seconds | std deviation of 0.23203 micro seconds
----------------------------------------
              precision    recall  f1-score   support

  Meningioma     0.9667    0.9158    0.9158        95
      Glioma     0.9817    0.9817    0.9817       218
   Pituitary     0.9669    1.0000    0.9832       146

    accuracy                         0.9739       459
   macro avg     0.9717    0.9658    0.9685       459
weighted avg     0.9739    0.9739    0.9736       459
```
