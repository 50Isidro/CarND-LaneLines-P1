# **Finding Lane Lines on the Road** 

## Writeup

---

**Introduction and goals**

This project is part of Term 1 in Udacity's Self-Driving Car Nanodegree Program. 

The goal is to use some basic computer vision algorithms to build a pipeline able to find lane markings on the road. This pipeline shall be able to process a car front-view image, finding the markings of the lane being driven, and returning the same image with the lane lines evidently marked.

The pipeline shall then be tested on:
* 6 test images
* 2 test videos
* 1 optional more challenging test video

This Writeup includes a description of the pipeline and a reflection on shortcomings and potential solutions

---


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
