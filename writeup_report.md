# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image1]: ./writeup_images/step_1.jpg "Step 1"
[image2]: ./writeup_images/step_2.jpg "Step 2"
[image3]: ./writeup_images/step_3.jpg "Step 3"
[image4L]: ./writeup_images/step_4_left.jpg "Step 4 left"
[image4R]: ./writeup_images/step_4_right.jpg "Step 4 right"
[image5]: ./writeup_images/step_5.jpg "Step 5"
[image7]: ./writeup_images/step_6.jpg "Step 7"

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I'm not using draw_lines() function. Instead of this I've created the new one get_image_with_lines() function. My pipeline consisted of 8 steps: 
1) Convert the images to grayscale
![alt text][image1]
2) Blur the images by using Gaussian blur
![alt text][image2]
3) Get edges by using Canny 
![alt text][image3]
4) Get two regions of interest, for left and right line accordingly. I decided to process separately to increase velocity and accuracy
![alt text][image4L] ![alt text][image4R]
5) Use Hough transform to detect line segments
![alt text][image5]
6) As a result of step 5 we get a array of lines (start and end points, actually). By using start and end points we can create linear equation (y = k * x + b) for every line in this array. When I create linear equation for every line, I can calculate k-coefficient which determines angle of slope. After that I can filter array of lines by angle of slope. Lines in the left region of interest should have angle of slope between -40 and -28 degrees; in the right region of interest - between 28 and 40 degrees. We are not interested in lines go strictly vertical or horizontal. Now I can calculate 'mean' line of filtered lines.
7) Draw left and right 'mean' lines from bottom to top of region of interest
![alt text][image7]
8) Save current 'mean' coefficients. I need these coefficients to move line smoothly (without big jumps) between frames of video. 

### 2. Identify potential shortcomings with your current pipeline

The main shortcoming: region of interest and angle of slope are hardcoded. It works only for current type of video/camera. 

Another shortcoming: lines are 'jumping' between frames of video.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to automatically detect region of interest, horizon line, center line.

Another potential improvement could be to create something like a buffer between frames of video for lines to reduce 'jumping'
