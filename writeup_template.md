## Writeup Template


**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: https://github.com/willofdiamond/Vehicle-Detection/blob/master/sampleimages.png

[image2]: https://github.com/willofdiamond/Vehicle-Detection/blob/master/Hogimages.png
[image3]: https://github.com/willofdiamond/Vehicle-Detection/blob/master/slidingwindows.png
[image4]: https://github.com/willofdiamond/Vehicle-Detection/blob/master/averagewindows.png
[link to video]:https://github.com/willofdiamond/Vehicle-Detection/blob/master/projectVideoResult.mp4 "Video"



---


#### 1. Flow of the project
1. Read images
2. Extract features
3. Train the classification model
4. Use different search windows and apply classification
5. Algorithm to eliminate false positives
7. Draw boxes on the detected vehicles
6. Processing the videos

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

First I had read the car and non car images. Sample of the car and non car images is shown in the below figure

![alt text][image1].

Later Hog features, spatial and histogram features are extracted from the images and stored in a features array. The features array is further divided in to training and test sets. The training set is using to train a Support Vector classifier while test set is used to check the model performance. For my project with all the Hog, spatial and histogram features I am achieving a test accuracy of 99.03%.  



#### 2. Explain how you settled on your final choice of HOG parameters.

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). After several trail and error I have fixed on  orient = 16, pix_per_cell = 16, cell_per_block = 2. Some of the hog images are displayed below


![alt text][image2]

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using 75% of the data and tested it on the remaining 25% of the data. When using only Hog features I am achieving up to 96% accuracy. But by adding spatial and histogram features a test accuracy of 99.03% has been achieved.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

 I had choose 4 sliding windows with decreasing window sizes 128x128, 96x96, 80x80 and 60x60 at different locations chosen by visual inspection of the image frame along the road. All sliding windows has a 50% overlap. The idea is to find cars at different locations and visually car size reduces in the video as the car progress on the road. The windows used is shown below. Code for sliding window can be found in the cell 11 of carDetect notebook

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I had used HSL 3 channel image HOG features plus spatially binned color and histograms of color as feature vector, which provided a nice result. As visible in the left column there are lot of false positives. The false positives are eliminated by using the average of the overlap of the windows and thresholded using a threshold from the heat maps. Windows detected in the previous images were also passed to reduce the false positives.


Here are six frames and their unprocessed windows, corresponding heatmaps and average windows:
![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to video]


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The problems that I faced while implementing this project were improving the classification accuracy of the classifier by changing different parameters and reducing the number of false positives.

The pipeline may fail when the lighting conditions are bad. Distinction between the nearby cars  and cars in the other lane has to be worked on. Training the classifier with wide variety of lighting conditions , resolution and using Deep learning algorithms will result in much accurate result.
