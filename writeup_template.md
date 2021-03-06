## Writeup Template

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)
[image0]: ./examples/undistort_output_2.png "Calibration"
[image1]: ./examples/test1_undist.png "Undistorted"
[image2]: ./examples/test1_trans.png "Road Transformed"
[image5]: ./examples/curvature.png "Fit Visual"
[image6]: ./examples/example_output_1.png "Output"
[video1]: ./project_video_generated.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  



### Camera Calibration


The code for this step is contained in the function camera_Calibraton() in the file advanced_lane_detection.py (line 22 through line 56 ).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image0]

### Pipeline (single images)


To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image1]

![alt text][image2]


I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `another_file.py`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)


I used the following source and destination points to perform a perspective transform
This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.


The logic for detecting lines is in the function find_lanes(). This function can be found in the file advanced_lane_detection.py (line 155 to 216). I pass the combined binary image to this function. 
I use the histogram function to find the maximum concentration of pixels to detect left and right lanes. 


I fit a degree 2 polynomial for the left and right line using the lane_curve and the impose_lane_area functions

![alt text][image5]

I did this in lines 221 through 230 in my code in `advanced_lane_detection.py`

I implemented this step in lines 234 through 270 in my code in `advanced_lane_detection.py` in the function `impose_Lane_Area()`.  

![alt text][image6]

---
The link to the video is provided on the root project foldter of github.

Here's a [link to my video result](./project_video_generated.mp4)

