# Face detection task

## Overview

There are three parts to the project, each handled by a separate neural network:

1. Face detection model: locate face in an image; sliding window
2. Keypoint model: cut the box of a face image, locate facial features and rotate the face. This is necessary because face recognition models are sensitive and image location noise must be minimized
3. Face recognition (classification) model: Extract features using a model pre-trained on faces, and feed these features into a classification model. Train the model and identify individuals. 


While this is a guided project done as part of an advanced convolutional networks course, it contained minimal instructions and no starter code and required independent decision making and an iterative approach to get all elements work well together. None of the models performed sufficiently well as built according to the guidance, and needed to be further enhanced to achieve better performance.

NOTE: I have done the image part, and am finishing the video part which I will upload shortly once ready.

## Archtecture

Location model: trained to generate a binary prediction (face/no face) in a image box with a specified dimensions. Model weights are transferred to a fully convolutional network in order to identify the presence of a face or faces in a larger picture (sliding window). 

Keypoint model: a neural network building a regression of keypoint features. The main problem with this model was its failure to perform in non-mainstream cases (e.g. when the image box was cut too high or too low relative to the face). Aggressive image augmentation and proper selection of image resize mode solved this problem. 

Classification model: extract features using model pre-trained on faces (the architecture is that of VGG19 with an altered top layer). I used the top layer for feature extraction, and classified the predictions using a small fully connected network using softmax given the number of persons  ~100. 

## Data cleaning

The major problem I dealt with was the inputs and predictions inconsistencies. For example, input files came in different formats and with different ratios of face image to the overall image. This problem became too significant for video classification to require a separate procedure to address it.

In addition, models'  predictions revealed some corner cases where they failed, and the models had to be revised and retrained. For example, I used aggressive regularization and image augmentation for training the keypoints model, and a gentle image augmentation for training the face classification model, which were key to much better performance.

## Training

Given the image augmentation and a relatively small datasets, in most cases I used generators. Regularization and image augmentation allowed me to train models on a GPU for a large number of epochs without overfitting.


## Results

I got >95 percent identification accuracy for the images part of the assignment;

I am finishing the video part of the assignment and update the results once done. The only difference is picking the right resolution for the test set which is video; I intend to do this by evaluating the prediction confidence of the best fit for the location model taken at several resolutions and selecting the best one. Retraining the location model on larger faces could be an alternative approach.
