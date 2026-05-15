[[pytorch]]
[[260411 - Brain Tumor Identification Plan]]

```
  


\section{Setup}

\subsection{Notebook setup code}

All results were run from a Jupyter Notebook hosted by Kaggle in order to utilise the 30 hours weekly GPU usage.  Two NVIDIA T4 GPUs, a single of which has been reported up to 35x faster training times \cite{2026_nvidia_t4_stats}. The notebook was setup to be semi-deterministic, as due to limited compute the balance between compute and reproducibility had to be struck, hence why stronger deterministic code such as \texttt{torch.use\_deterministic\_algorithms(True)} was not implemented results for the Jupyter notebook. The code then implements device agnostic code and number of workers, as this allows this notebook to work on any device, or number of GPUs.

  

\subsection{Exploratory Data Analysis}

The selected problem is brain tumour identification, which has 3064 T1-weighted contrast-enhanced images from 233 patients

\cite{2015_brain_tumour_dataset_cheng}. The aim of this experiment is to write both a basic baseline CNN, and also utilise a more powerful transfer learning model, so the data must be made compatible. The data was found to be split into 3 subdirectories, each representing a kind of tumours (meningioma=(1), glioma=(2), pituitary tumour=(3)).

\begin{figure}[htbp]

    \centering

    \includegraphics[width=1\linewidth]{fig-tumour-example-plots.png}

    \caption{An example of each of the three tumour types the model aims to classify.}

    \label{fig:tumour-example-plots}

\end{figure}

Next the characteristics for one example image from each tumour type was determined in order to better inform the preprocessing decisions later. the assumption is that all images within the same subdirectory retain the same characteristics. The all have the same characteristics being in RGBA mode, which stores images in red, green, blue and transparency channels. All images are the same 512x512, with the same minimum and maximum values. The folders were found to have different class weights with Meningioma, Glioma, and Pituitary, having 708, 1426, and 930 slices respectively:

  

\begin{wrapfigure}{r}{0.4\textwidth} % 'r' for right side

    \centering

    \includegraphics[width=0.38\textwidth]{fig-class_weights.png}

    \caption{Class weights for all tumour types}

    \label{fig:class_weights}

\end{wrapfigure}

  

\subsection{Data Preprocessing}

In order to implement a successful ResNet transfer learning model, appropriate transforms must be implemented, so the framework for the transforms are taken from the PyTorch documentation for setting up a ResNet transfer learning model \cite{2026_ResNetPyTorch}. ResNet takes 225x224 pixel size, and stores images in RGB, there are also a series of normalisations made to images so that they match what ResNet expects. Each colour channel has the mean subtracted in order to centre the mean at zero and is then divided by the standard deviation in order to prevent any particular colour channel from dominating.

(GET CLAUDE TO WRITE CODE THAT PLOTS SOME IMAGES POST TRANSFORM?, need to include steps and what of the transforms \textbf{are not} applied when viewing )

Aside from the base transforms in order to the dataset into the correct format for ResNet, some additional changes were made in order to reduce over fitting which was a significant problem the models encountered throughout the training and model refining process. \texttt{RandomHorizontalFlip()} flips the image in the horizontal axis, which can help prevent the model from remembering specific pixels, in addition to this \texttt{RandomRotation()} and \texttt{RandomErasing()}, which blacks out a region of the images was also implemented for the same purpose. For medial imaging, transforms must be implemented with care, as often they can loose their anatomical benefit if transformed too heavily, for example \texttt{ColorJitter()} and \texttt{GaussianBlur} can help with many computer vision projects, but changing the colour and clarity of the images can prevent the model from distinguishing between tumours and regular tissue. For the evaluation and test preprocessing, the images cannot be memorised, and are never used to train the data, so the above augmentations are removed.

\subsubsection{Data Loaders}

Once the data had been split into 70\% training, and 15\% each split for evaluation and testing, the Data Loaders were applied to each of the splits. The Batch size was initially chosen as 32, as this is the upper limit for small batch training for ResNet models \cite{batch_size_ml_masters}. This allows the GPUs availbile to maximised.
```

### Data loaders continued

Once the data had been split into 70\% training, and 15\% each split for evaluation and testing, the Data Loaders were applied to each of the splits. The Batch size was initially chosen as 32, as this is the upper limit for small batch training for ResNet models \cite{batch_size_ml_masters}. This allows the GPUs available to maximised. The training data loader is set to shuffle each batch, making it harder for the model to learn the ordering of the images.  The loaders are set to the correct number of workers and pins the memory, reducing the overhead of transferring from the  GPU to the CPU repeatedly if the GPU if available. 


## Functions
### Timer Function 
The timer function takes advantage of the \texttt{timeit} library in order to have a simple timer available where desired, was not required for the  training loop as `tqdm` has a built in timer which serves the purpose of tracking time per epoch. 

# Save Model
This functions purpose is to save functions for later analysis, it allows a customisable name, and the option to prevent overwriting the model with a time stamp if desired. It also has the key function of unwrapping the model from \texttt{DataParrallel()} such that it can be easily called for evaluation

**rename modle eval func to test func**

## Loop and Step Functions
These functions serve the ability to call a single function for both training and test steps within a training loop. The \texttt{step} function sets up device agnostic code, setups the context required for that particular stage of the step (for example \texttt{torch.inference_mode()}). The \texttt{loop} function then zeros the optimiser gradient if required, calculates batch loss and accuracy, and then backpropagates the loss and steps the optimizer accordingly, while printing and saving diagnostic information to track the models performance per epoch. 

## Stats Report 
This function is for determining all descriptive statistics and plots on each of the models after they have been trained to their respective optimal levels. It places the chosen model into inference mode, works out the predicted probabilities, predicted labels, and true labels, timing each batch throughout.
# Baseline CNN

### Model Architecture
The chosen model architecture for the baseline model was a smaller version of a Very Deep Convolutional Network \cite{simonyanVERYDEEPCONVOLUTIONAL2015. This model was chosen due to the high levels of success attributed to such models such that it was deemed a suitable baseline architecture to use as a framework. 

This model typically involves using many blocks that contain 3x3 Convolutional filters in order to learn the images. The architecture of this particular model involves 3 blocks, much less than a typical Very Deep Convolutional Network. Each block appliers two convolutional layers, which essentially "slide" across the image and identify patterns. After each convolutional layer, a the image goes through  Rectified Linear Unit (ReLU) layer. At the end of the block the image is passed through one Max Pooling layer, which reduces spatial and height dimensions by selecting the max from a 2x2 region of pixels. 

Each of the three layers increases in channels until the fourth block, which takes 128 channels then flattens, gets applied through one linear layer, one ReLU layer, before reaching the final classifier layer. Dropout randomly selects neurons to be  disabled at the end of each training loop, preventing any particular neuron from being memorised, subsequently helping prevent overfitting

### Loss and Optimizer function
The chosen loss formula was \texttt{CrossEntropyLoss()}, as this a widely used function considers that the prediction probabilities of the model output from SoftMax, and penalises incorrect guesses that had larger probabilities attributed to them more strongly. The chosen optimizer, was a adaptive moment estimate algorithm, Adam is a versatile optimizer geared towards machine learning problems with large datasets and high dimension parameter spaces, making it idea for computer vision problems. It has shown success in models containing similar architecture of convolutional layers, max pool and ReLU units (although in the cited paper by Kingma et al, 2014 use 5x5 convolutional filters and 3x3 max pooling, slightly larger than the baseline CNN)



### Resnet 18 
### Notes:
- resnet 50 overtrained rapidly 
- different lrs 
- treined cosine annealing 

One method of solving this issue is using transfer learning, where we can use millions of images images that a model has already been trained on and adjust the last few layers to tune the model to the tumor classification dataset. 
#### Architecture 

ResNet is a residual network that introduces skip connections to more easily optimize larger and deeper model. The purpose of ResNet was to solve the vanishing gradient problem, where some training gradients become extremely small, making it hard to optimise subsequent layers. ResNet utlises skip connections that bypass some connections within the model, which helps train the deeper layers more consistently.  

ResNet 18 uses residual blocks (**more on this**), and is relatively small compared to other ResNet models such as ResNet-50 or ResNet-101. (**add parameters here**) This gives the model fewer parameters and faster training and inference times. There is also the added benefit of the not overfitting as easily, as when experimenting using ResNet 50 as the transfer model, within 5 epochs it had already memorised the data, meaning the model is too large for the training dataset. 

For Resnet 18, the standard input image is identical to that defined above in section \ref{CORRECT SECTION HERE}. This allowed the same dataloaders to be used. Resnet 18 has XXXX layers, and to tune it to this specific model the classfier head was adjusted to accomadate this specific classifciation problem, with . . . 
### FIND ACADEMIC SOURCE 

