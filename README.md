## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

### Running Instructions:
`CarND_Advanced_Lane_Finding.ipynb` contains the full workflow.

### Output Videos:
1. `project_video_output`
2. `challenge_video_output`
3. `harder_challenge_video`

### Camera Calibration
____________________
#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.
______________________

##### Step 1: EDA (Cell 2)
This step takes care of the shape and structure of the image.

##### Step 2: Camera Calibration (Cell 2)
This step obtains camera matrix, distortion coefficiencts which we ran over all calibration images and passed it through the `cv2.calibrateCamera()` function.

##### Step 3: Distortion Correction (Cell 4,5)
Undistort the calibration image for testing using `cv2.undistort`.


### Pipeline (single images)
____________________

#### 1. Provide an example of a distortion-corrected image.
____________________

##### Step 3: Distortion Correction (Cell 6)
Undistort the test image for testing using `cv2.undistort`.

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.
____________________
##### Step 4: Sobel, Threshold and Color Transform (Cell 7,8)
###### Performing general image processing functions:
1. Undistortion
2. Grayscale
3. Gradient Threshold
4. Directional Threshold
5. Combination
6. Region of Interest

In 3,4,5, after experimenting with the challenge and harder video, had to introduce R, G and L thresholds.
In 6, after experimenting with all the test images, found appropriate vertices and are of interest.


#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.
____________________
##### Step 5: Perspective Transform (Cell 9,10)
1. Function performs perspective transform
2. Raw co-ordinates entered manually for the transformation


#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
____________________
##### Step 6: Lane Line Pixels for Image (Cell 11,12,13,14)
1. Found the peaks using histogram.
2. Used sliding window - Sliding window search using the histogram peaks as reference for the 2 lanes. Used 9 windows of width 100 pixels.
Polynomial (rectangle) is formed and lane lines are drawn accordingly.
3. Sliding Window Search using prior Information - Consecutive frames have lane lines very close to each other, so we use a margin of 50 pixels within the sliding window


#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.
____________________
##### Step 7: Radius of Curvature and Offset (Cell 15)
Finding the curvature of the road and the position of the car from the center

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.
____________________
##### Step 8: Inverse Transformation (Cell 16)
Converting the warped image to normal and combine with original image to form the lane over the image.

##### Step 9: Final Pipeline for Video (Cell 17)
1. Complete Pipeline for images.
2. restart_lane_finder() - This function restarts the lane finding from scratch if there are no values in leftx(y)\_predictions and rightx(y)\_predictions for frames.
3. smoothening_lane() - This function perfomes smoothening of values by averaging over previous 12 frames.

##### Step 10: Testing Final Video Pipeline on Test Images (Cell 18)
1. Running the pipeline on test images.
2. End-to-End solution architecture established on images.

---

## Pipeline (video) - Cell 19,20,21,22,23,24,25
____________________
#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).


____________________
1. [Project Output Video](./project_video_output.mp4) - Cell 20,21
2. [Challenge Output Video](./challenge_video_output.mp4) - Cell 22,23
3. [Harder Challenge Output Video](./harder_challenge_video_output.mp4) - Cell 24,25

#### NOTE: The harder challenge video is not that great
---

## Discussion
____________________
#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?
____________________
#### Challenges:
#### Gradient & Color Thresholding during lighting conditions
1. Due to different lighting conditions and shadows, video frames kept distorting so created enhance_img() to obtain a sharper image.
2. The code wasn't able to cope up with the challenge and harder challenge videos, so I made a R, G channel thresholding and L channel thresholding.
#### Frame Quality
3. The shadows and underlighting caused the lines to jump, so created smoothening_lane() to use the parameters from previous n frames and average them.
#### Lane Pattern Changes:
1. Lane lines changes, white symbols in the centre of the road - The frame smoothening took care of that challenge.
2. In the harder video, the lanes curve to quickly - Manually changed vertices for the harder challenge video.
##### Drawbacks:
1. The code is not very robust to wide angle turns on the road, specifically the harder challenge road. 
2. It doesn't perform very well in drastic lighting conditions.
##### Future Enhancements:
1. Adjustment of the brightness based on the intensity to create artificial lighting or darkness during opposite occuring conditions.
2. Using the curvature of the road as a parameter to shift the lane finding vertices, region of interest in order to gain accuracy.

## LICENSE:
This project has been open-sourced basedon on MIT License. All Developers are free to use my code or make pull requests without any issue.
