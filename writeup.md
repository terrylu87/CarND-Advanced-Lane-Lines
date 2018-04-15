## Writeup

### writeup

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

[image1]: ./output_images/undistorted.png "Undistorted"
[image2]: ./output_images/undistorted_img.png "Road Transformed"
[image3]: ./output_images/wraped_bin.png "Binary Example"
[image4]: ./output_images/wraped_color.png "Warp Example"
[image5]: ./output_images/lane.png "Fit Visual"
[image6]: ./output_images/outimg.png "Output"
[video1]: ./output_videos/project_video.mp4 "Project Video"
[video2]: ./output_videos/challenge_video.mp4 "Challenge Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The example code had already given the objpoints and imgpoints. I just use the for loop to run the cv2.calibrateCamera() function on every calibration image. Then applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 


![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

After a lot of trails and errors, I found it is easier to get robust binary image after prespective transform. So in my pipeline, I move this step after perspective transform. By combining gradx and the s channel of hls color space, I get a resonable binary output. Here's an example of my output for this step.

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

I chose the hardcode the source and destination points in the following manner:

```python
src=np.float32([[550,480],[735,480],[1090,720],[190,720]])
dst = np.float32([[190,0],[1090,0],[1090,720],[190,720]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I use the histogram peak to find an initial search point. And use sliding window search to find the lane pixels. Then fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I follow the sample code given in class to calculate the curvature. Since the estimated meter per pixel looks resanable, I use the value to calculate the offset from the center.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I use the inverse prespective transform to draw the lane on the undistorted image.

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I draw every step in a single frame. So I can see why my pipeline fails on the challenge and harder challenge video. In challenge video, there are servral line appears in the binary image due to sharp contrast between the lanes. Some smooth techniques are needed. I try to drop the undesirable lanes and use avrange lane width. It works, but still fail to survive the challenge track. However, the first project video looks ok.
I wish I could play around more, but I had to move on because the deadline of the final project is approching.



