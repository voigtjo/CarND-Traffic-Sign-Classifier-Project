# **Traffic Sign Recognition** ### Writeup for the CarND-Traffic-Sign-Classifier-Project---**The following steps were carried out for the implementation of the project:*** Load the data set (see below for links to the project data set)* Explore, summarize and visualize the data set* Design, train and test a model architecture* Use the model to make predictions on new images* Analyze the softmax probabilities of the new images* Summarize the results with a written report[//]: # (Image References)[label_frequency]: ./label_frequency.png "Label frequency"[right_of_way]: ./new-traffic-signs/1.png "Right-of-way next intersection"[right_of_way_perfect]: ./new-traffic-signs/1b.png "Right-of-way next intersection"[speed_limit_30]: ./new-traffic-signs/2.png "Speed limit (30km/h)"[priority road]: ./new-traffic-signs/3.png "Priority road" [keep_right]: ./new-traffic-signs/4.png "Keep right"[turn_left_ahead]: ./new-traffic-signs/5.png "Turn left ahead" [speed_limit_60]: ./new-traffic-signs/6.png "Speed limit (60km/h)"## Rubric Points### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  ---### Writeup / READMEYou're reading it! And here is a link to my [project code](https://github.com/voigtjo/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)### Data Set Summary & Exploration#### 1. Basic summary of the data set. I used the pandas library to calculate summary statistics of the trafficsigns data set:* The size of training set is 34799 * The size of the validation set is 4410 * The size of test set is 12630 * The shape of a traffic sign image is (32, 32)* The number of unique classes/labels in the data set is 43#### 2. Include an exploratory visualization of the dataset.Here is an exploratory visualization of the data set. It is a bar chart showing the histogram of label frequency:![alt text][label_frequency]### Design and Test a Model Architecture#### 1. PreprocessingIn the preprocessing I decided first to shuffle the data an than to normalize the image from value range 0 - 255 to 0.1 - 0.9#### 2. Model ArchitectureThe LeNet-5 architecture works well for the classification of digits. Essentially, every image can be represented as a matrix of pixel values.In concrete terms, the images with a traffic sign have a fundamental structural similarity to the images of figures. One difference is that there are not 10 labels as for digits, but in this case 43 classes of traffic signs.Therefore I took the LeNet-5 neural network architecture  for the classification of traffic signs.I decided to add dropouts to the neural network after the first fully connected layer to avoid overfitting and improve the accuracy. The normalization step in the pre-processing of the data and the dropouts together improved the accuracy from 88% to more than 93%.| Layer         		|     Description	        					| |-----------------------|-----------------------------------------------| | Input         		| 32x32x3 RGB image   							| | Convolution 5x5     	| stride:1, padding:valid, output shape:28x28x6	|| RELU					|												|| Max pooling	      	| padding:valid, output shape:14x14x6			|| Convolution 5x5	    | stride:1, padding:valid, output shape:10x10x16|| RELU					|												|| Max pooling	      	| padding:valid, output shape:5x5x16			|| Flatten				| output: 400									|| Fully connected		| output: 120       							|| Dropout		        |        										|| RELU					|												|| Fully connected		| output: 84       							    || RELU					|												|| Fully connected		| output: 43       							    |#### 3.Model TrainingI trained the model with a learning rate of 0.001, batch size as 128, 20 epochs and a keeping probability for dropouts of 0.5After each epoch, I measured the accuracy of the validation set. After 10 epochs the accuracy was constantly >0.93. ### Test a Model on New Images#### 1. I've choosen 7 German traffic signs found on the web: * ![alt text][right_of_way]* ![alt text][right_of_way_perfect]* ![alt text][speed_limit_30]* ![alt text][priority road]* ![alt text][keep_right]* ![alt text][turn_left_ahead]* ![alt text][speed_limit_60]#### 2. Here are the results of the prediction:| Image			               |     Prediction	        		|  Probability         | |------------------------------|--------------------------------|-------------:| | Right-of-way next intersection      | Ahead only|  100.00% || Right-of-way next intersection (perfect)      | Right-of-way next intersection |  100.00% || Speed limit (30km/h)	| Speed limit (30km/h) 	|100.00%  || Priority road			| Priority road			|100.00% || Keep right			| Keep right			|100.00% || Turn left ahead		| Turn left ahead      		|99.99%  || Speed limit (60km/h)	| Speed limit (60km/h)    	|100.00% |Without the (perfect) Right-of-way next intersection image, the model was able to correctly guess 5 of the 6 traffic signs, which gives an accuracy of 83%. This is ok, but the classification of the real life Right-of-way next intersection image (first image) ![alt text][right_of_way] is completly wrong.The angle under which a traffic sign can be seen, light conditionen as reflections and also the background might influence the results and make it difficult for your model to classify those. The classification is not yet robust for real life applications.I have checked with a perfect Right-of-way next intersection image (second image) ![alt text][right_of_way_perfect] that at least this is properly classified. (So in total model was able to correctly guess 6 of the 7 traffic signs, which gives an accuracy of 86%)