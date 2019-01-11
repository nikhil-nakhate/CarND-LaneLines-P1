# **Finding Lane Lines on the Road** 

---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./pipeline_stages_images/solidYellowCurve2_edges.jpg "Edges"
[image2]: ./pipeline_stages_images/solidYellowCurve2_masked.jpg "Masked Edges"
[image3]: ./pipeline_stages_images/solidYellowCurve2_hough.jpg "Hough"
[image4]: ./pipeline_stages_images/solidYellowCurve2_final.jpg "Final"

---

### Reflection

### 1. Description of pipeline.

My pipeline consisted of 4 main steps.

The following are the stages with a short description and images that to better demostrate the action performed

#### 1. Canny Edge Detection:

In this step I first convert the image to grayscale. I then perform gassian blurring on the image to get rid of small amouts of noise and smooth the image. I then use the Canny edge detector to obtain the sharp edges in the scene.

![alt text][image1]

#### 2. Region of Interest Mask:

In the second step I apply a mask on the edge detected image that zeros out the image that wouldn't contain the lane lines. This will restrict the edges predominantly to the lane lines identified which would help in the further stages.

![alt text][image2]

#### 3. Hough Transform:

The Hough transform is generally used to detect well formed geometric shapes. In this project, it is used to detect straight lines from the already identified edges. The equation of the line is represented as: rho = x cos (theta) + y sin (theta).
The hough transform function takes various params and outputs a set of line segments. The below image shows these line segments superimposed on the original image.

![alt text][image3]

#### 4. Weighted Average of line segment slopes and intercepts:

The output of the Hough transform is a set of line segments which are parts of the lane lines in the image. But the task is to detect the lane line in its entirety rather than in parts. Also there is a slight amout of error which causes certain lines to have slightly different slopes rather than being idetified as a single line segment. To tackle this, I take a weighted average of the various line segments, by dividing it into two sets of positive and negative slopes. The weights are the lengths of the segments.

![alt text][image4]


The only change to the draw_lines() function was that it now returns an image with the lines drawn on a 2D array of zeros of the size of the original image. This is then used as an input to the weighted_img function which can then be used along with the image to modify the weight of the lines and the original image.

There is also a change made to the hough_lines() function and it now returns the actual lines rather than an image with lines drawn on it.


### 2. Shortcomings with my current pipeline


One potential shortcoming would be what would happen when the road is curved. Currently I am joining the point on the bottom of the image with the point that is identied above from the hough lines and using average slope of hough lines. This doesn't tackle curved lines.

Another shortcoming could be that due to the error introduced in the various stages, there is a sudden change in the slope of the lane lines identified, albeit quite small. But this still needs to be addressed.


### 3. Possible improvements to your pipeline

A possible improvement would be to fit a spline of higher order in order to tackle curved roads instead of using average slope and a line

Another potential improvement could be to use a sliding window of 5 or 10 frames so that the fluctuation in the detection will be minimized.
