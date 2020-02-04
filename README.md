# SFND 3D Object Tracking

Welcome to the final project of the camera course. By completing all the lessons, you now have a solid understanding of keypoint detectors, descriptors, and methods to match them between successive images. Also, you know how to detect objects in an image using the YOLO deep-learning framework. And finally, you know how to associate regions in a camera image with Lidar points in 3D space. Let's take a look at our program schematic to see what we already have accomplished and what's still missing.


<img src="images/3n1o01.png" width="779" height="414" />

<img src="images/course_code_structure.png" width="779" height="414" />



In this final project, you will implement the missing parts in the schematic. To do this, you will complete four major tasks: 
1. First, you will develop a way to match 3D objects over time by using keypoint correspondences. 
2. Second, you will compute the TTC based on Lidar measurements. 
3. You will then proceed to do the same using the camera, which requires to first associate keypoint matches to regions of interest and then to compute the TTC based on those matches. 
4. And lastly, you will conduct various tests with the framework. Your goal is to identify the most suitable detector/descriptor combination for TTC estimation and also to search for problems that can lead to faulty measurements by the camera or Lidar sensor. In the last course of this Nanodegree, you will learn about the Kalman filter, which is a great way to combine the two independent TTC measurements into an improved version which is much more reliable than a single sensor alone can be. But before we think about such things, let us focus on your final project in the camera course. 

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level project directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./3D_object_tracking`.

## FP.1 Match 3D Objects
Code implemented in camFusion_Student.cpp starting at line 301.
Using current and previous dataframes we track the pairs of bounding boxs and their IDs. In order to find the best match we find the cases with largest matches.

## FP.2 Compute Lidar-based TTC
Code implemented in camFusion_Student.cpp starting at line 256.
In order to make the results statisitcally robust and remove the outliers three new functions are defined. "mean", "sigma" and "RemoveOutlier".
The "RemoveOutlier" function removes the data points that are not in the 2 * (standard deviation) of the mean.

## FP.3 Associate Keypoint Correspondences with Bounding Boxes
Code implemented in camFusion_Student.cpp starting at line 134.
After calculating the mean distance of all keypoint matches the points that are satisfying the condition of having a distance smaller than the mean and are located in the region of itrest are seleceted.

## FP.4 Compute Camera-based TTC
Code implemented in camFusion_Student.cpp starting at line 159. Following the ideas that were developed in the corresponding lecture.

## FP.5 Performance Evaluation 1
Experimenting with different combinations of detector and descriptor in the code, majority of the cases included at least one bad TTC reading which can be attributed to the outlier removal method. 

## FP.6 Performance Evaluation 2

FAST + BRIEF combination is working good for camera TTC but lidar TTC has some wrong values.
FAST + ORB combination includes wrong values for both camera and lidar.

| Detector        | Descriptor           | Lidar TTC (Worst reading)  |  Camera TTC (Worst reading)|
| -------------   |:-------------------: |:-----------------:|:-----------------:|
| FAST            | BRIEF                | 34.21 s           |       -           |
| FAST            | ORB                  | 34.5              |       24.34       |

Other possible combinations demonstrate same type of behaviour.

