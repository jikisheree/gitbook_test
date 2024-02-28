# MACHINE LEARNING DESIGN
Overview of  machine learning and training model frame work.

<!-- <div class="warning">

**Section**

- [ 
-  

</div> -->

## Data set


**kaggle.com**
- https://www.kaggle.com/code/gleblevankov/meow-meow
- This dataset, composed of 440 sound recordings, contains meows emitted by cats in different contexts. Specifically, 21 cats belonging to 2 breeds (Maine Coon and European Shorthair) have been repeatedly exposed to three different stimuli that act as labels for prediction:

**Cat sound**
 ![](../../images/ml/es_hungry.png)
 
- Example Sound of Hungry cat.
 
<p style="text-align: center;">Sound wave hungry</p> 
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/es_hungry.png"></p>

<p style="text-align: center;">  Mel frequency cepstrum coefficient</p>
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/sp_hungry.png"></p>
 
 

- Example Sound of Isolated cat.

<p style="text-align: center;">Sound wave Isolated</p> 
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/es_isolated.png"></p>

<p style="text-align: center;">  Mel frequency cepstrum Isolated</p>
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/sp_isolated.png"></p>
 

- Example Sound of Happy cat.
<p style="text-align: center;">Sound wave Happy</p> 
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/es_happy.png"></p>

<p style="text-align: center;">  Mel frequency cepstrum Happy</p>
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/sp_happy.png"></p>
 


**Features Extraction form Cat sound**
- We can extract features from sound wave by splitting the audio into short segments to obtain a sequence of frequency spectra, each covering only a small slice of time, and stack the resulting spectra together into a spectrogram.
- This spectrogram plots the frequency content of an audio signal as it changes over time.
 
*<p align="center"><img height ="600px" class="center-block" src="../../images/ml/wave_to_mfcc.png"></p>*
-  Mel frequency cepstrum 
-  Mel frequency cepstrum coefficient

**Features Extraction from Cat sound**
 
![](../../images/ml/dataset.png)

## Model Classification Architecture
- Considered as the back bone of Classification
The architecture used in the Model is **Convolutional Neural Network**, with receive  **Mel frequency cepstrum coefficient**,for input Dimension **160 * 120**, and feedforward to **Convolution , pooling**  and **Fully Connected weight** layer for Classification by 3 output node.

![Convolutional Neural Network model](../../images/ml/model.png)
*<p style="text-align: center;">Convolutional Neural Network model</p>*
-  with  Convolutional Neural Network model it has weight (params) total 82,972,163 parameter
    - all parameter are trainable
    
**Traing process**
 
*<p align="center"><img width  ="800px" class="center-block" src="../../images/ml/train.png"></p>*
with  loss: 1.0762 and accuracy: 0.7020
- Accuracy of our model on test data :  70.20202279090881 %

**Accuracy **
 

*<p align="center"><img width  ="800px" class="center-block" src="../../images/ml/martix.png"></p>*
- Confusion matrix is a table that is used to define the performance of a classification algorithm. A confusion matrix visualizes and summarizes the performance of a classification algorithm.


- Frome confusion matrix we can calculated 
    - Accuracy = (TPs + TNs) / (TPs+TNs+FPs + FNs)
    - Precision = TPs / (TPs + FPs)
    - Recall = TPs/(TPs+FNs)
    - F1 = 2 x (Precision x Recall)/(Precision + Recall)
*<p align="center"><img width  ="800px" class="center-block" src="../../images/ml/recall.png"></p>*
 