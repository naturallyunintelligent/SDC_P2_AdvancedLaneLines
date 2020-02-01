
Naturally Unintelligent Writeup


# Advanced Lane Finding Project

__The steps of this project are:__
 - Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
 - Apply a distortion correction to raw images.
 - Use colour transforms, gradients, etc., to create a thresholded binary image.
 - Apply a perspective transform to rectify binary image ("birds-eye view").
 - Detect lane pixels and fit to find the lane boundary.
 - Determine the curvature of the lane and vehicle position with respect to centre.
 - Warp the detected lane boundaries back onto the original image.
 - Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle
position.





__Rubric Points__
Camera Calibration

1. Have the camera matrix and distortion coefficients been computed correctly and
checked on one of the calibration images as a test?

The code for this step is contained in the second code cell of the P2.ipynb notebook 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the
world. 

Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are
the same for each calibration image. Thus, `objp` is just a replicated array of coordinates, and `objpoints`
will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.

`imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with
each successful chessboard detection.

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.

Before:

[//]: #	"Image References"

​                  

[image1]: ./camera_cal/calibration1.jpg	"Before"

 ![alt text][image1]



After:

[//]: #	"Image References"

​                  

[image2]: ./camera_cal/calibration1_undist.jpg	"After"

 ![alt text][image2]

This distortion correction is applied to the test
images using the `cv2.undistort()` function, here is an example result:

Original Example:

[//]: #	"Image References"
[image3]: ./test_images/test1.jpg "Original Example"

![alt text][image3]



Undistorted Example:

[//]: #	"Image References"
[image4]: ./writeup_images/test1_undist.jpg	"Undistorted Example"

 ![alt text][image4]






__Pipeline (single images)__

The pipeline is in code cells 4 - 11, which are all titled _Pipeline Function X_ to aid with navigation and review. The function which runs the pipeline, `run_full_pipeline`, is in cell 13.



1. Has the distortion correction been correctly applied to each image?

    The test images were processed in the fourth code cell in P2.ipynb, Pipeline Function 1: Undistort raw images.

    * The `pickle` file  with the calibration is opened to obtain the calculated `mtx` and `dist` matrices 

    * The `cv2.undistort` function is used with these two matrices to transform the image

      

2. Has a binary image been created using color transforms, gradients or other methods?

    A combined_binary image is created in "Pipeline Function 2", the fifth code cell in P2.ipynb 

    * The image is converted to hls with `cv2.COLOR_RGB2HLS`, then using a threshold for the saturation value, the first binary is obtained, `S_binary`.

    * The image is converted to greyscale using `cv2.COLOR2GRAY` and a second binary, `sxbinary` is obtained using the Sobel operator to take the derivative in x to accentuate lines away from the horizontal.

    * The `combined_binary` image is formed by stacking the two results. Here is an example:

      Combined Binary:

      [//]: #	"Image References"
      [image6]: ./writeup_images/test1_combinedbinary.jpg "Combined Binary"

      ![alt text][image6]

      

    * NB:  It can be helpful to look at a combined colour "binary", showing the pixels from each of the two thresholding processes, to help visualise the results whilst choosing the threshold values:

      Colour "Binary":
      
    [//]: #	"Image References"
      [image7]: ./writeup_images/test1_colourbinary.jpg "Colour Binary"

      ![alt text][image7]
      
      

3. Has a perspective transform been applied to rectify the image?

    The perspective transform is applied in Pipeline Function 6, using the hardcoded source, `src` and destination `dst` points, which are setup in the `run_full_pipeline` function.

    The perspective transform was verified by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image. You can see this in action with a straight road:

    Road with Straight Lines:

    [//]: #	"Image References"
    [image8]: ./writeup_images/straight_lines2_undist.jpg "Road with Straight Lines"

    ![alt text][image8]

    

    Detected Lines Warped to Bird's Eye View:

    [//]: #	"Image References"
    [image9]: ./writeup_images/straight_lines2_warped.jpg "Detected Lines Warped to Bird's Eye View"

    ![alt text][image9]

    

4. Have lane line pixels been identified in the rectified image and fit with a polynomial?

    Using 

    `try:

    ​	left_fitx

    ​	except NameError:`

    the decision is made as to whether this is the first image of the video, so that a sliding windows approach is used to identify the lane lines.

    Sliding Windows Approach:

    [//]: #	"Image References"
    [image10]: ./writeup_images/test6_undist_wLane_binwarp.jpg "Sliding Windows Approach"

    ![alt text][image10]

    

    For subsequent images, a search around the existing polynomials comes into play.

    

5. The polynomials are then warped back onto the image, (and for the final output a polyfill function is used for a more aesthetically pleasing "lane" visualisation).

    Detected Lines Warped back to Perspective View:

    [//]: #	"Image References"
    [image11]: ./writeup_images/test6_undist_wLane.jpg "Detected Lines Warped back to Perspective View"

    ![alt text][image11]

    

6. Having identified the lane lines, has the radius of curvature of the road been estimated, and the position of the vehicle with respect to centre in the lane?

    Pipeline Function 6 calculates the radius of curvature of the two kines, then averages them to output the value then displayed on the final result image, and then also calculates the position with respect to centre.

    Both these values are then displayed on the image/video:

    [//]: #	"Image References"
    [image13]: ./writeup_images/test5_lane_overlay.jpg "Detected Lines Warped to Bird's Eye View"

    ![alt text][image13]





__Pipeline (video)__
1. Does the pipeline established with the test images work to process the video?
    It sure does! Here's a link to my video result:

  ./output_images/challengeaccepted.mp4





__README__
1. Has a README file been included that describes in detail the steps taken to construct
the pipeline, techniques used, areas where improvements could be made?
You're reading it!



__Discussion__


Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.

Challenges overcome:

* Initial camera cal showed some unusal distortion - Had the wrong size chess board!!! 6x9!!!

* Spent a lot of time getting imwrite to work - not sure of route cause of this, think it was a combo of issues: concatenating pathnames, mixing up path and file names etc

* Struggled with visualising the images inline in the jupyter notebook, as they would display in a strange colour, despite saving correctly and plotting correctly using `plt.imshow`.



Potential shortcomings, failure cases and future improvements:

* I have written this as a function based pipeline. Speaking to a colleague who is also studying this course, he chose a class based approach, and I realised I don't fully understand the advantages of this, so will go away and read up about classes and discuss with him further.
* The hardcoded perspective transform could be improved by using some detection techniques to identify the horizon and vanishing point etc 
* The pipeline seems to function reasonably well on the images. However, whilst the pipeline successfully processes the video, it does not perform particularly well, and shows some shortcomings on both the challenge video and even more so on the harder challenge video:
  * Road surface changes, shadows and lighting all cause considerable issues. This pipeline is very rudimentary and would need considerable improvement if it was to be used to actually successfully identify a lane.


