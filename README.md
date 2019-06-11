## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image1]: ./examples/undistort_output.png "Undistorted"

[image2]: ./output_images/undist01.PNG "Undist01"
[image3]: ./output_images/undist02.PNG "Undist02"
[image4]: ./output_images/undist03.PNG "Undist03"
[image5]: ./output_images/undist04.PNG "Undist04"
[image6]: ./output_images/undist05.PNG "Undist05"
[image7]: ./output_images/undist06.PNG "Undist06"
[image8]: ./output_images/undist07.PNG "Undist07"
[image9]: ./output_images/undist08.PNG "Undist08"

[image10]: ./output_images/Gradiant.PNG "Gradiant Transformed"
[image11]: ./output_images/binary_combo_example.PNG "Gray Binary"
[image12]: ./output_images/warped_straight_lines.PNG "Warp Example"
[image13]: ./output_images/color_fit_lines.PNG "Fit Visual"
[image14]: ./output_images/CombinedS.PNG "CombinedS"
[image15]: ./output_images/Drawlane.PNG "Drawlane"
[image16]: ./output_images/gray_gradiant.PNG "Gray_Gradiant"
[image17]: ./output_images/LanedetectedWithMeters.PNG "LanedetectedWithMeters"
[image18]: ./output_images/Sbinary.PNG "Sbinary"
[image19]: ./output_images/HBinary.PNG "HBinary"
[image20]: ./output_images/Warped.PNG "Warped"


[video1]: ./project_video_solution.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The aim of this project was to detect the lane by overcoming real time issues like distortion in camera images. In real world images will have three dimension and image captured by camera will have 2 dimension.  

For this, initially I caliberated the camera by using chessboard images. It's like training ML algorithum. I converted 3D Image (x,y,z) to 2D (x,y). 3D images having object points to 2D images having imgpoints.
Using cv2.findChessboardCorners() I got conners of images which will be used to calibrate the image.
To calibrate camera I used OpenCV's existing lib function cv2.calibrateCamera(). I used the return arguments form the function to undistort the images.
I applied my undistort funtion on a chessboard sample and below output. 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
Below are the raw images and there output after undistortion.
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]


#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

After undistorting all the sample images, I applied different transform for example Sobel X, Magnitude and Direction transform on sample image, using some open CV  existing functions like cv2.Sobel(), arctan2() and so on.
The output of the different transformed images will look like as below.
![alt text][image10]
I used a combination of color and gradient thresholds to generate a binary image.

After this I also applied color transform on the color image.
First I applied gray binary transform and result from it was as below.
![alt text][image16]
I also applied Hue binary transform on the sample image.
![alt text][image19]
After applying saturation transform the result was displayed as below.
![alt text][image18]

After trying out different combination I could get a images in which the lane lines can be seen accurately.
![alt text][image14]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `transform_image()`.  The `transform_image()` function takes as inputs an image (`img`), as well as number of inside corners nx and ny. Source (`src`) and destination (`dst`) points are calculated inside the function itself .  I chose the hardcode the source and destination points in the following manner:

```python
    src = np.float32([[595,460], [280,700], [725,460], [1125,700])
    dst = np.float32([[250,0], [250,720], [1065,0], [1065,720]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image20]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
