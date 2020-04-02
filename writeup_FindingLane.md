# **Finding Lane Lines on the Road** 

## Writeup For Finding Lane Lines

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


---

### Reflection

### 1. Description of the pipeline.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I have used Gaussian filter for smoothing of the gray scale image. I have used kernel size 3 for the Gaussian filter. 

Then in the third step, this smooth gray image is passed to the Canny edge detection algorithm to get the edges in the entire image. Canny edge detection algorithm already internally does Gaussian smoothing with kernel size of 5. But I wanted to apply further smoothing to achieve better results. 

As the fourth step, I am only focusing on the region of interest as the lanes will always be in particular region in the image. For this, I am using quadrilateral polygon to extract the region of interest. The details of vertices are provided in the code. 

In the final step, I am applying Hough tranform to the edges image with region of interest to get the lines image.
For this, through parameter tuning, I could get the best result using below Hough transform parameters.

--rho = 2 # distance resolution in pixels of the Hough grid--
--theta = np.pi/180 # angular resolution in radians of the Hough grid--
--threshold = 10     # minimum number of votes (intersections in Hough grid cell)--
--min_line_length = 33 #minimum number of pixels making up a line--
--max_line_gap = 2    # maximum gap in pixels between connectable line segments


The Lines image then passed to the draw_lines() function to ultimately draw the final lines which represent lanes.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging the lines using slope and intercept and calculated the final line coordinate for both the lanes.

The resulted single line image is then annotated on top of the original image by using the cv2.addWeighted() function. This gives us output images showing lane lines being identified.   

To see the output of the pipeline, I would like to encourage you to check the source file.  



### 2. Potential shortcomings with the pipeline


This pipeline doesn't work on the curved lanes. 
Moreover, there are certain images where pipeline will get zero number of line as the lane marking is sometimes so small it doesn't meet the criteria passed to Hough transform to detect the line. If we reduce the criteria to include the small lane markings then you can get lot of false positives in lines and this will hamper the accuracy of the pipelie. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to somehow tune the parameters in such a way that there will not be rise in number of false positive of lane lines and still small lane marking would be identified as a valid lane lines. 
