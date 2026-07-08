---
tags:
  - note
created: 2026-05-08
---

- [x] Fully comment and clean up all code 📅 2026-05-15 ✅ 2026-06-02
# Tags
[[pytorch]]
[[260504 - Plan For Assignment 2 FINAL]]
# Computing Assignment 2 Brain Tumour Identification Report

## Abstract 
introduce resent, and the two models approximate architecture 
Results etc
## Exploratory Data Analysis 

- device agnostic code 
- reproducibility 
	- determinism vs compute speed 
- File Structure 
- Class Weights 
- plot some examples
- File data types:
	- size 
	- intensity 
	- type (RGBA)

## Pre-processing and Data loaders 

#### pre-processing
What does resent expect:
- `.Lambda`
	- converts images into RGB from RGBA
- `.Resize` and `.CenterCrop` 
- `.Normalize` 
how can we get the most out of our small dataset, training only
- `.RandomHorizontalFlip()`
- `.transforms.RandomRotation(10)`

**Split data 0.7, 0.15, 0.15** 

#### Data Loaders 
Batch size = 32, this was chosen because of the following paper 
[[batch_size_ml_masters]]

We chose shuffle = true for training, but we did not do this for validation and test loops, as this can create bias in comparison, and the model does not learn on the eval. so there is no risk of memorisation.

###  Model Functions
- timer: allows us to determine runtime, key metric considering limited compute
- highly functionated, as requested by examination board  

we set the mode, this determines if we are in evaluation mode or train mode for our model

for the `train` case: 
put into training mode
- zero the gradient 
- do the forward pass 
- calculate the loss 
- calculate accuracy 
- backpropagation 
- optimiser step

for the `test` case: 
put into evaluation mode
- do the forward pass 
- calculate the loss
- calculate the accuracy 
## Model 1 - Baseline CNN
https://stackoverflow.com/questions/52468956/how-do-i-visualize-a-net-in-pytorch 
visualise using this
better for generalisability (no data loader or transforms), easier to implemement

we copied the structure from this paper 
[[simonyanVERYDEEPCONVOLUTIONAL2015]]

## Model 2 - Resnet Transfer learning

## Evaluation