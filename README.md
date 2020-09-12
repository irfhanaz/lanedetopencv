# lanedetopencv
## **Investigating the Accuracy of Real-Time Lane Detection using OpenCV**

According to Wikipedia, *"A lane is part of a roadway (carriageway) that is designated to be used by a single line of vehicles, to control and guide drivers and reduce traffic conflicts."*

![](https://github.com/irfhanaz/lanedetopencv/blob/master/inputimg.png)

Whilst driving, human drivers perform the comman task of identifying lanes to ensure their vehicles are within the lane constraints while driving. As we know, this prevents collisions and other accidents on the road. When faced with self-driving or autonomous cars, there must be a model for the vehicle to detect these lanes itself. This is where **Lane Detection** comes into play. 

We will be using OpenCV, an open source computer vision and machine learning software library, which contains over 2500 optimized algorithms to detect and recognize faces, identify objects, classify human actions in videos, track camera movements, track moving objects, extract 3D models of objects, etc.

In this rudimentary model to detect lanes, three major concepts need to be understood:
(1) Finding the Region of Interest
(2) Using Hough lines
(3) Drawing the Lines
(4) Image Preprocessing

### **Finding the Region of Interest:**

In real life, the road is very rarely without obstacles, whether it be other vehicles or road signs. When applying real-time lane detection, we'll need to be able to ignore all objects in our frame, leaving only the lane line markings. To do this, we must narrow down the region of interest to include only the most necessary portion of the frame. We can do that using frame masking as seen below:

![](https://github.com/irfhanaz/lanedetopencv/blob/master/frame_mask_applied.png)

A frame mask is a NumPy array where the pixel values in certain cells are set to 0 or 255. It is a simple but effective way to remove unwanted objects from our frame and to narrow down on region of interest in easy test cases.

### **Probabilistic Hough Transforms**

The Hough Transform is a technique to detect any shape as long as a mathematical representation of the shape exists. In Lane Detection, we’re looking for *lines*!
Line Representation: ⍴ = xcosθ+ ysinθ
⍴ - perpendicular distance from origin to the line
θ - the angle formed by this perpendicular line and horizontal axis measured in counter-clockwise

Probabilistic Hough Transform is an optimization of the Hough Transform. Instead of taking all the points in the image and incrementing the cells of the accumulator array as it iterates through the points, it takes only a random subset of points. Its parameters allow for it to return an array of the endpoints of each line it detected in the image.

##### **Function: *cv2.HoughLinesP()***
*Parameters*
1. Binary Image (obtained through use of canny edge detection)
2. ⍴ accuracy
3. θ accuracy
4. Threshold - min. vote to be considered a line ie. min. length of line
5. minLineLength - Minimum length of the line. Line segments shorter than this are rejected.
6. maxLineGap - Maximum allowed gap between line segments to treat them as a single line.

### **To Draw the Lines onto the Image:**
*Function parameters:* the image and the lines array
1. First make a copy of the given image
2. Then create a blank image of the same size of the original image
3. Iterate through each line in the lines array to draw the lines on the blank image
4. Merge the copy of the image and the “blank” image into the final image
5. Return final image

![](https://github.com/irfhanaz/lanedetopencv/blob/master/img_with_lines.png)

### **Image Preprocessing**

Now that we've created functions for an individual image, we can perform real-time lane detection by utilizing the VideoCapture function from the OpenCV library. We can loop through each individual frame to process it using the functions. Then, we can combine the frames to display a video to see the results of the real-time lane detection using OpenCV. 

### **TEST CASE #1:**
![](https://github.com/irfhanaz/lanedetopencv/blob/master/inputvid.gif)
### **Output:**
![](https://github.com/irfhanaz/lanedetopencv/blob/master/lanedetoutput1.gif)
### **TEST CASE #2:**
![](https://github.com/irfhanaz/lanedetopencv/blob/master/inputvid%20(1).gif)
### **Output:**
![](https://github.com/irfhanaz/lanedetopencv/blob/master/lanedetoutput4.gif)
### **TEST CASE #3:**
![](https://github.com/irfhanaz/lanedetopencv/blob/master/inputvid%20(2).gif)
### **Output:**
![](https://github.com/irfhanaz/lanedetopencv/blob/master/lanedetoutput2.gif)

### **Outcome**

Although the output for test case #1 worked fairly well, the same cannot be said for test cases #2 or #3. It's clear that hardcoding the frame masking technique to detect narrow down the region of interest isn't effective if there are still other objects within your region of interest. Moreover, it begs the question for lane detection when there are curves and disjointed lane lines due to the test of time. While OpenCV works well for straight pathways without any obstacles too close to it, through our outputs, we can see that a much more advanced line detection technique is needed. 

### **What Next?**

Research on Advanced Line Detection Techniques and Implementation of a Model where:
1. Detect exactly two lane lines, i.e. the left and right lane boundaries of the lane the vehicle is currently driving in.
2. Not detect adjacent lane lines
3. The vehicle must be within a lane and must be aligned along the direction of the lane
4. If only one of two lane lines have been successfully detected, then detect the second line by implementing the lane approximation function
