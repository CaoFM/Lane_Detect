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

My pipeline consisted following steps. 
1. __Select color__ from image. Knowing the lane lines are either yellow or white, I used two color masks to select the pixels that are close to yellow or white.

1. __Select region of interest__ in the image.

1. Convert image to __gray scale__.

1. Apply __Gaussian filter__.

1. Find edges with __Canny edge detection__. I learned that I can get reasonable threshold to be used for the Canny edge detection parameter by taking the median value of the gray scaled image and standard deviation.  

1. Find all lines in __Hough Space__.

1. __Combine lines into 2 lane lines__. In order to draw a single line for each side of the lane, I modified the draw_lines() function into conducting the following steps.
   1. Calculate the __slope and offset__ (m,b, in y = mx + b) for each line
   1. Calculate each line's __intersecting point__ with the bottom of the image and the top of the 'region of interest'
   1. __Categorize lines__ into either __'left' or 'right'__ line of the lane, based on where the line intersects the bottom of the image.
   1. Take the __average__ of the bottom and top interseting X coordinate for the 'left' and 'right' line groups respectively and the result is the (X,Y) coordindate of the two ends of the lines that we should draw on the original image.
   1. Noticing that in the _optional challenge_, the left and right lane line crosses path at the top of our 'region of interest'. I re-calculate the (X,Y) of the far end of the lane lines, if they are going to cross each other in the final drawing.

1. Finally, __draw__ the lines on the original image.


### 2. Identify potential shortcomings with your current pipeline

Some shortcomings I anticipate:
1. The test image/video are generated when the host vehicle is in the middle of the lane. When host vehicle is not perfectly in the lane, the 'pipe-line' may have trouble clustering lines into one lane line
1. Lane lines are first filtered by color, the result of which may greatly affected by lighting condition.

### 3. Suggest possible improvements to your pipeline

1. The videos are currently processed on a frame to frame basis. In reality, a lane line does not disapear / shift abruptly. A better algorithm may take advantage of the continuous nature of lane lines to filter out noisy content of the image, or fill in the gap when lane detection is momentarily unavailable
