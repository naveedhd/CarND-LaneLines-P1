# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[lane_image]: ./test_images_output/solidWhiteCurve.jpg "Lane Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:

1. Colored image to grayscale
2. Gaussian Blur with kernel size of 5
3. Canny edge detector with low threshold of 50 and high threshold of 150
4. Region extraction by Trapezoidal masking
5. Hough lines with threshold of 50, minimum line length of 10 and maximum line gap of 50
5. Merging the original and line image


In order to draw a single line on the left and right lanes, I modified the draw_lines() function to:

1. Classify the left and right lane lines by slope.
2. For each left and right lane lines, calculate single line by averaging the x, y minimum and maximum points and slope.
3. Calcalate line of equation using the slope and one pair of points (x_min, y_min)
4. Using line equation, find the end points on lower and upper boundaries of mask region
5. Draw the line using these end points

Example of image after passing through the pipeline looks like following: 

![lane_image][image1]


### 2. Identify potential shortcomings with your current pipeline

1. I am using average for left and right lane lines. This will not work well when the lanes ahead are very curvy or there are other marking other than straight lanes are detected.

2. The masking region, specified by trapezoidal polygon is hardcoded, and assumes that the car is moving in the middle of lanes. As the car moves away from the middle, this can cause the lane marking to go out of the mask and hence not being detected. This hardcoded mask also assumes that the distance between lane markings is fixed.

3. Other general shortcoming of this approach is the assumption that lane markings will be present in all roads. However for self-driving vehicales, this is not a safe assumption as there might be roads without proper lane markings.

### 3. Suggest possible improvements to your pipeline

The current pipeline should be adapted to video as opposed to images so that the transient errors are filtered out.
