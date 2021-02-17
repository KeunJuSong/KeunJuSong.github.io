---
title : "Spot the Difference Game"
permalink : /projects/spot-the-difference-game/
excerpt: "Spot the difference Game is developed with Visual Stduio and OpenCV. Especially, contours detection is the main function in this project."
tags: [opencv, visual-studio-code, spot-the-diff]
toc: true
---

# **Overview**

* Term project in Embedded Vision course (Undergraduate)
* Application of Image Processing with using OpenCV
* If you want more information about its source code, visit my **[Github](https://github.com/KeunJuSong/Spot-the-difference-Game)**

# **Idea**

* For easy control, this program is based on mouse-click actions
* Contours detection is the main function in this program process

# **Description**

* Have 3 levels of difficulty, Hard-Normal-Easy
* Time limit is depend on the level of difficulty
* Contours Detection
  * Use 3 OpenCV methods
    * ```cvAbsDiff()```
    * ```cvFindContours()```
    * ```cvDrawContours()```
  * More information about these 3 methods, refer some documentation sites

## **cvAbsDiff()**

* Format
  * ```cvAbsDiff(CvArr* src1, CvArr* src2, CvArr* dst)```
* Function
  * Solve the absolute value from the difference between ```src1``` and ```src2``` (matrix form)
  * Put the solved absolute value in ```dst``` variable
  * For this program, this method solves the difference value between **original image** and **manipulated image** 

## **cvFindContours()**

* Format
  * ```cvFindContours(CvArr* image, CvMemStorage* storage, CvSeq** firstContour, int headerSize = sizeof(CvContour), int mode = CV_RETR_LIST, int method = CV_CHAIN_APPROX_SIMPLE, CvPoint offset = cvPoint(0,0))```
* Function
  * Find the number of contours in binary image (Convert from input image)
  * Return variable (sequence), ```firstContour``` is the outermost contour
  * For this program, this method solves **the contours from the result image of cvAbsDiff() method**
* Parameters
  * ```image``` 
    * Input image (original), must be 8-bit single channel image(Binary Image)
  * ```storage```
    * Memory that searched contours are saved in
  * ```firstContour```
    * The pointer of the outermost contour
  * ```headerSize```
    * Size of Sequence Header
  * ```mode```
    * ```CV_RETR_EXTERNAL``` 
    * ```CV_RETR_LIST``` 
    * ```CV_RETR_CCOMP``` 
    * ```CV_RETR_TREE``` 
  * ```method```
    * ```CV_CHAIN_CODE```
    * ```CV_CHAIN_APPROX_NONE``` 
    * ```CV_CHAIN_APPROX_SIMPLE```   
    * ```CV_CHAIN_APPROX_TC89_L1, CV_CHAIN_APPROX_TC89_KCOS```
    * ```CV_LINK_RUNS```
  * ```offset```

## **cvDrawContours()**

* Format
  * ```cvDrawContours(CvArr* img, CvSeq* contour, CvScalar external_color, CvScalar hole_color, int max_level, int thickness = 1, int lineType = 8)```

* Function
  * Draw the contours from ```CvSeq* contour``` 
  * Differentiate exterior or hole before drawing
  * For this program, this method draws **the contours from the result of cvFindContours() method**
* Parameters
  * ```image```
  * ```contour```
    * Contours information from ```cvFindContours()``` method
  * ```external_color```
  * ```hole_color```
  * ```max_level```
  * ```thickness```
  * ```lineType```

# **Result**

* Game start & Setting levels difficulty

  <figure>
    <img src="{{ '/assets/images/Spot_the_Difference_game-start.jpg' | relative_url }}" alt="Game start screen">
    <figcaption>Game Start</figcaption>
  </figure>

  <figure>
    <img src="{{ '/assets/images/Spot_the_Difference_level-difficulty.png' | relative_url }}" alt="Level difficulty">
    <figcaption>Level difficulty</figcaption>
  </figure>

* Game play screen

  <figure>
    <img src="{{ '/assets/images/Spot_the_Difference_game-play1.png' | relative_url }}" alt="Game play-inital">
    <figcaption>Game Play - Inital mode</figcaption>
  </figure>

  <figure>
    <img src="{{ '/assets/images/Spot_the_Difference_game-play2.png' | relative_url }}" alt="Game play-run">
    <figcaption>Game Play - Running mode</figcaption>
  </figure>

* Game clear & over

  <figure>
    <img src="{{ '/assets/images/Spot_the_Difference_game-clear.png' | relative_url }}" alt="Game Clear">
    <figcaption>Game Clear</figcaption>
  </figure>

  <figure>
    <img src="{{ '/assets/images/Spot_the_Difference_game-over.jpg' | relative_url }}" alt="Game Over">
    <figcaption>Game Over</figcaption>
  </figure>

# **Development Environment**

* Visual Studio 2017 (C++ language)
* OpenCV

# **Reference**

* [Contours detection in OpenCV](https://m.blog.naver.com/PostView.nhn?blogId=st8123&logNo=220326837110&proxyReferer=https:%2F%2Fwww.google.com%2F)
* [Documentation_cvAbsDiff()](https://opencvlib.weebly.com/cvabs-cvabsdiff-cvabsdiffs.html)
* [Documentation_cvFindContours()](https://opencvlib.weebly.com/cvfindcontours.html)
* [Documentation_cvDrawContours()](https://opencvlib.weebly.com/cvdrawcontours.html)
