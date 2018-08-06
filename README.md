### Overview
In this project, we used computer vision techniques to identify lane lines on the road, first in an image, and later in a video stream (really just a series of images). 

### Reflection

#### 1. Describe your pipeline.

My Basic Pipeline consisted of 9 steps. 

**1.Convert the image to grayscale.**

**2.Apply gausian smoothing on grayscale image.**

**3.Apply canny algorithm to get edges image.** 

**4.Apply hough transform to get hough lines.**
Here however I modify the hough_lines method to filter and draw only those lines with positive slope starting from roughly 15 degrees and negative slope ranging from 90 to 150 degress .(This step was added in Challenge video as it contains lot of noise lines)

**5.Mask the image to include area of image which would approximately contain the lane lines**

**6.Now to segregate between left and right line I need find left set of points and right set of points .**
First to find left points I start to loop over each pixel from centre of image to left end of image to save all left points in leftLineXPts and leftLineYPts.
Secondly to find right points I start to loop over each pixel from centre of image to right end of image to save all right points in rightLineXPts and rightLineYPts.
Now I have saved seperate left points and right points 

**7. Now apply linear regression to left points to get left fitting line's slope and intercept .**
Then apply linear regression to right points to get right fitting line's slope and intercept .

**8. Now draw lines using above slope and intercept choosing masked area vertices as enpoints for the line .**

**9. Finally filter out the hough lines to keep only the lane lines in the image .**
 

This pipeline worked fine on white and yellow video but in the case of challenge the hough lines detected were missing right hand side lane in few frames .
So I came up with the idea of also added points found in previous frame wot find regression line .
Therefore after point 7. **I add previous points to found points and then find regression line and in the end of each frame I update previous left and right points if current frame found left and right points** . This improved my output on challenge problem but it is still not optimal and more sofiticatted strategy is needed.



#### 2. Identify potential shortcomings with your current pipeline

Potential shortcoming would happen when 

1. If Image is not centred .
   Currently my algorithm is completed based on finding left and right points based on the centre of the image .
   If the image is shifted my algorithm will break to find the lines points correctly . 

2. If slope of noice lines lies in the filtered range .
   Currently I filter lines based on the slope which can break with more complicated image .

3. If lanes are too close .
   Current approach will fail in the case of close lines as I will not be able to get left and right points clearly .

#### 3. Suggest possible improvements to your pipeline

1. Change method of detection of left and right lanes 

2. Currently I only take previous frame to improve number of points to get the regression line . 
   To improve we can avrage out more frame but this strategy might fail for sudden changes in picture like turns .
