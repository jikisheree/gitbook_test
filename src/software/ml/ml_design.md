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
- form Sound wave we can Extraction Features with by splitting the audio into short segments to obtain a sequence of frequency spectra.  each covering only a small slice of time, and stack the resulting spectra together into a spectrogram. 
- by this spectrogram plots the frequency content of an audio signal as it changes over time.
 
*<p align="center"><img height ="600px" class="center-block" src="../../images/ml/wave_to_mfcc.png"></p>*
-  Mel frequency cepstrum 
-  Mel frequency cepstrum coefficient

**Features Extraction form Cat sound**
 
![](../../images/ml/dataset.png)

## Model Classification Architecture
- Considered as the back bone of Classification
The architecture used in the Model is **Convolutional Neural Network**, with receive  **Mel frequency cepstrum coefficient**, in Dimension **160 * 120**, and **Convolution , pooling**  and **Fully Connected weight** for Classification by 3 output node.

![Convolutional Neural Network model](../../images/ml/model.png)
*<p style="text-align: center;">Convolutional Neural Network model</p>*
-  with this Convolutional Neural Network model it have weight (params) total 82,972,163 parameter
    - all parameter are trainable
    
**Traing process**
 
*<p align="center"><img width  ="800px" class="center-block" src="../../images/ml/train.png"></p>*
with  loss: 1.0762 and accuracy: 0.7020
- Accuracy of our model on test data :  70.20202279090881 %

**Accuracy **
- Works as the bidge of Access Layer and Core Layer

*<p align="center"><img width  ="800px" class="center-block" src="../../images/ml/martix.png"></p>*
- martrix

*<p align="center"><img width  ="800px" class="center-block" src="../../images/ml/recall.png"></p>*
- recall 