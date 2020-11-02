# Diagnosing Pneumonia Using AI

<a href="https://github.com/ariavathlete/pneumonia_diagnosis/blob/master/Clinical%20Case%20of%20Pneumonia.pdf">PRESENTATION</a>
<a href="https://github.com/ariavathlete/pneumonia_diagnosis/blob/master/Diagnosing_Pneumonia_Blog.pdf">| BLOG</a>

  <img src='images/blue.PNG' width='50%'/>

# Table Of Contents
* [Purpose](#purpose)
* [Data Description](#data-description)
* [Data Proportion](#data-proportion)
* [Data Augmentaion](#data-augmentation)
* [Model](#model)
* [Recommendation](#recommendation)
* [Future Work](#future-work)
  
<!---
# = h1
## = h2
### = h3
#### = h4
##### = h5
--->

# Purpose
The purpose of this research is to build a classifier that can correctly diagnose Pneumonia. Why Pneumonia?
   * 100,000 Deaths per year due to the misdiagnosis of pneumonia. Wrongful diagnosis of pneumonia can be very life threatening given that it leads to an increase in severity due to lack of treatment. Especially in cases where the patient might have a more serious infection like COVID-19.

   * Pneumonia is the reason for 1 out of  6 childhood death making it the leading cause of fatality in kids under 5 years. 

   * In the United States, the death rate of pneumonia is 10 out of every 100,000 individuals and this usually the rate in most developed countries. Meanwhile, in Africa, the death rate of pneumonia is 100 out of every 100,000 individuals and this is normal in most developing countries.

   <img src='images/timeline.PNG' width='80%'/>

## Data Description
For this research, I used the Pneumonia dataset from Kaggle’s website. I used 2,682 x-ray images of patient which were labeled by a specialist as either Normal or Pneumonia. The merged dataset file can be found in the xray folder of this repository. The datasets downloaded can be found: https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia
  <img src='/images/kaggle.PNG' width='80%'/>

## Data Augmentation
The data was imbalanced so I'll use ImageDataGenerator to create additional dataset to help our modeling training. This will allow the network to see more diversification withing the dataset without any reduction in how representative the dataset for each category is during training. I won’t do the same for the test dataset as I won’t want to tamper with the data that I’ll be validating with. My parameters here are;

* shear_range=0.2
* rotation_range=20
* width_shift_range=0.2
* height_shift_range=0.2
* horizontal_flip=True
* vertical_flip=False
* zoom_range=0.2

After that, I inserted the images using flow. My parameters are; 32 images should be used for training at a given instance (batch size), my image size is 64 X 64 and the class mode is set to categorical.
### Checkpoint
#### ModelCheckpoint
* monitor = val_loss
* mode = min
* save_best_only = True
* verbose = 1

#### EarlyStopping
* monitor = val_loss
* mode = min
* save_best_only = True
* verbose= 1

#### ReduceLROnPlateau
* monitor = val_loss
* patience = 30
* verbose = 1 
* factor = 0.8
* min_lr = 0.0001
* mode = auto
* min_delta = 0.0001
* cooldown = 5

I go on and apply the same parameters I used for my training dataset to my test dataset and then I call my fit 100 epochs.


## [Model](./student.ipynb)
The network used is VGG19 because it’s known for having pretty high accuracies for image classification problems so I have no doubt it would work perfectly for my problem. After importing my VGG19 model and set the appropriate weights for the type of images in the dataset and set the Include Top parameter to false. This will ensure that the last layer is drop and I did this because I don’t want to classify thousand different categories when my specific problem only has two categories. So, for this I skip the last layer. The first layer is also dropped since I can simply provide my own image size as I did.

## Interpretion
The accuracy is 95 % and this is the amount of time the predicted result is actually correct.

The recall percentage is 95% and this is the probability of the model diagnosing a correct positive diagnosis out of all the times it diagnosed positive. This would be the best metric in this case as we would rather give a wrong positive diagnosis than give a wrong negative diagnosis.
 
  <img src='images/cm_pne.PNG' width='50%'/>

The model loss is 0.14 out and this is the amount the model penalizes for incorrect predictions ~ 10%

  <img src='images/loss_pne.PNG' width='90%'/>

The AUC score is 0.94 and this is the average probability that the model can diagnose each X-ray image correctly.

  <img src='images/roc_pne.PNG' width='50%'/>


### Recommendation
The recall score will be the main metric for this project since it’s the most important metric in medical problems given that - doctors will rather make a wrong positive diagnosis than make a wrong negative.

Health professionals are welcomed to integrate this model, after thorough verification, into their medical software to help them correctly diagnose pneumonia.



# Future Work
   * Other Lung Diseases: Create a classifier to differentiate pneumonia x-rays from other lung infections like COVID-19,  Tuberculosis, etc.
 
   * Target Detection: Create a classifier to detect what section of the lungs the infection is located.

   * Model Improvement: Collect more data and tune more layers to the transfer learning model to improve its performance..
