## progress Table

| Link and Section                                                                                                       | Date     | Note      |
| ---------------------------------------------------------------------------------------------------------------------- | -------- | --------- |
| [0:32:24](https://www.youtube.com/watch?v=V_xro1bcAuA&t=1944s) 5. Different learning paradigms                         | 20/04/26 | Obosidian |
| [2:03:26](https://www.youtube.com/watch?v=V_xro1bcAuA&t=7406s) 18. Tensor attributes (information about tensors)       | 22/04/26 | kaggle    |
| 2:23:00 Mat Multiplication                                                                                             | 23/04/26 | VSCODE    |
| 2:57:48 25. Reshaping, viewing and stacking                                                                            | 23/04/26 | VSODE     |
| [5:41:15](https://www.youtube.com/watch?v=V_xro1bcAuA&t=20475s) 43. Training a model with PyTorch (intuition building) | 25/04/27 | Kaggle    |
| [8:32:00](https://www.youtube.com/watch?v=V_xro1bcAuA&t=30720s) 60. Introduction to machine learning classification    | 26/04/27 | Kaggle    |
|                                                                                                                        |          |           |


## Link
https://www.youtube.com/watch?v=V_xro1bcAuA&t=1070s

### Tags
[[machine learning]]
[[pytorch]]
[[python]]
Machine Learning is turning things (data) into numbers and finding patterns in the numbers. 

### Machine Learning vs. Deep Learning
Artificial intelligence 
- machine learning
	- deep learning

why would you want to use machine learning or deep learning? 
**why not?**
for a complex problem, can you think of all the rules! 
consider driving- very complex 

--- 
### what is deep learning good for? 
- problems with long lists of rules
- when trad approach fails 
- continually changing environments- deep learning can adapt "learn" new scenarios 
- very large datasets
	- can you imagine trying to make rules for 101 foods, better to train ml than have these rules 

---
### what is deep learning not good for? 
- when you need explain ability- pattens learned by a deep learning model are uninterpretable by a human 
- when the traditional rule approach is a good option 
- when errors are unacceptable
	- since outputs of deep learning models aren't always predictable. 
- when you don't have much data 

--- 
### machine learning vs deep learning 
- traditional machine learning 
	- used on structured data 
	- "rows and columns"
	- we like to use a gradient boosted machine eg XGBOOST

	- machine learning types include 
	- random forest 
	- gradient boosted models 
	- naive bayes 
	- nearest neighbor 
	- support vector machine 

	- deep learning types include 
	- neural network 
	- fully connected neural networks 
	-  convolutional neutral networks 
	- recurrent neural network 
	- transformer

- deep learning 
	- unstructured data 
	- if you had loads of text, unstructured 
	- images 
	- voices- voice assistant 
	- typically, we use a neural network

--- 
### what are neural networks. 
**my definition:** a neural network  is a computational model inspired by the structure and functions of biological neural networks, such as those that are in the brain. 

- 3b1b recommended for learning about NN 

## Daniel Bourke's definition 
before data gets used with a neural network, it needs to be turned into numbers
$\longrightarrow$
we then pass our numbers through our neural network, this is numerical encoding 
$\longrightarrow$

--- 
choose the appropriate neural network for your problem 
inputs $\longrightarrow$ numerical encoding $\longrightarrow$ learns representation (patterns/features/weights) $\longrightarrow$ represents outputs $\longrightarrow$ outputs 

--- 

### anatomy of a neural networks 

![[fig-260420-anatomy-of-a-nn-24hr-pytorch.png]]
- mention of ResNet, common for computer vision  

--- 

### types of learning

Supervised learning 
- labels

Unsupervised and self-supervised learning 
- no labels 
- extracts patterns 

transfer learning
- very important paradigm 
- take patterns one model has learned ---> transfer it to another model 
- "head start" for your model

### Q: What is deep learning actually used for? 
- computer vision 
- self driving cars
- facial recognition 
- NLP 
- Medical diagnostics
	- medical imaging of brain tumors THIS IS US
- Sequence2Sequence (Seq2Seq)
- Classification/regression
