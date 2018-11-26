# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

## Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project we will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

The main goal of this project is to design a pipeline which will find the lane lines on the road easily. In the end I have designed a LaneDetection class to wrap the pipeline to detect the lanes in the videos.

## Pipeline
In this section I will cover the steps in detail to create the pipeline, which will hep us to finding the lane lines in the images. Then I will save the output images.The steps to achieve the result is described below:



- Convert the image to `grayscale` for easier manipulation
- Apply `Gaussian blur` to smoothening the edges
- Apply `Canny edge detection`
- Trace the `region of interest` and discard rest of the regions
- Perform a `hough transform` to find the lane lines within the `region of interest`and color them red
- Seperate `left` and `right lanes`
- `Extrapolate` them to create two smooth lines

### Convert the image to `grayscale` for easier manipulation


Here we are going to detect white or yellow lines on images, which show a high contrast if the image is converted to grayscale. In grayscale mode road is black so anything brighter than will come out with high contrast.


![gray scale images](./test_images_output/grayscale_image)

### Apply Gaussian Blur
Gaussian Blur is a pre-processing technique to smoothen the edges of an image. In OpenCV Gaussian blur takes a odd number of integer kernel_size. For this project I choose the Kernel size 5 by trial & error.

![Gaussian Blur Images](./test_images_output/gaussian_blur)

### Canny Edge Detection
After successfull completion of Gaussian Blur, I have applied Canny Edge Detector. Canny edge detection is a powerful algorithm to identify the edges in an image. In OpenCV we need to pass two arguments to low_threshold & high_threshold in addition to the gaussian blurred images.
I have set the low & high threshold 50 & 150 respectively.



### Region of Interest
After the edge detection our job is to identify which are the road lanes among all the edge detected. So wee need to define a region of interest to identify the lanes. By looking at the images I guessed & defined a polygon with the help of a function I defined. 


![Canny Images](./test_images_output/canny_images)



### Hough Transform
After the Canny edge detection our job is to perform a Hough Transform to extract the lines present on the images & color them. Hough Transform will find lines by identifying all points lie on them. This is achieved through converting normal co-ordinate system to a polar co-ordinate system.

![Hough Lines](./test_images_output/seperated_lanes)


### Left & Right Lane Seperation
To be able to detect the lanes properly I have seperated left & right lanes by following below principles:

- Left lane slope should be negative
- Right lane slope should be positive

Then I defined a function which can detect the left & right lanes properly

In the following images I have color coded the left & right lanes

![Seperate Lanes](./test_images_output/seperated_lanes)

### Line Extrapolation

To detect a full line from the bottom of the screen, it should be able to interpolate the different points returned by the hough transform function. I have attempted to find the line by minimizing the [least squared error](https://en.wikipedia.org/wiki/Least_squares). I have imported the `stats` module from `scipy`. I got the below images as output:

![final output](./test_images_output/final_output)

## Test on Videos

Three vidoes are provided here. My pipeline is successfully running on the 10 & 27 seconds version. My implementation is working great on the first two videos but not working fine on the challenge vidoes where curves are present. As a video is a sequence of frames I used the information from previous frames to smoothen the lines. I used standard _deque_ to store the last 15 computed line coefficients. It is working very well but failing on the optional challenge video.

## Shortcomings

I have faced challenges initially in performing the Hough transform. I find it tricky to set the parameters manually. In the challenge video my pipleline is working fine as log as there are straight lines but failing once the curve lines are coming

## Future Improvements

I beileve parameter tuning of every function can be possible done dynamically. Also Deep Learning can be implemented to identify lane lines.
