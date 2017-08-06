# **Finding Lane Lines on the Road** 
## Writeup

[//]: # (Image References)
[challenge_frame]: ./test_images/challenge_frame.jpg "Problematic challenge video frame"
[challenge_frame_preproc1]: ./writeup_images/challenge_frame_preproc1.png "Pre-processing with grayscale and Gaussian smoothing"
[challenge_frame_preproc2]: ./writeup_images/challenge_frame_preproc2.png "Pre-processing with gamma adjustment"
[annotated]: ./writeup_images/InkedwhiteCarLaneSwitch_annotated.jpg "Problematic line"

### Introduction

This project is part of Term 1 in Udacity's Self-Driving Car Nanodegree Program. 

The goal is to use some basic computer vision algorithms to build a pipeline able to find lane markings on the road. This pipeline shall be able to process a car front-view image, finding the markings of the lane being driven, and returning the same image with the lane lines evidently marked.

The pipeline shall then be tested on:
* 6 test images
* 2 test videos
* 1 optional more challenging test video

This Writeup includes a description of the pipeline, challenges encountered, solutions tried, and a reflection on potential improvements.

### Image processing

#### Original pipeline

The image processing pipeline consisted, originally as per project template, of the following steps:
1) Grayscaling
2) Gaussian smoothing
3) Canny edge detection
4) Interest region masking
5) Hough line detection

#### Task: extrapolate lane lines from Hough lines

Other than adjusting the parameters of the used algorithms, the instructions were to enhance step 5 of the pipeline as to extrapolate the location of the left and right lanes, instead of simply drawing the pieces of them that could be found using the Hough transform. To that end, I have inserted an extra function call within the *hough_lines* function (actually a copy named *hough_lines2*) to extrapolate lane lines from the Hough output lines, before the lines are drawn onto a blank (i.e. black) image.

Here's how the lane extrapolation works:
1. I iterate through the Hough lines, rejecting the ones with an implausible slope, and assigning the others to either the left or the right lane, also based on slope.
2. I fit a straight line, or 1st degree polynomial, to each set of points, i. e. left and right.
3. I define one of the endpoints of each lane to be the bottom of the image, and the other such that there is an arbitrary gap between the lines on the far end.

After extrapolation, the two lane lines are fed to the *draw_lines* function to be drawn on a blank image as before.

#### Testing and adjusting parameters

The test images revealed where the pipeline was working and where not. Through several iterations, I have loosened the region of interest mask and the slope filtering of the Hough lines, to prevent rejecting useful data points. On the other hand, I have adjusted the parameters of the Canny and Hough algorithms, as to improve noise rejection. In the end, my pipeline was capable of handling all the test images and the two mandatory test videos.

#### "Challenge" video

The challenge video was troublesome with the pipeline described above, as the algorithms were simply unable to find the left lane at a certain point. For troubleshooting, I have extracted a sample frame from the problematic part of the video.

![alt text][challenge_frame]

As you can see, the car drives through a short patch of road with a very light-colored surface and dark smudges. I then ran this frame step-by-step through the pipeline. The left lane is already difficult to recognize on the original image, and it was even more difficult after steps 1 and 2. Although grayscaling and Gaussian smoothing do well in rejecting noise, they do more harm than good when it comes to contrast, which is the main issue here.

![alt text][challenge_frame_preproc1]

That's where I tried the solution of a former nanodegree student, using gamma correction for image pre-processing instead. Credits to https://github.com/muddassir235. As you can see on the image below, it does a much better job at highlighting the lane lines and rejecting image noise.

![alt text][challenge_frame_preproc2]

With that change I was then able to find the lane lines in the challenge video.

#### Final pipeline

With the lane extrapolation and the borrowed image pre-processing described above, my final pipeline was the following:
1. Gamma adjustment
2. Canny edge detection
3. Interest region masking
4. Hough line detection
5. Lane line extrapolation

### Potential shortcomings

Although my image processing pipeline was able to "pass the tests", it has clear room for improvement in robustness and stability. For robustness of the algorithm, the first logical solution is to further tune the Canny and Hough parameters. When it comes to stability of the lane line, maybe a kind of low-pass filter would help correct the observed "jumpy" behaviour.

I have also experimented with outlier removal as the Hough lines are attributed to the right and left lanes. My approach was based on slope and the y-intercept of the Hough lines, but it turned out to not be much help. Maybe an iterative approach based on absolute distance between the fitted line and the data point would yield better results.

### Conclusion

The developed pipeline proved capable of finding the lane lines on all given test images and videos. I say "good enough for now", and I will let the teachers surprise me on how to build on these capabilities.
