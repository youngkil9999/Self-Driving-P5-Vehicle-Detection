
#Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/1.png
[image2]: ./output_images/2.png
[image2_1]: ./output_images/2_1.png
[image3]: ./output_images/3.png
[image4]: ./output_images/4.png
[image4_1]: ./output_images/4_1.png
[image4_2]: ./output_images/4_2.png
[image5_1]: ./output_images/5_1.png
[image6_1]: ./output_images/6_1.png
[image6_2]: ./output_images/6_2.png
[image6_3]: ./output_images/6_3.png
[image6_4]: ./output_images/6_4.png
[image6_5]: ./output_images/6_5.png
[image6_6]: ./output_images/6_6.png
[image7]: ./output_images/7.png
[image7]: ./output_images/7.png

[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook (or in lines # through # of the file called `Submit1.ipynb`).  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]
![alt text][image3]
![alt text][image2_1]


####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and...
tried change color, specially RGB, YCrCb and searching window sizes and overlap sizes

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM with 8000 images of vehicles and 8000 images of non-vehicles.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

firstly, closed out left side of the images so don't be bothered by the other side of the lanes vehicles.
also, windows searches over half of the bottom images only and the windows sizes varies from 12x12 to 128x128 with 3 steps. so total windows would be like 12x12, 70x70, and 128x128.

Total mixed search windows images
![alt text][image4]

each window size
![alt text][image4_1]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Processed only with HOG features and the result is not good. I will proceed it to the next step now. I will optimize the image later. Currently, I could get the image something like this.
![alt text][image5_1]
![alt text][image4_2]

---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](https://youtu.be/JtLiAoPsB4o)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![alt text][image6_1]
![alt text][image6_2]
![alt text][image6_3]
![alt text][image6_4]
![alt text][image6_5]
![alt text][image6_6]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image7]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Working on how to optimize features for searching the cars in a highway. Need some advice to figure out how to optimize the filters. tried to change the color, RGB, HSV, YCrCb and parameters for threshold. but not perfectly working.


Update. I got a best optimized solution so far. as far as I could search for.
