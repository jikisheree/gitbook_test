# Machine Learning Pipeline Design
Overview of  machine learning and training model frame work.

 

## Pipeline flow
 

- For our project we make Machine Learning in Amazon Web Services (AWS) for  Create , Training , Deploy , re-Traing.

**Overall Pipeline**
 ![](../../images/ml/Pipeline.png)

 

**Training Process**
 
 
<p style="text-align: center;">Build model</p> 
<p align="center"><img  class="center-block" src="../../images/ml/Pipeline_1.png"></p>
 

<!-- 1. สร้าง Project และทำการสร้าง โครงสร้างของ model ใน AWS  และดึงข้อมูล Training Data set ที่เก็บไว้ใน S3 เพื่อทำการ CodeBuild model ขึ้นมา  -->

1. Creat Project and build sturcture for model in AWS then Commit Code to AWS Load Training Data set in Amazon s3 with codeBuild to Start Pipeline

<!-- 2.  Start Pipline and train Model ใน Sagemaker Pipeline ที่ run อยู่ใน ml.p3.8xlarge ซึงมี GPU 4 ตัวในการ traing model โดยจะใช้เวลาประมาณ 24hr ในการ train
- โดยจะมี model อยู่ 2 ตัวทำการ train อยู่ใน Sagemaker คือ model ตัวเก่า และ ตัวใหม๋ -->
 
2.  Start Pipline and train Model in Sagemaker Pipeline that run on instance   ml.p3.8xlarge with 4 GPU  NVIDIA V100 with 16 GB/GPU ram in Total 64 GB vram to traing model estimated use 24hr to train

<!-- - โดยจะมี model อยู่ 2 ตัวทำการ train อยู่ใน Sagemaker คือ model ตัวเก่า และ ตัวใหม๋ -->
- Sagemaker will have 2 model that will train  : old model and new ground truth model 

<!-- 3. จะได้ Registry Amazon SageMaker Model ที่ train เสร็จแล้ว -->

3. we will get Registry of Amazon SageMaker Model that are trained




**DEPLOY Process**
 

<p style="text-align: center;"></p> 
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/Pipeline_2.png"></p>
 
<!-- 4. ทำการ เลือก และ Approve model ที่่ความแม่นยำที่สูง หรือ พอรับได้ commit และ build เพื่อที่จะทำ model ตัวนี้ไป deploy  -->

 4. we will choose model and Approve  with high or acceptable accuracy then commit and build  for deploy 

<!-- 5.  ทำการ deploy ลง  Deploy models for inference endpoint  โดยใช้ instance ml.g4dn.xlarge ซึ่งมี GPU 1 ตัว ในการ run model ตลอด 24hr -->

5. Deploy to   Amazon SageMaker for inference endpoint  with use  instance ml.g4dn.xlarge  with 1 GPU  NVIDIA V100 with 16 GB GPU RAM  for runing inference endpoint 24hr

6. SageMaker endpoint will have Monitoring model every week or day due to our setting to monitor  accuracy of prediction
 




<p style="text-align: center;">   </p>
<p align="center"><img width ="600px"  class="center-block" src="../../images/ml/Pipeline_gt.png"></p>
ground truth

<!-- - หากสิ้งที่ model ทำนายออกมาแล้วไม่ถูกต้อง user จะสามารถแก้ไขได้ โดยจะทำการเก็บสิ่งที่ user แก้ไขให้ถูกต้อง โดย -->
- if model model made incorrect predictions , user can make edit for correct predictions with will save to S3 Bucket
<!-- - เป็น base line โดยเราจะใช้สิ่งนี้เป็น ground truth และใช้ เป็น data set ในการ train model ใหม่ -->

- to be base line for use as ground truth and use as new data set for training new model 

**Retrain**
 
<p style="text-align: center;"></p> 
<p align="center"><img height ="300px" class="center-block" src="../../images/ml/Pipeline_3.png"></p>

<!-- 7. หากความแม่นยำของ model ต่ำเกินค่าที่รับได้ Amazon CloudWatch Event จะทำการ แจ้งเตือน และ Trigger  Pipeline Build model -->
7. If accuracy of model lower then our  threshold , Amazon CloudWatch Event will  alerm and  Trigger Pipeline Build model

<!-- 8. Trigger  Sagemaker Pipeline ที่ run อยู่ใน ml.p3.8xlarge จะทำการ Train model ตัวเก่า และตัวใหม่ โดยใช้  ground truth   ที่ user ทำการแก้ไข เพื่มเป็น data set เพิ่มด้วย  -->
8. Trigger  Sagemaker Pipeline that run in  ml.p3.8xlarge will re Training old model and new modl with use  ground truth  that  user are fix addition to  data set   
<!-- - จากนั้นทำ 4 ต่อ -->
 -  to 4 
 