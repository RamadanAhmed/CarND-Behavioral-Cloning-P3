# **Behavioral Cloning** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/center_2016_12_01_13_30_48_287.jpg "Center Image"
[image2]: ./examples/left_2016_12_01_13_30_48_287.jpg "Left Image"
[image3]: ./examples/right_2016_12_01_13_30_48_287.jpg "Right Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

My model consists of a convolution neural network with 3x3 & 5x5 filter sizes and depths between 24 and 64 and fully connected layers

The model includes RELU layers to introduce nonlinearity, and the data is normalized and cropped in the model using a Keras lambda layer and Cropping2D. 

#### 2. Attempts to reduce overfitting in the model

The model was trained and validated on different data sets to ensure that the model was not overfitting. The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

The model was trained using 3 different images(center, left, right).

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually.
The model used steering correction which is manually choosen after several trials which helps determine the offset of steering angle between the 3 images 

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used the provided training data from udacity because it will be more general.

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy for deriving a model architecture was test the model on every change and determine the model behaviour

My first step was to use a convolution neural network model similar to the lenet 'used in traffic signs' I thought this model might be appropriate because it have different convolution layers and can be easily modified to serve our need

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. 

To combat the overfitting, I modified the model such that I added left and right images to training and validation data and also added mesaurements for both by using steering center provided by data and make and offset correction by 0.2 and normalize the image using lambda layer and finally add cropping layer so the model learn from area of concern only

Then I trained the model and seen the validation error and training error is very small and near each other so It indicate the model doesn't overfit the data

The final step was to run the simulator to see how well the car was driving around track one

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

_________________________________________________________________
Layer (type) | Output Shape | Param #
-------------|---------------|--------
lambda_1 (Lambda) | (None, 160, 320, 3) | 0
cropping2d_1 (Cropping2D) | (None, 90, 320, 3) | 0
conv2d_1 (Conv2D) | (None, 43, 158, 24) | 1824
conv2d_2 (Conv2D) | (None, 20, 77, 36) | 21636
conv2d_3 (Conv2D) | (None, 8, 37, 48) | 43248
conv2d_4 (Conv2D) | (None, 6, 35, 64) | 27712
conv2d_5 (Conv2D) | (None, 4, 33, 64) | 36928
flatten_1 (Flatten) | (None, 8448) | 0
dense_1 (Dense) | (None, 100) | 844900
dense_2 (Dense)  | (None, 50) | 5050
dense_3 (Dense) | (None, 10) | 510
dense_4 (Dense) | (None, 1) | 11


Total params: 981,819 ,Trainable params: 981,819,Non-trainable params: 0

#### 3. Creation of the Training Set & Training Process

Udacity provides sample dataset that I think was too general that we help the model to be general and here are examples of the data used from the images
![alt text][image1] ![alt text][image2] ![alt text][image3]