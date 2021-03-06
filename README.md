# Monitoring face mask compliance using computer vision
<img src="https://user-images.githubusercontent.com/70846659/134135944-962fdbb7-e5cf-461b-ad9b-e057aff07287.png" width="600" height="350" />

## The problem
While authorities enforce the mandatory use of face masks as a preventive measure against COVID-19 transmission,  there are many cases of half-compliance or sham compliance. Many would wear masks, but slide them down onto their chins or take them off completely while talking to someone. In other cases, the mask is not properly positioned, exposing the nose, mouth or chin. 

Such widespread practice is severely undermining the effectiveness of measures put in place and to the extent it provides a false sense of confidence to the public, stricter adherence to the regulation need to be implemented especially with infectious variants spreading.

This challenge presents an interesting application of computer vision which the project aims to explore. 

**Can we use deep learning to distinguish between a person who is not wearing a mask and one who is? And if so, whether he/she is wearing the mask properly?** 

## Objective and scope 
Achieving a good level of accuracy for all three cases will be crucial for implementation. Hence, the main objective is to reach at least 90% accuracy across all three classes (‘without mask’, ‘with mask’, ‘incorrectly worn’). Special attention will be given to the type of mistakes the model makes. Specifically, there is some challenge expected in identifying the proper use of masks but less so in classifying whether the subject is wearing a face mask (regardless of positioning). 

In improving the implementation of preventive measures against Covid, there are undeniably other factors to consider such as the type of face mask worn. However, this will be out of scope and can be considered in a further study following the success of this project. Additionally, the scope is restricted to the detection of correctly worn face masks on individuals rather than on groups of people.

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
The images were uniformly resized to 224 x 224 prior to model training and processed in batches of 128. The models were trained with an Adam optimiser for 50 epochs although early stopping was implemented based on decreasing model loss. 

**Baseline performance**

Before training on more advanced neural architectures, I used a 3-block VGG-style model to establish a baseline performance level, upon which the succeeding models will be benchmarked against. 

<img src="https://user-images.githubusercontent.com/70846659/134152811-9eb04fd0-c23f-41ca-85b5-e586178b4219.png" width="450" height="600"/>


While achieving a decent performance of 90% accuracy, classification of subjects without masks requires further improvement as 13% are erroneously predicted as having masks.

**Leveraging on existing model architectures**

To select the model architecture best suited to this problem, I evaluated the performance under the following scenarios:  
1) Using the pre-trained model and weights as a feature extractor;
2) Fine-tuning the pre-trained model by unfreezing and retraining the last two layers; and
3) Learning the weights of the model architecture by unfreezing the base model 

Transfer learning has allowed us to surpass the benchmark accuracy with ResNet50 consistently performing better than the other architectures across the three scenarios. 

<img src="https://user-images.githubusercontent.com/70846659/134142328-f4d6bdba-8cd9-448e-a2e5-0787ba66d18c.png" width="700" height="250" />

The ResNet model was then optimized for the number of layers to unfreeze and the learning rate. The best model was trained with a 0.001 learning rate with all layers of the base model fine-tuned.

<img src="https://user-images.githubusercontent.com/70846659/134161727-e134bea3-4dd3-432c-90c2-96e27c488c02.png" width="450" height="600" />

This achieved a 97% accuracy rate. Looking at the predictions for each class, it is important to highlight the following: 
1) Model achieved top performance on identifying subjects without masks 
- F1 score of 0.99
- Out of the subjects who are not wearing masks, the model misses to identify only 2% of the group. Keeping this number to a minimum is crucial from a practical standpoint as these are the individuals who need to be prompted to comply with the protocols.   

2) The lowest predictive performance on a class still surpassed our benchmark, achieving a 97% recall
- The error mainly comes from misclassifying the 3% of subjects with properly worn masks as not having worn them properly. If implemented, making this type of mistake is more ‘forgivable’ as it leans towards a more cautious reaction.

## Conclusion
Using the ResNet50 architecture, we were able to achieve a 97% accuracy rate on the test set, demonstrating the potential of using computer vision to enforce stricter compliance to face mask regulations. 

It is worth noting, however, that this performance level is based on the specific dataset used for the project which has its limitations. Majority of incorrectly worn face masks samples were synthetic, edited surgical face masks overlaid on the subjects’ faces, raising possible challenges in the generalizability of the model, specifically in detecting non-surgical incorrectly worn masks. Moreover, the training dataset could benefit from increased diversity in terms of age and race. Hence, before implementation, model performance should be tested against these possible shortcomings.

With deployment in mind, future projects can focus on extending the model to make classifications for multiple subjects at one as this will be valuable in monitoring crowded places like train stations and airports. Moreover, future models can also aim at making predictions in real-time (video input instead of images) and improving the efficiency of the model. 

