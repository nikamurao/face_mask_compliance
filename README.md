# Monitoring face mask compliance using computer vision

![image](https://user-images.githubusercontent.com/70846659/134134624-b94951d4-39e4-4895-808b-62725e486e9a.png)

## Motivation for the project 
While authorities enforce the mandatory use of face masks as a preventive measure against COVID-19 transmission,  “one can observe many cases of half-compliance or sham compliance.” Many would wear masks, but “slide them down onto their chins or take them off completely while talking to someone on the street or speaking on the phone”. In other cases, the mask is not properly positioned, exposing the nose, mouth or chin. 

Such widespread non-compliance is severely undermining the effectiveness of measures put in place and to the extent it provides a false sense of confidence to the public, stricter adherence to the regulation need to be implemented especially with more infectious variants spreading.

This challenge presents an interesting application of computer vision which the project aims to explore. Can we use deep learning to differentiate between a person not wearing a face mask and wearing one? And if so, is he/she wearing it properly? 

Developing an image classifier to discriminate between these cases will be valuable to authorities and even private establishments who would like to enforce the use of face masks more strictly. Serving primarily as a proof-of-concept, this project seeks to demonstrate the potential of using computer vision to actively monitor proper compliance of wearing face masks.

## Objective and scope 
The aim is to develop an image classifier that can distinguish not only between a person wearing a face mask or not, but also indicate whether the face mask is correctly being worn. Achieving a good level of accuracy for all three cases will be crucial for implementation. Hence, the main objective is to reach at least 90% accuracy across all three classes (‘without mask’, ‘with mask’, ‘incorrectly worn’). Special attention will be given to the type of mistakes the model makes. Specifically, there is some challenge expected in discriminating between the proper and the improper use of masks but less in detecting whether the subject is wearing a face mask or not.  

In improving the implementation of preventive measures against Covid, there are undeniably other factors to consider such as the type of face mask worn. However, this will be out of scope and can be considered in a further study following the success of this project. Additionally, the scope is restricted to the detection of correctly worn face masks on individuals rather than on groups of people. The main deliverables of the project are code, model and accompanying report for documentation.

The dataset
While there are many available datasets containing images of people wearing masks, there are not enough for the improper use of face masks (i.e. not covering nose, mouth, chin). Hence, a combination of a few sources were used to train the model. 

[FIGURE]

The combined final dataset created a total of 6,041 images (2,015 examples of correctly worn masks, 2,096 examples of incorrectly worn masks, and 1,930 examples of no mask worn) which were then split 60/20/20 between train, development and test sets. 


## Modelling
The images were uniformly resized to 224 x 224 prior to model training and processed in batches of 128. The models were trained with an Adam optimiser for 50 epochs although early stopping was implemented based on decreasing model loss.  
Before training on more advanced neural architectures, I used a 3-block VGG-style architecture to establish a baseline performance level, upon which the succeeding models will be benchmarked against. 


