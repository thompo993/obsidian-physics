### Tags
[[machine learning]]
[[pytorch]]
[[python]]

### What is PyTorch? 
- most popular research deep learning framework
- write fast deep learning code in python (able to run on GPUs)
- able to access many pre-built deep learning models (`torchhub/torchvision.models`)
- whole stack: preprocess data, model data, deploy model 
- original deigned and use in house by Facebook, no open source. 
#### papers with code
papers with code is great online resource for papers, with code AI 
https://huggingface.co/papers/trending 

### why PyTorch? 
"With tools like colab, keras, and tensorflow, virtually anyone can solve in a day with no initial investment, is the same as what someone could do in a quarter with $20k in hardware
- open AI 
- meta 
- Microsoft
- many people use PyTorch
- **EVERYONE USES IT**

#### What is a GPU/TPU
- GPU- graphics processing unit
	- uses an interface called CUDA
		- parallel computing platform 
		- application programming interface 
		- general purpose processing
- TPU - tensor processing unit

### what is a Tensor?
A tensor is a algebraic object that describes a multilinear relationship between sets of algebraic objects associated with vector space. 

consider the anatomy of a neural network [[260411 - Pytorch for deep learning & Machine Learning - 00 - What is Machine Learning]]
![[fig-260420-anatomy-of-a-nn-24hr-pytorch.png]]

A Tensor seems like it is simply a matrix. 

`Torch.tensor` is very relevant to this whole course 

![[fig-260422-pytoch-what-is-a-tensor.png]]


### What is a [[tensor]]

## what are we going to cover 
- pytorch basics and fundamentals 
later
- preprocessing data (getting data into tensors)
- building and using pretrained deep learning models 
- fitting a model to the data  
- making predictions 
- evaluating model predictions 
- saving and loading models 
- using a trained models to make predictions on custom data
- ![[fig-260422-pytoch-workflow-what-is-pytorch.png]]



### how to approach this course 
- this course is focused on writing pytorch code 
- code along
	- so we will need to boot a lot of Kaggle notebooks 
- explore and experiment
- visualize what you don't understand
- ask questions (copilot could be used)
- do the exercises 
- share your work
### How to not approach this course 
- avoid overthinking the course 
	- don't say "i cant learn"

### learning pytorch resources 
GitHub repo: https://github.com/mrdbourke/pytorch-deep-learning
Q&A: abit late too this sadly, but use chat gpt or Claude
Course Online Book: https://www.learnpytorch.io/


code is written in vscode