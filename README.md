# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

- First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load.
- Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed.
- In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson.
- In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures.

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning.

## Dependencies for Running Locally

1. cmake >= 2.8

- All OSes: [click here for installation instructions](https://cmake.org/install/)

2. make >= 4.1 (Linux, Mac), 3.81 (Windows)

- Linux: make is installed by default on most Linux distros
- Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
- Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)

3. OpenCV >= 4.1

- All OSes: refer to the [official instructions](https://docs.opencv.org/master/df/d65/tutorial_table_of_content_introduction.html)
- This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors. If using [homebrew](https://brew.sh/): `$> brew install --build-from-source opencv` will install required dependencies and compile opencv with the `opencv_contrib` module by default (no need to set `-DOPENCV_ENABLE_NONFREE=ON` manually).
- The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)

4. gcc/g++ >= 5.4

- Linux: gcc / g++ is installed by default on most Linux distros
- Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
- Windows: recommend using either [MinGW-w64](http://mingw-w64.org/doku.php/start) or [Microsoft's VCPKG, a C++ package manager](https://docs.microsoft.com/en-us/cpp/build/install-vcpkg?view=msvc-160&tabs=windows). VCPKG maintains its own binary distributions of OpenCV and many other packages. To see what packages are available, type `vcpkg search` at the command prompt. For example, once you've _VCPKG_ installed, you can install _OpenCV 4.1_ with the command:

```bash
c:\vcpkg> vcpkg install opencv4[nonfree,contrib]:x64-windows
```

Then, add _C:\vcpkg\installed\x64-windows\bin_ and _C:\vcpkg\installed\x64-windows\debug\bin_ to your user's _PATH_ variable. Also, set the _CMake Toolchain File_ to _c:\vcpkg\scripts\buildsystems\vcpkg.cmake_.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

## MP.1 Data Buffer Optimization

This is accomplished by creating the `RingBuffer` class in `MidTermProject_Camera_Student:29`. \
If the addition of a new element would increase the size of the buffer beyond the limit, an element is erased from the front before pushing a new element to the back.

## MP.2 Keypoint Detection

The HARRIS detector is implemented in `matching2D_Student:103`. The remainder of the detectors were implemented in `matching2D_Student:139`. They are selectable by the `string detectorType` variable in `MidTermProject_Camera_Student:68`.

## MP.3 Keypoint Removal

The keypoint removal is implemented using the `cv::KeyPointsFilter::runByPixelsMask` method on `MidTermProject_Camera_Student:126`.

## MP.4 Keypoint Descriptors

The keypoint descriptors are implemented in `matching2D_Student:39` and are configurable through the `string descritorType` variable in `MidTermProject_Camera_Student:69`. It is also important to set the `descriptorClass` variable depending on what type of descriptor is being used.

## MP.5 Descriptor Matching

FLANN is implemented in `matching2D_Student:19` and k-nearest neighbor is implemented in `matching2D_Student:28`.

## MP.6 Descriptor Distance Ratio

The descriptor distance ration test is implemented in `matching2D_Student:31` with a distance ratio of 0.8.

## MP.7 Performance Evaluation 1

| Detector  | Img 1 | Img 2 | Img 3 | Img 4 | Img 5 | Img 6 | Img 7 | Img 8 | Img 9 | Img 10 | Avg Neighborhood Size |
| --------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ------ | --------------------- |
| SHITOMASI | 125   | 118   | 123   | 120   | 120   | 113   | 114   | 123   | 111   | 112    | 4                     |
| HARRIS    | 50    | 54    | 53    | 55    | 56    | 58    | 57    | 61    | 59    | 57     | 4                     |
| FAST      | 419   | 427   | 404   | 423   | 386   | 414   | 418   | 406   | 396   | 401    | 7                     |
| BRISK     | 264   | 282   | 282   | 277   | 296   | 279   | 289   | 272   | 266   | 254    | 22.04                 |
| ORB       | 92    | 102   | 106   | 113   | 109   | 125   | 130   | 129   | 127   | 128    | 54.39                 |
| AKAZE     | 166   | 157   | 161   | 155   | 163   | 164   | 173   | 175   | 177   | 179    | 7.89                  |
| SIFT      | 138   | 132   | 124   | 137   | 134   | 140   | 137   | 148   | 159   | 137    | 5.63                  |

## MP.8 Performance Evaluation 2

N/A is specified for combinations that are incompatible in OpenCV

| Detector  | Descriptor | Total Matches for all 10 Images |
| --------- | ---------- | ------------------------------- |
| SHITOMASI | BRIEF      | 944                             |
| SHITOMASI | ORB        | 907                             |
| SHITOMASI | FREAK      | 766                             |
| SHITOMASI | AKAZE      | N/A                             |
| SHITOMASI | SIFT       | 927                             |
| HARRIS    | BRIEF      | 400                             |
| HARRIS    | ORB        | 449                             |
| HARRIS    | FREAK      | 403                             |
| HARRIS    | AKAZE      | N/A                             |
| HARRIS    | SIFT       | 459                             |
| FAST      | BRIEF      | 2831                            |
| FAST      | ORB        | 2762                            |
| FAST      | FREAK      | 2225                            |
| FAST      | AKAZE      | N/A                             |
| FAST      | SIFT       | 2782                            |
| BRISK     | BRIEF      | 1073                            |
| BRISK     | ORB        | 1510                            |
| BRISK     | FREAK      | 1525                            |
| BRISK     | AKAZE      | N/A                             |
| BRISK     | SIFT       | 1646                            |
| ORB       | BRIEF      | 545                             |
| ORB       | ORB        | 761                             |
| ORB       | FREAK      | 421                             |
| ORB       | AKAZE      | N/A                             |
| ORB       | SIFT       | 763                             |
| AKAZE     | BRIEF      | 1266                            |
| AKAZE     | ORB        | 1186                            |
| AKAZE     | FREAK      | 1188                            |
| AKAZE     | AKAZE      | 1259                            |
| AKAZE     | SIFT       | 1270                            |
| SIFT      | BRIEF      | 702                             |
| SIFT      | ORB        | N/A                             |
| SIFT      | FREAK      | 596                             |
| SIFT      | AKAZE      | N/A                             |
| SIFT      | SIFT       | 800                             |

## MP.9 Performance Evaluation 3

N/A is specified for combinations that are incompatible in OpenCV

| Detector  | Descriptor | Average Time (ms) |
| --------- | ---------- | ----------------- |
| SHITOMASI | BRIEF      | 9.79              |
| SHITOMASI | ORB        | 12.12             |
| SHITOMASI | FREAK      | 32.32             |
| SHITOMASI | AKAZE      | N/A               |
| SHITOMASI | SIFT       | 25.91             |
| HARRIS    | BRIEF      | 9.92              |
| HARRIS    | ORB        | 12.52             |
| HARRIS    | FREAK      | 35.73             |
| HARRIS    | AKAZE      | N/A               |
| HARRIS    | SIFT       | 24.20             |
| FAST      | BRIEF      | 3.78              |
| FAST      | ORB        | 5.44              |
| FAST      | FREAK      | 27.85             |
| FAST      | AKAZE      | N/A               |
| FAST      | SIFT       | 22.48             |
| BRISK     | BRIEF      | 67.51             |
| BRISK     | ORB        | 74.39             |
| BRISK     | FREAK      | 88.66             |
| BRISK     | AKAZE      | N/A               |
| BRISK     | SIFT       | 95.23             |
| ORB       | BRIEF      | 20.29             |
| ORB       | ORB        | 28.43             |
| ORB       | FREAK      | 43.73             |
| ORB       | AKAZE      | N/A               |
| ORB       | SIFT       | 46.53             |
| AKAZE     | BRIEF      | 47.84             |
| AKAZE     | ORB        | 55.99             |
| AKAZE     | FREAK      | 68.27             |
| AKAZE     | AKAZE      | 84.90             |
| AKAZE     | SIFT       | 61.91             |
| SIFT      | BRIEF      | 83.31             |
| SIFT      | ORB        | N/A               |
| SIFT      | FREAK      | 107.48            |
| SIFT      | AKAZE      | N/A               |
| SIFT      | SIFT       | 140.53            |

### Recommendations

1. FAST / BRIEF - Largest number of matches and fastest time
2. FAST / ORB - Third largest matches and second fastest time
3. FAST / SIFT - Second largest matches and still very fast time
