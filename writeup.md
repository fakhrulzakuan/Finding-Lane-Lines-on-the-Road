# **Finding Lane Lines on the Road** 

## Writeup Template

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/1_grayscale.jpg "Grayscale"
[image2]: ./test_images_output/2_blur.jpg "Gaussian blur"
[image3]: ./test_images_output/3_canny.jpg "Canny"
[image4]: ./test_images_output/4_region.jpg "ROI selected"
[image5]: ./test_images_output/5_masked.jpg "Masked"
[image6]: ./test_images_output/6_hough.jpg "Hough Lines"
[image7]: ./test_images_output/7_final.jpg "Final Output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of # steps.

Step 1: Converted the image to grayscale.

![alt text][image1]

Step 2: Applied the Gaussian blur.

![alt text][image2]

Step 3: Applied the Canny to get the lane edges

![alt text][image3]

Step 4: Applied ROI mask to ignore the unimportant region for our next filter.

![alt text][image4]
![alt text][image5]

Step 5: Used Hough to connect pixel dots of the lane edges and create lines.

![alt text][image6]

Step 6: Based on line equation y = mx + c ; m=gradient (slope) , c = y-intercept. 
I separated left and right lanes based on the polarity of the gradient of the lines created from the Hough.
If the gradient is above 0 then the lines are from the right lane, else the lines are from left lane. 

Step 7: Then I appended left and right lines to the respective array (list) together with each line y-intercept and gradient.
[x1 , y1, x2, y2, gradient, y-intercept, line-length]

Step 8: Obtained the average value of the lines gradient and y-intercept but put weight on the line length.  

Step 9: Draw solid line based on the average gradient and y-intercept. The line must start from the bottom of the image, therefore, the y1 must be the maximum number of image pixel for the Y axis.
With known value of y1, m and c, I calculated x1 = (y1 - c)/ m

For x2 and y2, y2 is the minimum y pixel of the image ROI. 
Again, x2 is calculated using line equation. 

Step 10:

With both coordinates, a new solid line are drawn to indicates the position of the left and right road lanes.

![alt text][image7]

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be when in a situation where there is a sharp road curvature since the solid lines are drawn based on 1st order of linear equation. In this case, the said equation is not suitable to draw line on curve lane. 

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to keep the lines drawn from previous frame to avoid flickering between frames. 
The lines drawn should be more smoother for each frame transition.
