# Monitoring face mask compliance using computer vision
<img src="https://user-images.githubusercontent.com/70846659/134135944-962fdbb7-e5cf-461b-ad9b-e057aff07287.png" width="600" height="350" />

## The problem
While authorities enforce the mandatory use of face masks as a preventive measure against COVID-19 transmission,  there are many cases half-compliance and sham compliance. "Many would wear masks, but “slide them down onto their chins or take them off completely while talking to someone on the street or speaking on the phone”. In other cases, the mask is not properly positioned, exposing the nose, mouth or chin. 

Such widespread practice is severely undermining the effectiveness of measures put in place and to the extent it provides a false sense of confidence to the public, stricter adherence to the regulation need to be implemented especially with more infectious variants spreading.

This challenge presents an interesting application of computer vision which the project aims to explore. Can we use deep learning to differentiate between a person not wearing a face mask and wearing one? And if so, is he/she wearing it properly? 

Serving primarily as a proof-of-concept, this project seeks to demonstrate the potential of using computer vision to actively monitor proper compliance of wearing face masks. This could be valuable to authorities and even private establishments who would like to enforce the use of face masks more strictly. 

## Objective and scope 
Achieving a good level of accuracy for all three cases will be crucial for implementation. Hence, the main objective is to reach at least 90% accuracy across all three classes (‘without mask’, ‘with mask’, ‘incorrectly worn’). Special attention will be given to the type of mistakes the model makes. Specifically, there is some challenge expected in discriminating between the proper and the improper use of masks but less in detecting whether the subject is wearing a face mask or not.  

In improving the implementation of preventive measures against Covid, there are undeniably other factors to consider such as the type of face mask worn. However, this will be out of scope and can be considered in a further study following the success of this project. Additionally, the scope is restricted to the detection of correctly worn face masks on individuals rather than on groups of people. The main deliverables of the project are code, model and accompanying report for documentation.

## The dataset 
While there are many available datasets containing images of people wearing masks, there are not enough for the improper use of face masks (i.e. not covering nose, mouth, chin). Hence, a combination of a few sources were used to train the model. 

<img src="https://user-images.githubusercontent.com/70846659/134138532-c15a548f-805d-4159-9d21-7e3d8e66161e.png" width="500" height="300" />

- MaskedFace-Net: https://github.com/cabani/MaskedFace-Net 
- Alibaba Tianchi: https://tianchi.aliyun.com/dataset/dataDetail?dataId=93724
- FaceMaskDetection: https://github.com/chandrikadeb7/Face-Mask-Detection

The combined final dataset created a total of 6,041 images (2,015 examples of correctly worn masks, 2,096 examples of incorrectly worn masks, and 1,930 examples of no mask worn) which were then split 60/20/20 between train, development and test sets. 

A few examples for each label is provided below. 

<img src="https://user-images.githubusercontent.com/70846659/134144149-b76bc41b-c41e-4e53-bfa1-88167617c9e2.png" width="700" height="600" />

## Modelling
The images were uniformly resized to 224 x 224 prior to model training and processed in batches of 128. The models were trained with an Adam optimiser for 50 epochs although early stopping was implemented based on decreasing model loss. Before training on more advanced neural architectures, I used a 3-block VGG-style architecture to establish a baseline performance level, upon which the succeeding models will be benchmarked against. 

**Baseline performance**

INSERT BASELINE PERFORMANCE HERE 

**Leveraging on existing model architectures**

To select the model architecture best suited to this problem, I evaluated the performance under the following scenarios:  
1) Using the pre-trained model and weights as a feature extractor;
2) Fine-tuning the pre-trained model by unfreezing and retraining the last two layers; and
3) Learning the weights of the model architecture by unfreezing the base model 

Transfer learning has allowed us to surpass the benchmark accuracy with ResNet50 consistently performing better than the other architectures across the three scenarios. 

<img src="https://user-images.githubusercontent.com/70846659/134142328-f4d6bdba-8cd9-448e-a2e5-0787ba66d18c.png" width="600" height="250" />

The ResNet model was then optimized for the number of layers to unfreeze and the learning rate. 

INSERT PERFORMANCe.


## Conclusion
Transfer learning has been key to unlocking superior model performance. Using the ResNet50 architecture, we were able to achieve a 97% accuracy rate on the test set, demonstrating the potential of using computer vision to enforce stricter compliance to face mask regulations. 

It is worth noting, however, that this performance level is based on the specific dataset used for the project which has its limitations. Majority of incorrectly worn face masks samples were synthetic, edited surgical face masks overlaid on the subjects’ faces, raising possible challenges in the generalizability of the model, specifically in detecting non-surgical incorrectly worn masks. Moreover, the training dataset could benefit from increased diversity in terms of age and race. Hence, before implementation, model performance should be tested against these possible shortcomings.

With deployment in mind, future projects can focus on extending the model to make classifications for multiple subjects at one as this will be valuable in monitoring crowded places like train stations and airports. Moreover, future models can also aim at making predictions in real-time (video input instead of images) and improving the efficiency of the model. 

