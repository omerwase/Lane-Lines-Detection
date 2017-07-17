# **Finding Lane Lines on the Road** 

## Submidtted by Omer Waseem on 16-07-2017

[image1]: ./test_images_output/hough_lines/solidYellowCurve.jpg "Hough lines on solid yellow curve"

[image2]: ./test_images_output/polyfit_lines/solidYellowCurve.jpg "Polyfit lines on solid yellow curve"

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I did not use the help functions (including draw_lines()) in order to get practice writing the whole code from scratch. I assumed that would be permissible. 

My pipeline consists of the following steps:
1. Convert image to grayscale
2. Blur image using GaussianBlur()
3. Find edges using Canny() edge detection
4. Create a 4 sided polygon mask using fillPoly()
5. Perform Hough transform on masked edge lines
6. Split Hough lines into left and right sides
7. For each side use polyfit() to estimate line of best fit
8. Add each line to the original image and return

I split left and right lines based on the middle of the image (max(x-axis)/2). By tuning parameters for HoughLinesP() I was able to obtain good representations of the lanes in the image. The image below shows these Hough lines in red.

![alt text][image1]

I then modified the function to split lines on each side into seperate arrays. Two of x values (for left and right side) and two of y values. These arrays were used in polyfit() to obtain a line of best fit for each side, as seen in the image below.

![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline

There are a number of shortcomings in my algorithm:

1) My algorithm functioned poorly on the challenge video. I believe this is mainly due to the application of HoughLinesP in combination with a 1-dimentional polyfit. 

2) To split Hough lines between left and right I used the middle of the image as the boundary. I believe this will not work well in practice since it assumes the car's camera is pointing towards the middle of the road.

3) Another reason for my algorithm functioning poorly on the challenge video is likely due to the car's hood being visible near the bottom. In general the mask applied in my pipeline is very specific to the positioning of the camera.

### 3. Suggest possible improvements to your pipeline

The improvements discussed below correspond to the shortcomings described above (numbered respectively):

1) One possible solution to identifying curved lanes would be to treat each Hough line as its own line and find a curved line for which the Hough lines are a tangent. I did not spend too much time on this since there is another project later in the course, which I believe address this issue.

2) Instead of spliting Hough lines based on the middle of the image, they can be split based on their slope. This should somewhat address the issue of the car's camera pointing away from the middle of the road.

3) The camera's position in the car will need to be fixed. If the hood of the car is part of the image it will need to be removed through the mask.
