# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./pipeline.jpg "Pipeline"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. 

1) Select white and yellow colors in the image using `cv2.inRange` 
2) Convert to grayscale.
3) Use gaussian blur.
4) Detect edges using Canny edge detection.
5) Select the region of interest in the image.
6) Find hough lines in the image and extrapolate them to get one line for the left lane and one line for the right lane.
7) Use `weighted_img` to draw the final output with transparency on the initial image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function using following steps:
1) split all the line points from hough lines detected into two sets (left lane points, right lane points) based on slope.
2) use `cv2.fitLine` to find a fit line through a points of a single lane.
3) use fit line slope and intercept to draw the lines from the bottom of the image to roughly the middle.

![Pipeline][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when there is a sharp turn, so the curve of the route is different.

Another shortcoming could be when it's not a highway but a village/city street.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to detect a region of interest automatically (for example using CNN). The wrong region of interest is one of the reason the default pipeline is not working for the challenge video.

Another potential improvement could be to reduce noise found in hough lines - some segments(points) do not belong to either of the lanes. I use very simple algorithm based on slope to reduce noise, but as the video shows it doesn't seem enough. Maybe Kalman filter could help here.

Another potential improvement could be to consider the previous frames in the pipeline - for example we could take weighted results based on previous frames, so the more blurry frames would still show somewhat valid results.
