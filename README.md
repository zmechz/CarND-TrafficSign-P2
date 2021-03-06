#**Traffic Sign Recognition**

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Example References)

[TrainSampleData]: ./examples/training_samples_per_class.png "Training Samples per Class"
[ValidSampleData]: ./examples/validation_samples_per_class.png "Validation Samples per Class"
[TestingSampleData]: ./examples/testing_samples_per_class.png "Testing Samples per Class"
[OriginalYield]: ./examples/original_yield.png "Traffic Sign - Yield"
[YieldGray]: ./examples/grayscale_yield.png "Traffic Sign - Yield - Grayscale"
[YieldGrayEq]: ./examples/grayscale_eq_yield.png "Traffic Sign - Yield - Grayscale + Histogram Equalization"
[YieldGrayEqNorm]: ./examples/grayscale_eq_norm_yield.png "Traffic Sign - Yield - Grayscale + Histogram Equalization + Normalization"
[PredictionsFinal]: ./examples/new_images_final_results.png "New images predictions"
[NNArchitecture]: ./examples/nn-architecture.png "My Neural Network Architecture"

[//]: # (New Image References)
[Pedestrians]: ./new_images/pedestrians.jpg "Pedestrians"
[Roadwork]: ./new_images/road_work.jpg "Road Work"
[Stop]: ./new_images/stop.jpg "Stop"
[TurnRightAhead]: ./new_images/turn_right_ahead.jpg "Turn Right Ahead"
[Yield]: ./new_images/yield.jpg "Yield"


## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/zmechz/CarND-TrafficSign-P2/blob/master/Traffic_Sign_Classifier.ipynb)

###Data Set Summary & Exploration

####1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the Numpy library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32x32x3 (WxHxChannels)
* The number of unique classes/labels in the data set is 43


####2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data:<br>
![TrainSampleData]
<br>
![ValidSampleData]
<br>
![TestingSampleData]
<br>

###Design and Test a Model Architecture

####1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

Original image:<br>
![OriginalYield]

1) I decided to convert the images to grayscale because using only one colour channel as input for the neural network would reduce the number of parameters to be processed by each layer. This could make training and validation achieve better precision. <br>
![YieldGray]

2) I decided to perform histogram equalization to adjust contrast. Images that were too dark, become lighter and easier to see after that.<br>
![YieldGrayEq]

3) As a third step, I decided to perform image normalization to fix glare issues.<br>
![YieldGrayEqNorm]


####2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:


| Layer | Description |
|:-------------------:|:---------------------------------------------:|
| Input | 32x32x3 RGB image |
| Convolution 3x3 | 1x1 stride, VALID padding, outputs 28x28x12 |
| RELU | |
| Max pooling	      | 2x2 stride,  outputs 14x14x12 	              |
| Convolution 3x3	  | 1x1 stride, VALID padding, outputs 10x10x32   |
| RELU				  |									        	  |
| Max pooling	      | 2x2 stride,  outputs 5x5x32   	              |
| Convolution 3x3	  | 1x1 stride, VALID padding, outputs 3x3x64     |
| RELU				  |	|
| Flatten             | Input = 576, Output = 800                     |
| Dropout             | Keep probability = 75%                        |
| Fully connected	  | Input = 800, Output = 256 				      |
| RELU				  |										          |
| Dropout             | Keep probability = 75%                        |
| Fully connected	  | Input = 256, Output = 84  				      |
| RELU				  |										          |
| Fully connected	  | Input = 84, Output = 43   				      |
| Softmax			  |    						          |


![NNArchitecture]


####3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used the following parameters:

Name | Value
------------ | -------------
**EPOCHS** | 80 |
**BATCH_SIZE** | 128
**LEARNING_RATE** | 0.0005
**MU** | 0
**SIGMA** | 0.1

####4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 99%
* validation set accuracy of 95.4%
* test set accuracy of 93.8%

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
I used Lenet original architecture from the Lesson Project Traffic Sign Classifier part 11. Lenet is a well-known good performance/stable architecture and it was suggested to be tested as a possible architecture for this project.

* What were some problems with the initial architecture?
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
* Which parameters were tuned? How were they adjusted and why?
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

After trying to fine tune the batch size, number of epochs, sigma and mu, and using image preprocess techniques, the best case scenario was achieving around 90% accuracy on training and validation.
I tried increasing the number of EPOCHs to 100, but doing so, made me realize my model was suffering from overfitting.
Training accuracy was improving fast and soon it became very stable, while validation accuracy stop to improve much earlier. Loss was minimal on training but not so good on validation/test.
Therefore, I decided to increase the number of filters, add one more convolution layer to my neural network to make it more complex, and used dropouts at end to reduce overfitting. In the end I reduced EPOCHs to 80.


###Test a Model on New Images

####1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![Pedestrians]
![Roadwork]
![Stop]
![TurnRightAhead]
![Yield]

The original images I found on the web were too large (more than 300 x 300 pixels) and when I applied resize to 32x32 (to satisfy NN input), these images lost too many pixels and became poor quality thumbnails. This made it even harder for them to be recognized ny the NN. If I would have used smaller pictures, the accuracy could have been even better.
First image (Pedestrian) has a "watermark" on its center. This make prediction goes totally wrong about it.

####2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					|
|:---------------------:|:---------------------------------------------:|
| Pedestrian      		| Roundabout mandatory							|
| Road Work    			| Road Work										|
| Stop					| Stop											|
| Turn Right Ahead		| Turn Right Ahead				 				|
| Yield	            	| Yield              							|


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This result was a little bit worse than the test set. Probaly, this was a consequence of using just 5 signs to calculate accuracy in this case.

####3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The predicition result:
![PredictionsFinal]

For the first image, the model is totally random due to the "watermark" (spin logo) on its center.
The prediction hasn't even mentioned "Pedestrian" as a possible case.
