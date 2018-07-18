# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Workflow for the pipeline :


1. Converting the image in HSV rather than grayscale because all the color information is encoded in single channel H.
2. Removing any noise from the image using the opencv library's gaussianBlur function.
3. Masking the yellow and white lane lines using cv2.InRange based on the threshold for yellow and white. 
4. Apply canny edge detector based on the masked yellow and white image. 
5. Calculate the vetrices which are used in finding hough lines 
6. Apply probalistic Hough Lines to get the set of points in region of interest that might create an edge according to the parameters specified

I make modification on the draw_lines() where i apply three separate lines. ( One line defin the left side and find the lane and colored which not depend on the horizon but on the line on the road, other same but on the right side, and extrapolated line depend on the highest lane in horizon)


To be able to trace a full line and connect lane markings on the image, we need to distinguish left from right lanes. 
- left lane: as x value (i.e. width) increases, y value (i.e. height) decreases: slope must thus be negative
- right lane:as x value (i.e. width) increases, y value (i.e. height) increases: slope must thus be positive

From the array of lines returned by HoughLines we iterate and calculate the slope and intercept which are stored in separate arrays based on if the slope is positive or negative, if the slope is greater than the threshold with which we are cleaning the noisy lines which are not relevant.  

As i have all the information stored about the y_max and y_min , slope and intercept based on that liner equatation we can calculate the x_min and x_max.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming is because of extrapolation , if we have a car in front of us and cover one ( drive little on right or left) filled lane so the algorithm for extrapolating the lane will cover the car like there is a open road, also straight lines do not work when there are curves on the road.


### 3. Suggest possible improvements to your pipeline

As this algorithm treat each frame as separate image on which apply finding lane algoritham , improvments can be made in draw_lines() function to use information from previous frames. 