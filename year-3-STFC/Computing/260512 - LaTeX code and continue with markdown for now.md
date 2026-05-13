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

Once the data had been split into 70\% training, and 15\% each split for evaluation and testing, the Data Loaders were applied to each of the splits. The Batch size was initially chosen as 32, as this is the upper limit for small batch training for ResNet models \cite{batch_size_ml_masters}. This allows the GPUs available to maximised. 