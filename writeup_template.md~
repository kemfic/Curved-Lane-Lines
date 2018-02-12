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

[image1]: ./output_images/undistort.png "Undistorted"
[image2]: ./output_images/undistort_.png "Test undistortion"
[image3]: ./output_images/filter_warp.png "Perspective warp and color/gradient thresholding"
[image5]: ./output_images/pipeline.png "Full Pipeline"
[image6]: ./output_images/draw_lines.png "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "P4.ipynb". The Calibration & Undistortion pipeline uses the undistort_img() and undistort() functions.

I used `objpoints` and `imgpoints` outputs from cv2.findChessboardCorners to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I combined gradient and color thresholded images to generate a binary image. The code for this can be found under the pipeline() function in P4.ipynb

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform can be found under a function called perspective_warp(), in P4.ipynb. It receives an input image, an src array containing the coordinates for the initial shape, and an array called dst which contains the destination points. Using the cv2.warpPerspective function, we can effectively stretch the src points to the dst coordinates, and use the transformation to get a bird's eye view of the image.

```python
# The src and dst arrays contain float values that represent the coordinates with respect to the image size. We have more flexibility with input images w/ different sizes
src=np.float32([(0.43,0.65),(0.58,0.65),(0.1,1),(1,1)]),
dst=np.float32([(0,0), (1, 0), (0,1), (1,1)])


img_size = np.float32([img.shape[1], img.shape[0]])
dst_size = (1280, 720)

src = src * img_size
dst = dst * dst_size
```


I verified that the warp worked properly by outputting the warped image alongside the original image, and adjusted my src and dst coordinates to get evenly spaced lane lines.
![alt text][image3]
#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I then used the sliding_window() function to apply a sliding window algorithm to detect the lane lines. The sliding window algorithm uses restricted regions of interest that can be used to detect lines within the image. The initial position is determined by finding the 2 maxima on a histogram of the image. As it detects clusters of pixels, it stores the detected pixels for later use and computes the mean of the clustered pixels to determine the starting position for the next window. Once the entire image has been scanned with the sliding window algorithm, we then use the detected pixels to fit a 2-degree polynomial into a curve that represents lane lines.

![alt text][image4]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the get_curve function in P4.ipynb. 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.


![alt text][image5]


### Complete Pipeline

You can see the image showing the entire pipeline below:

![alt text][image6]
---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I had a lot of issues with zones that had sharp turns, shadows, and changing textures. The color and gradient thresholding was not sufficient in keeping the program from inaccurately labelling the lane lines. In order to prevent this, I applied a smoothing operation by getting a rolling average of the curve parameters. This way, the curve couldn't change too rapidly by averaging that frame's detected curve with that of previous frames.

My pipeline doesn't work well under shadows, very sharp curves, and areas with rapidly changing texture.
