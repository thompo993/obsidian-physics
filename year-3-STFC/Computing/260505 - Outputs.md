# tags
[[260504 - Plan For Assignment 2 FINAL]]
[[pytorch]]

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