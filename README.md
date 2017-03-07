#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)


Creating a Great Writeup
---
For this project, a great writeup should provide a detailed response to the "Reflection" section of the [project rubric](https://review.udacity.com/#!/rubrics/322/view). There are three parts to the reflection:

1. Describe the pipeline

2. Identify any shortcomings

3. Suggest possible improvements

We encourage using images in your writeup to demonstrate how your pipeline works.  

All that said, please be concise!  We're not looking for you to write a book here: just a brief description.

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup. Here is a link to a [writeup template file](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md). 


The Project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to get started on this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

You will complete the project code in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out <A HREF="https://www.packtpub.com/books/content/basics-jupyter-notebook-and-python" target="_blank">Cyrille Rossant's Basics of Jupyter Notebook and Python</A> to get started.

Jupyter is an Ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  Follow the instructions in the notebook to complete the project.  

**Step 3:** Complete the project and submit both the Ipython notebook and the project writeup

## List of files submitted by Nov05

1. README.md (writeup, including project instructin, file list, pipeline description, shortcoming description, improvement suggestion, etc., can be found in this file)
2. P1.ipynb (code and .jpg test result pictures can be found in this file)
3. P1.html (if it is downloaded with the .mp4 files, the annotated videos can be viewed in this file)
4. Picture files ending with _01.jpg, _02.jpg (test pictures with annotation)
5. white.mp4, yellow.mp4, extra.mp4 (the test result videos)

## Pipeline description by Nov05
### The pipeline contains 6 steps:
1. grayscale() - change a color picture into a grayscale one.
2. gaussian_blur() - use Gaussian Blur reduce noises.
3. canny() - use Canny function to detect edges in the picture.
4. region_of_interest() - apply a quadrilateral mask to the picture to locate the lane line edges.
5. hough_lines() - apply Hough transform to draw lane lines on a blank the same size as our image.
6. weighted_img() - add the lane lines to our original image.

### There are 3 stages to build the pipeline:
1. lane_detection() - make sure the lane lines can be detected and marked with line segments.
2. lane_drawing() - change the draw_lines() to draw_lines_average(), in which the line segments are averaged/extrapolated to map out the full extent of the lanes. 
* 2.1 In this step, I used position and slope to differenciate the segments into two groups: one for the left lane, the other for the right lane. If the start point and the end point's x-coordiates are less than (picture width/2 + 20), the slope is in range (-1, -0.6), the segment belongs to the left lane; if the start point and the end point's x-coordinates are larger than (picture width/2 - 20), the slope is in range (0.5, 1), the segment belongs to the right lane.
3. Improve the draw_lines_average() function to get better results.

### Once finished the pipeline, apply it to the test videos, and keep turing parameters and the draw_lines()/draw_lines_average() function to get better results.

## Shortcomings identified by Nov05
1. The mask area is fixed. If the picture angle changes, the mask area will need to be changed mannually.
2. The lane line annotations seem shaky in the video. The position and slope change from frame to frame. I have tried different ways to reduce the changes: use mean of the slopes of the line segements, or use median value, or add different weights to different line segments (the longer its slope weights the more)... For the final result I submitted, I chose to use average of coordinates of start points and end points of the segments, to get a start point and end point, then extend it to a full length in the mask area. 
3. There are noise segments. I tried to limit the area and slope range to reduce the noise, for example, for the left lane, only segments on the left (x1, x2 < (picture width / 2 + 20)) and slope in range (-1, -0.6) would be used to draw the full lane.

## Possible improvement suggested by Nov05
1. I might be able to use the position and slope from the last frame to adjust both in the current frame, to reduce the shakiness of the annotaions.
2. The pipeline doesn't work well in processing the challenging video, in which the lanes bent more, and there are more dynamics of the lane lines.
