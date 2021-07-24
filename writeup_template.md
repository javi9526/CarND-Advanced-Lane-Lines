# Self-Driving Car Engineer Nanodegree

## Project: Advanced Lane Finding
---

The project is divided into 4 parts:

1. Camera Calibration.
2. Calculate perspective transform.
3. Image lane finding.
4. Video lane finding.

---
**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image0]: ./camera_cal/calibration1.jpg "Original"
[image1]: ./output_images/Undist/calibration1.jpg "Undistorted"

[image2]: ./output_images/Perspective/straight_lines8.jpg "Original"
[image3]: ./output_images/Perspective/straight_lines_warped.jpg "Road Transformed"

[image4]: ./test_images/test4.jpg "Image Example"
[image5]: ./output_images/test_images/test4.jpg "Image with lane detection"

[image6]: ./output_images/sliding_windows.jpg "Sliding windows" 
[image7]: ./output_images/search_around_poly.jpg "Search around poly"
[image8]: ./output_images/test_images/test4.jpg "Warped lane detection"

[video1]: ./project_video_output.mp4 "Video"



## 1. Camera calibration

In this section we will compute the camera calibration matrix and distortion coefficients given a set of chessboard images in the folder "camera_cal". 

After that, a distortion correction will be applied to those images, with the corresponding representation to see the difference. 

The code for this step is contained in the first code cell of the IPython notebook located in "./P2.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

ORIGINAL
![alt text][image0]

UNDISTORTED
![alt text][image1]



## 2. Calculate perspective transform 

In this section, we will estimate the perspective transform matrix to obtain an eye bird vision of the lane.

The code for this step is contained in the second, third, fourth and fifth code cells of the IPython notebook located in "./P2.ipynb".  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the source and destination points using the image with yellow lines `test_images/straight_lines2.jpg` provided in the following manner:

```python

# Source coordinates
src_yellow = np.float32(
    [[591,450],
     [688,450],
     [1050,674],
     [257,674]])
# Destination coordinates
dst_yellow = np.float32([
    [257,0],
    [1050,0],
    [1050,674],
    [257,674]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 591, 450      | 2570, 0       | 
| 688, 450      | 1050, 0       |
| 1050, 675     | 1050, 675     |
| 257, 675      | 257, 675      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

ORIGINAL
![alt text][image2]

WARPED
![alt text][image3]




## 3. Image lane finding 

The steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

####  Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

To obtain the lane lines pixels and fit their positions I used the `fit_poly_and_color_lane_lines()` functions, which is implemented in the eighth cell, in lines 187 through 120. 

At first, this function finds the lane lines pixels by calling `find_lane_pixels()` (sliding windows method, implemented in lines 50 through 190 in the same cell) or `search_around_poly()` (finds lane lines points searching in a bounded region around the polinomial function calculated before with sliding windows method). The election of the method depends on the sanity check.

Once we have the lane lines points, its time to do a 2nd polynomial fit for each line.

IMAGEN SLIDING WINDOWS
![alt text][image6]

IMAGEN SEARCH AROUND POLY
![alt text][image7]

LANE DETECTION
![alt text][image8]

#### Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

For the radius of curvature, I did applied the formula that uses the fit polynomial coefficients in lines 240 through 250 in the sixth cell of the IPython notebook located in "./P2.ipynb".

#### Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

For plotting the result back, I implemented the same perspective transform of the lane lines image at first. In this case, with the inverse matrix. This is made in line 45 in in the 10th code cell of the IPython notebook located in "./P2.ipynb". Then I weighted that image with the original undistored. The result is:


![alt text][image7]

---

## 4. Video lane finding

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)



