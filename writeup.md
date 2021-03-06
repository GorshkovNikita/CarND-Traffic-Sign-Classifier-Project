# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/number_of_examples.png "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./german_traffic_signs/sign_example.png "Sign example"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/GorshkovNikita/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32x32
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. 
It is a bar chart showing number of examples of each sign in training set.

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I decided not to convert the images to grayscale because it can harm model performance since we lose a lot of color information, which can be very useful for determining sign class.

As a last step, I normalized the image data because it improves performance of optimization algorithm. For that, I used the following formula: `normalized_image = (image - 128) / 128`. It doesn't change the contents of an image but makes mean equals zero.

Here is an example of an original image:

![alt text][image3]


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride, outputs 14x14x6                   |
| Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x16.  |
| RELU                  |                                               |
| Max pooling           | 2x2 stride, outputs 5x5x16                    |
| Flatten               | Outputs 400                                   |
| Fully connected		| Outputs 120        							|
| RELU                  |                                               |
| Fully connected       | Outputs 84                                    |
| RELU					|												|
| Fully connected       | Outputs 43                                    |
|:---------------------:|:---------------------------------------------:|
 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an AdamOptimizer to optimize cross entropy loss function between logits and one hot encoded labels
of training data. I chose following hyperparameters:
```
epochs = 8
batch_size = 120
learning_rate = 0.002
```

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.992
* validation set accuracy of 0.933 
* test set accuracy of 0.897

I chose to use the LeNet architecture for this project. I adjusted architecture from the LeNet lab to process
colored images, because the color of sign is important for classifying it correctly. Also I adjusted the output layer
to have outout size equal to number of sign classes.
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are eight German traffic signs that I found on Google Street View:

<img src="./german_traffic_signs/12.png" height="100">
<img src="./german_traffic_signs/13.png" height="100">
<img src="./german_traffic_signs/15.png" height="100">
<img src="./german_traffic_signs/17.png" height="100">
<img src="./german_traffic_signs/18.png" height="100">
<img src="./german_traffic_signs/22.png" height="100">
<img src="./german_traffic_signs/25.png" height="100">
<img src="./german_traffic_signs/35.png" height="100">

Images with labels 15 (No Vehicles), 22 (Bumpy Road), 25 (Road Work), 35 (Ahead Only) might be difficult to classify because there aren't many samples of such signs in
training set.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| Actual label  |
|:---------------------:|:---------------------------------------------:|:-------------:| 
| Priority road         | Priority road                                 | 12            |
| Yield     			| Yield 										| 13            |
| No Vehicles			| Speed limit (30km/h)									| 15            |
| No entry      		| No entry 					 				| 17            |
| General caution			| General caution     							    | 18            |
| Bumpy road       | Bumpy road                               | 22            |
| Road Work                  | Road Work                                          | 25            |
| Ahead only            | Ahead only                                    | 35            |


The model was able to correctly guess 7 of the 8 traffic signs, which gives an accuracy of 87.5%. 
This compares favorably to the accuracy on the test set of 89.7%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the cell under `Output Top 5 Softmax Probabilities For Each Image Found on the Web` section of the Ipython notebook.

The top five soft max probabilities were the following:
Priority road:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Priority road   									| 
| .0     				| End of all speed and passing limits 										|
| .0					| Dangerous curve to the left											|
| .0	      			| Yield					 				|
| .0				    | Speed limit (80km/h)      							|

Yield:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Yield   									| 
| .0     				| No vehicles 										|
| .0					| Ahead only											|
| .0	      			| Priority road					 				|
| .0				    | Speed limit (80km/h)      							|


No Vehicles:
 
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.858         			| Speed limit (30km/h)   									| 
| 0.095     				| Speed limit (70km/h) 										|
| 0.035 					| Speed limit (50km/h)											|
| 0.009 	      			| No vehicles					 				|
| .0 				    | Turn right ahead      							|

Road Work:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| Road Work   									| 
| .0     				| Dangerous curve to the right 										|
| .0					| Road narrows on the right											|
| .0	      			| General caution					 				|
| .0				    | Pedestrians      							|

Ahead only:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.993         			| Ahead only   									| 
| 0.006     				| Dangerous curve to the right 										|
| .0					| Road narrows on the right											|
| .0	      			| General caution					 				|
| .0				    | Pedestrians      							|

General caution:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.0         			| General caution   									| 
| .0     				| Pedestrians 										|
| .0					| Speed limit (70km/h)								|
| .0	      			| Traffic signals					 				|
| .0				    | Road narrows on the right      							|

No entry:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.993         			| No entry   									| 
| 0.006     				| Stop 										|
| .0					| 	Speed limit (20km/h)											|
| .0	      			| Yield					 				|
| .0				    | Bumpy road      							|

Bumpy road:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.997         			| Bumpy road   									| 
| 0.001     				| Wild animals crossing 										|
| 0.001					| Road work											|
| .0	      			| Bicycles crossing					 				|
| .0				    | Dangerous curve to the right      							|


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


