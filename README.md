
Naturally Unintelligent Writeup


# Advanced Lane Finding Project

__The steps of this project are:__
 - Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
 - Apply a distortion correction to raw images.
 - Use color transforms, gradients, etc., to create a thresholded binary image.
 - Apply a perspective transform to rectify binary image ("birds-eye view").
 - Detect lane pixels and fit to find the lane boundary.
 - Determine the curvature of the lane and vehicle position with respect to center.
 - Warp the detected lane boundaries back onto the original image.
 - Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle
position.

__Rubric Points__
Camera Calibration
1. Have the camera matrix and distortion coefficients been computed correctly and
checked on one of the calibration images as a test?

The code for this step is contained in the second code cell of the P2.ipynb notebook 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the
world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are
the same for each calibration image. Thus, objp is just a replicated array of coordinates, and objpoints
will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.
imgpoints will be appended with the (x, y) pixel position of each of the corners in the image plane with
each successful chessboard detection.I then used the output objpoints and imgpoints to compute the camera calibration and distortion coefficients using the cv2.calibrateCamera() function. I applied this distortion correction to the test
image using the cv2.undistort() function and obtained this result:


__Pipeline (single images)__
1. Has the distortion correction been correctly applied to each image?
To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this
one:<br>

[image1]: ./test_images/test1.jpg "Original Example"
[image2]: ./writeup_images/test1_undist.jpg "Undistorted Example"

2. Has a binary image been created using color transforms, gradients or other methods?
Oooh, binary image... you mean like this one? (note: this is not from one of the test images)
3. Has a perspective transform been applied to rectify the image?
The code for my perspective transform is includes a function called warper() , which appears in lines 1
through 8 in the file example.py (output_images/examples/example.py) (or, for example, in the 3rd code
cell of the IPython notebook). The warper() function takes as inputs an image ( img ), as well as source
( src ) and destination ( dst ) points. I chose the hardcode the source and destination points in the
following manner:
src = np.float32(
[[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
[((img_size[0] / 6) - 10), img_size[1]],
[(img_size[0] * 5 / 6) + 60, img_size[1]],
[(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
[[(img_size[0] / 4), 0],
[(img_size[0] / 4), img_size[1]],
[(img_size[0] * 3 / 4), img_size[1]],
[(img_size[0] * 3 / 4), 0]])This resulted in the following source and destination points:
Source Destination
585, 460 320, 0
203, 720 320, 720
1127, 720 960, 720
695, 460 960, 0
I verified that my perspective transform was working as expected by drawing the src and dst points onto
a test image and its warped counterpart to verify that the lines appear parallel in the warped image.
4. Have lane line pixels been identified in the rectified image and fit with a polynomial?
Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:5. Having identified the lane lines, has the radius of curvature of the road been estimated?
And the position of the vehicle with respect to center in the lane?
Yep, sure did!

__Pipeline (video)__
1. Does the pipeline established with the test images work to process the video?
It sure does! Here's a link to my video result

__README__
1. Has a README file been included that describes in detail the steps taken to construct
the pipeline, techniques used, areas where improvements could be made?
You're reading it!

__Discussion__


Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might
fail and how I might improve it if I were going to pursue this project further.

Challenges overcome:
Initial camera cal showed some unusal distortion - Had the wrong size chess board!!! 6x9!!!
Spent a lot of time getting imwrite to work - not sure of route cause of this, think it was a combo of issues: concatenating pathnames, mixing up path and file names etc

Potential shortcomings, failure cases and future improvements:
I have written this as a function based pipeline. Speaking to a colleague who is also studying this course, he chose a class based approach, and I realised I don't fully understand the advantages of this, so will go away and read up about classes and discuss with him further.












## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)
![Lanes Image](./examples/example_output.jpg)

In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Check out the [writeup template](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup.  

Creating a great writeup:
---
A great writeup should include the rubric points as well as your description of how you addressed each point.  You should include a detailed description of the code used in each step (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  You should include images in your writeup to demonstrate how your code works with examples.  

All that said, please be concise!  We're not looking for you to write a book here, just a brief description of how you passed each rubric point, and references to the relevant code :). 

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup.

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `output_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

