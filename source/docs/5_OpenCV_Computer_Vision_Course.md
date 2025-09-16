# 5. OpenCV Computer Vision Course

## 5.1 Application Course

### 5.1.1 Color Recognition

* **Program Logic**

The color recognition function entails the camera recognizing objects in red, green, and blue colors. Once the target color is identified, the object will be encircled with a circle of the corresponding color in the returned image.

Color processing is executed through the Lab color space. Initially, the RGB color space is converted to Lab, followed by binarization processing. Subsequent operations, including expansion and corrosion, are performed to obtain an outline containing solely the target color. Finally, the color outline is encircled to achieve color identification of the object.

* **Operation Steps**

Before operation, transfer the file '**color_discern.py**' saved in '[**5. OpenCV Computer Vision Course/5.1 Application Course/ Source Code**'](https://drive.google.com/drive/folders/1u79MXLClK7q9AD03_WuSTePY3r_dp8wG?usp=sharing) to the Raspberry Pi.

For file transfer methods, please consult the documents located in the directory ["**2. Raspberry Pi 5 Basic Operation and Configuration \> 2.2 Remote Access and Remote File Transfer**".](2_Raspberry_Pi_5_Basic_Operation_and_Configuration.md#remote-access-and-remote-file-transfer)

> [!Note]
>
> **The input command should be case sensitive, and keywords can be complemented using 'Tab' key.**
>

(1) Open VNC, and use short-cut '**Ctrl+Alt+T**' to open the command-line terminal, then run the command '**cd Opencv**' and hit Enter to navigate to the directory containing the program files.

```bash
cd Opencv
```

(2) Type the command '**python3 color_discern.py**' to start the color recognition.

```bash
python3 color_discern.py
```

At this point, the live camera feed window will occur. When a red, green, or blue object is detected, a circle of the corresponding color will appear to encircle the object, as depicted in the following illustration:

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image4.png" />

(3) If you need to terminate the program, hit '**ESC**' key.

* **Program Analysis**

The source code of the program is saved in: `/home/pi/Opencv/color_discern.py`

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image5.jpeg" />

(1) Import Library Files

Import the cv2 library from OpenCV, as well as the sys, time, and math modules. Additionally, instantiate the numpy library.

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image6.jpeg" />

(2) Set the Color Threshold

Next, establish the threshold for color recognition. Initially, define the target thresholds for color recognition by the camera (note that OpenCV's default color model is BGR, with "'red': (0, 0, 255),'blue': (255, 0, 0),'green': (0 , 255, 0)"). Then, specify a range for the threshold, as illustrated below:

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image7.jpeg" />

(3) Retrieve Recognition Image

① The second parameter invokes the VideoCapture() function to instantiate the camera object. Its argument 0 denotes the first camera. If there are multiple cameras, the parameter can be adjusted to 1, 2, 3, and so forth.

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image8.png" />

② Within the while loop, utilize the read() function of the camera object to capture a frame of the video and display it.

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image9.jpeg" />

③ Then, wait for a duration of 1 unit time. If the "ESC" key is pressed on the keyboard during this interval, exit the program by closing the window.

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image10.jpeg" />

④ Call destroyAllWindows() function to close all the windows.

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image11.png" />

(4) Color Recognition

After capturing the recognition image through the camera, the image undergoes processing via the getAreaMaxContour() function to obtain the object's outline. The specific processing method is illustrated below:

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image12.jpeg" />

Subsequently, the identification information is printed by invoking the putText() function from the cv2 library, as depicted in the figure below (excerpt from the function):

<img class="common_img" src="../_static/media/chapter_5/section_1/media/image13.png" />

The parameter breakdown of the aforementioned code `cv2.putText(img, "Color:" + detect_color,(10, img.shape[0] - 10)`, `cv2.FONT_HERSHEY_SIMPLEX, 0.65, draw_color, 2)`" is as follows:

The first parameter `image` represents the target image.

The second parameter `Color + detect color` denotes the text string to be drawn.

The third parameter `(10, img.shape[0] - 10)` indicates the coordinates of the lower left corner of the text string in the image.

The fourth parameter `cv2.FONT_HERSHgpiodetectEY_SIMPLEX` specifies the font type for printing.

The fifth parameter `0.65` signifies the font size.

The sixth parameter `draw_color` specifies the font color.

The seventh parameter `2` denotes the font thickness.

### 5.1.2 Human Face Detection

* **Program Logic**

Load the pre-trained Haar feature classifier model. Next, initialize a window and capture the video stream. Process each frame of the video by converting it to grayscale and applying the Haar feature classifier for face detection. Detected face positions will be highlighted by drawing rectangular frames on the original image. Finally, display the processed image in real-time within the window.

* **Operation Steps**

Before operation, transfer the file '**face.py、haarcascade_frontalface_default.xml**' saved in '**[5. OpenCV Computer Vision Course/ 5.1 Application Course/ Source Code](https://drive.google.com/drive/folders/1u79MXLClK7q9AD03_WuSTePY3r_dp8wG?usp=sharing)**' to the Raspberry Pi.

For file transfer methods, please consult the documents located in the directory "**[2. Raspberry Pi 5 Basic Operation and Configuration \> 2.2 Remote Access and Remote File Transfer](2_Raspberry_Pi_5_Basic_Operation_and_Configuration.md#remote-access-and-remote-file-transfer)**".

> [!Note]
> 
>**The input command should be case sensitive, and keywords can be complemented using 'Tab' key.**

(1) Open VNC, and use short-cut '**Ctrl+Alt+T**' to open the command-line terminal, then run the command '**cd OpenCV/face/**' and hit Enter to navigate to the directory containing the program files.

```bash
cd OpenCV/face/
```

(2) Type the command '**python3 face.py**' to start the face detction.

```bash
python3 face.py
```

At this point, the live camera feed window will occur. The human face will be recognized and framed.

<img class="common_img" src="../_static/media/chapter_5/section_2/media/image4.png" />

(3) If you need to terminate the program, hit '**ESC**' key.

* **Program Analysis**

The source code of this program is saved in:` /home/pi/Opencv/face/face.py`

```python
#导入所需的库(Import required libraries)
import cv2 as cv
import numpy as np
#检测函数(Detection function)
def face_detect(image):
    gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY) #转化图像为灰度图(Convert image to grayscale)
    face_detector = cv.CascadeClassifier("./haarcascade_frontalface_default.xml")#读取人脸数据(Read face data)
    faces = face_detector.detectMultiScale(gray,1.02,20)#进行人脸检测(Perform face detection)
    for x, y, w, h in faces:
        cv.rectangle(image, (x, y), (x + w, y + h), (0, 0, 255), 2)#对人脸位置画框(Draw rectangle around face position)
    cv.imshow("face_detect", image)#展示(Display)
#运行人脸检测并显示(Run face detection and display)
def video_face_detect():
    capture = cv.VideoCapture(0)#设置使用的相机(Set the camera to use)
    while True:
        ret, frame = capture.read()#读取相机图像(Read camera image)
        frame = cv.flip(frame, 1)#将回传画面设置图像水平翻转(Set the returned image to horizontal flip)
        face_detect(frame)#人脸检测(Face detection)
        c = cv.waitKey(10)
        if c==27:  #按下ESC键退出(Press ESC key to exit)
            break
```

(1) Import Library Files

Import the cv2 library from OpenCV. Additionally, instantiate the numpy library.

```
import cv2 as cv
import numpy as np
```

(2) Main Function Analysis

① Human Face Real Time Detection

```
if __name__ == '__main__':
    video_face_detect()#实时检测人脸(Real-time face detection)
```

* Call `video_face_detect()` function to execute the human face detection.

```python
def video_face_detect():
    capture = cv.VideoCapture(0)#设置使用的相机(Set the camera to use)
    while True:
        ret, frame = capture.read()#读取相机图像(Read camera image)
        frame = cv.flip(frame, 1)#将回传画面设置图像水平翻转(Set the returned image to horizontal flip)
        face_detect(frame)#人脸检测(Face detection)
        c = cv.waitKey(10)
        if c==27:  #按下ESC键退出(Press ESC key to exit)
            break
```

* In the video_face_detect() function, invoke the VideoCapture() function from the cv2 library to instantiate the camera object.

```
    capture = cv.VideoCapture(0)#设置使用的相机(Set the camera to use)
```

Its parameter "0" indicates the first camera. If there are multiple cameras, adjust the parameter to 1, 2, 3, and so forth, accordingly.

* Within the while loop, utilize the read() function of the camera object to capture a frame of the video, and employ the flip() function from the cv2 library to horizontally flip the returned image.

```
    while True:
        ret, frame = capture.read()#读取相机图像(Read camera image)
        frame = cv.flip(frame, 1)#将回传画面设置图像水平翻转(Set the returned image to horizontal flip)
```

* Call face_detect() function to detect the image.

```
        face_detect(frame)#人脸检测(Face detection)
```

```
def face_detect(image):
    gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY) #转化图像为灰度图(Convert image to grayscale)
    face_detector = cv.CascadeClassifier("./haarcascade_frontalface_default.xml")#读取人脸数据(Read face data)
    faces = face_detector.detectMultiScale(gray,1.02,20)#进行人脸检测(Perform face detection)
```

* In the face detection function, for enhanced detection speed, initially employ the cvtColor() function from the cv2 library to convert the source image into grayscale. The first parameter "image" represents the source image, while "cv.COLOR_BGR2GRAY" denotes the target image. Subsequently, utilize the cv.CascadeClassifier() function to read the facial data. Then, call the detectMultiScale() function to identify faces. Within this function, the first parameter "gray" denotes the image to be analyzed, the second parameter "1.02" signifies the proportion coefficient of the search window in consecutive scans, indicating that each search window expands by 2%. The third parameter "20" represents the minimum number of adjacent rectangles required to constitute the detection target.

```
    for x, y, w, h in faces:
        cv.rectangle(image, (x, y), (x + w, y + h), (0, 0, 255), 2)#对人脸位置画框(Draw rectangle around face position)
    cv.imshow("face_detect", image)#展示(Display)
```

* Finally, utilize the cv.rectangle() function to frame the face positions and employ the cv.imshow() function to display the resultant image.

* Subsequently, wait for a duration of 10 units. If the ESC key is detected as keyboard input during this period, exit the program by closing the window.

```
        c = cv.waitKey(10)
        if c==27:  #按下ESC键退出(Press ESC key to exit)
            break
```

② Exit Face Detection

Invoke destroyAllWindows() function to close all the image window.

```
    cv.waitKey(0) #按下任意键退出(Press any key to exit)
    cv.destroyAllWindows()
```

### 5.1.3 Tag Recognition

*  **Program Logic**

Tag recognition involves identifying the corresponding ID of a tag card through camera recognition, displaying it on the return screen post-processing.

AprilTag, akin to barcodes and QR codes, serves as a visual positioning identifier facilitating swift tag detection and relative position calculation, meeting real-time requirements. The principles of tag recognition are delineated as follows:

(1) Image Acquisition and Processing:

① Initialize the camera.

② Capture the image and perform operations including copying, remapping, and display.

③ Convert the image information from BGR format to grayscale.

(2) Label Detection:

Obtain the coordinate information of the four corner points of the tag code.

Draw the outline of the Tag label based on the obtained coordinates.

(3) Label Information Acquisition:

① Clarify lattice coordinates within the determined quadrilateral.

② Further validate the encoding's reliability and match it with the known encoding library.

③ After filtering and validation, calculate the tag's ID and rotation angle.

(4) Tag Recognition and Buzzer Control:

① Frame the recognized tag.

② Control the buzzer to produce sound upon recognition.

(5) Determining Largest Labels:

① Convert recognized label coordinates into unscaled coordinates.

② Determine whether they represent the largest labels.

* **Operation Steps**

Before operation, transfer the file '**apriltag_discern.py**' saved in '**[5. OpenCV Computer Vision Course/5.1 Application Course/ Source Code](https://drive.google.com/drive/folders/1u79MXLClK7q9AD03_WuSTePY3r_dp8wG?usp=sharing)**' to the Raspberry Pi.

For file transfer methods, please consult the documents located in the directory "**[2. Raspberry Pi 5 Basic Operation and Configuration \> 2.2 Remote Access and Remote File Transfer](2_Raspberry_Pi_5_Basic_Operation_and_Configuration.md#remote-access-and-remote-file-transfer)**".

> [!Note]
> 
> **The input command should be case sensitive, and keywords can be complemented using 'Tab' key.**

(1) Open VNC, and use short-cut '**Ctrl+Alt+T**' to open the command-line terminal, then run the command '**cd Opencv**' and hit Enter to navigate to the directory containing the program files.

```bash
cd Opencv
```

(2) Type the command '**python3 apriltag_discern.py**' to start the tag recognition.

```bash
python3 apriltag_discern.py
```

If you encounter the following prompt, it indicates that the "**apriltag**" package is missing and must be manually installed.

<img class="common_img" src="../_static/media/chapter_5/section_3/media/image4.jpeg" />

(3) Install cmake compilation tool first to ensure the packages can be successfully compiled.

<img class="common_img" src="../_static/media/chapter_5/section_3/media/image5.jpeg" />

(4) After installing the compilation tool, execute the command '**pip3 install apriltag -i <https://pypi.tuna.tsinghua.edu.cn/simple>**', and hit Enter to install the '**apriltag**' package.

<img class="common_img" src="../_static/media/chapter_5/section_3/media/image6.jpeg" />

(5) Run the command '**python3 apriltag_discern.py**', and hit Enter to run the program.

At this point, the terminal's return screen will open, and the camera will identify tags within its visual range. Upon recognition of a tag, it will be framed on the return screen, and the ID information will be printed in the lower-left corner, as illustrated below:

<img class="common_img" src="../_static/media/chapter_5/section_3/media/image7.jpeg" />

(6) If you need to terminate this program, hit '**ESC**' key.

* **Program Analysis**

The source code of the program is saved in `/home/pi/Opencv/apriltag_discern.py`

```python
#标签识别(Tag Recognition)
import sys
import cv2
import math
import time
import numpy as np
import apriltag
import RPi.GPIO as GPIO

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)

#apriltag检测(AprilTag detection)

if sys.version_info.major == 2:
    print('Please run this program with python3!')
    sys.exit(0)
```

(1) Import Library Files

Import the cv2 library from OpenCV. Additionally, instantiate the numpy library.

```python
#标签识别(Tag Recognition)
import sys
import cv2
import math
import time
import numpy as np
import apriltag
import RPi.GPIO as GPIO
```

(2) Tag Detection

```
            cv2.drawContours(img, [np.array(corners, np.int)], -1, (0, 255, 255), 2)
```

AprilTag recognition primarily utilizes the drawContours() and putText() functions from the cv2 library:

The drawContours() function is employed to draw the outline of the tag. Taking "cv2.drawContours(img, \[np.array(corners, np.int)\], -1, (0, 255, 255), 2)" as an example, the parameters in the brackets hold the following meanings:

The first parameter "img" represents the image where the outline will be drawn.

The second parameter "\[np.array(corners, np.int)\]" denotes the contour itself, which is a Python list.

The third parameter "-1" specifies the contour to be drawn from the provided contour list; the value "-1" represents all contours within it.

The fourth parameter "(0, 255, 255)" signifies the outline color, with the order representing Blue, Green, Red; here, it represents yellow.

The fifth parameter "2" determines the width of the outline; "-1" indicates filling the outline with the specified color.

```
        cv2.putText(img, "tag_id: " + str(tag_id), (10, img.shape[0] - 30), cv2.FONT_HERSHEY_SIMPLEX, 0.65, [0, 255, 255], 2)
        cv2.putText(img, "tag_family: " + tag_family, (10, img.shape[0] - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.65, [0, 255, 255], 2)
```

The putText() function is utilized to display text content on the image. Taking `cv2.putText(img, "tag_id: " + str(tag_id), (10, img.shape\[0\] - 30), cv2.FONT_HERSHEY_SIMPLEX, 0.65, \[0, 255, 255\], 2)` as an example, the parameters are interpreted as follows:

The first parameter `img` represents the input image.

The second parameter `"tag_id: " + str(tag_id)` denotes the text to be added.

The third parameter `(10, img.shape[0] - 30)` specifies the coordinates of the upper-left corner of the added content.

The fourth parameter `cv2.FONT_HERSHEY_SIMPLEX` indicates the font style for the added content.

The fifth parameter `0.65` determines the font size.

The sixth parameter `[0, 255, 255]` defines the font color, with the order representing Blue, Green, Red; here, it denotes yellow.

The seventh parameter `2` specifies the font width.

### 5.1.4 Shape Recognition

The shape recognition feature enables the camera to identify objects of various shapes. Once the object's shape (triangle, rectangle, circle) is recognized, it is framed accordingly in the captured image. Initially, the object's color is recognized, followed by the identification of its corner points. These corner points are then distinguished, with the respective numbers varying based on the shape, as outlined in the table below:

| **Shape** | **Number of corner points** |
| :-------: | :-------------------------: |
|     0     |           Circle            |
|     3     |          Triangle           |
|     4     |          Rectangle          |

Once the shape of the object is recognized, the object will be framed with the corresponding shape.

To delve into the specific implementation principle of this function, please refer to the code analysis provided in this document.

* **Preparation**

(1) Hardware Preparation

Connect the camera to the Raspberry Pi controller.

(2) Software Preparation

①Access the Raspberry Pi system using VNC according to the instructions provided in '**[2. Raspberry Pi 5 Basic Operation and Configuration\2.2 Remote Access and Remote File Transfer](2_Raspberry_Pi_5_Basic_Operation_and_Configuration.md#remote-access-and-remote-file-transfer)**'.

②Setup OpenCV environment according to the tutorials provided in '**[5. OpenCV Computer Vision Course\5.2 Basic Course\ 5.2.2 Build OpenCV Environment](#anchor_5_2_2)**'.

*  **Import Shape Recognition Program Files**

> [!Note]
> 
> **The camera is not hot-swappable. Please ensure it is plugged into the USB interface on the Raspberry Pi motherboard before powering on the Raspberry Pi**

(1) Connect the camera to the Raspberry Pi controller and start the Raspberry Pi. After the Raspberry Pi is booted up successfully, establish the connection between the Raspberry Pi and VNC.

(2) Click-on <img src="../_static/media/chapter_5/section_4/media/image2.png" /> to transfer files.


<img class="common_img" src="../_static/media/chapter_5/section_4/media/image3.jpeg" />

(3) Choose '**Send files** ', and select the '**ShapeRecognize.py**'file, then click-on '**Open**'.

<img class="common_img" src="../_static/media/chapter_5/section_4/media/image4.jpeg" />

<img class="common_img" src="../_static/media/chapter_5/section_4/media/image5.jpeg" />

<img class="common_img" src="../_static/media/chapter_5/section_4/media/image6.jpeg" />

* **Start Testing**

(1) Power on the Raspberry Pi, and connect the Raspberry Pi to the control software VNC.

> [!Note]
> 
> Access the Raspberry Pi system using VNC according to the instructions provided in '**[2. Raspberry Pi 5 Basic Operation and Configuration\2.2 Remote Access and Remote File Transfer](2_Raspberry_Pi_5_Basic_Operation_and_Configuration.md#remote-access-and-remote-file-transfer)**'.
> 

(2) Click-on <img src="../_static/media/chapter_5/section_4/media/image7.jpeg" /> or press 'Ctrl+Alt+T' to open the command-line terminal.

<img class="common_img" src="../_static/media/chapter_5/section_4/media/image8.jpeg" />

(3) Execute the command '**cd Desktop/**' and hit Enter to switch to the system desktop.

```bash
cd Desktop/
```

(4) Run the command '**python3 ShapeRecognize.py**' to initiate the color recognition.

```bash
python3 ShapeRecognize.py
```

(5) At this point, the live camera feed will occur. When an object of red, green, or blue color is recognized, a circle of the corresponding color will appear to encircle the object.

(6) If you need to terminate this program, press 'q'key to terminate the running program.

(7) To facilitate later viewing, we can move/copy this program to the directory `home\pi\Ai_vision`.

<img class="common_img" src="../_static/media/chapter_5/section_4/media/image11.jpeg" />

* **Code Analysis**

Right-click the program file, and select '**Thonny** **Python** **IDE**'to open the program file.

<img class="common_img" src="../_static/media/chapter_5/section_4/media/image12.jpeg" />

(1) Import Library File

Import the CV2 library file from OpenCV, and initialize the numpy library.

```
import cv2
import numpy as np
```

(2) Detect Object Color and Draw a Frame

Next, define a function to identify the color of the detected object and draw its outline. The specific code is displayed in the figure below:

```python
#检测颜色并画框(Detect colors and draw frames)
def detection(gray,image):
    # 检测边界using a findContours() function(Detect boundaries using findContours() function)
    contours, _ = cv2.findContours(
        gray, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    i = 0
    # list for storing names of shapes(List for storing names of shapes)
    for contour in contours:
        # here we are ignoring first counter because
        # findcontour function detects whole image as shape
        #if i == 0:
            #i = 1
           # continue

        # 计算边长数量cv2.approxPloyDP() function to approximate the shape(Calculate edge count using cv2.approxPolyDP() function to approximate the shape)
        approx = cv2.approxPolyDP(
            contour, 0.06 * cv2.arcLength(contour, True), True)
        # 根据检测到边缘画边using drawContours() function(Draw edges based on detected boundaries using drawContours() function)
        cv2.drawContours(image, [contour], 0, (0, 0, 255), 5)
    try:
        # 寻找中心点finding center point of shape(Find center point of shape)
        M = cv2.moments(contour)
        if M['m00'] != 0.0:
            x = int(M['m10'] / M['m00'])
            y = int(M['m01'] / M['m00'])
```



Invoke the findContours() function from the cv2 library to identify the object's boundaries, and set i to a value of 0, as illustrated in the figure below:

```python
#检测颜色并画框(Detect colors and draw frames)
def detection(gray,image):
    # 检测边界using a findContours() function(Detect boundaries using findContours() function)
    contours, _ = cv2.findContours(
        gray, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    i = 0
```

Simultaneously, establish a loop to iterate through the detection data.

Subsequently, calculate the object's side length by invoking the `approxPolyDP()` function from the cv2 library.

```python
        # 计算边长数量cv2.approxPloyDP() function to approximate the shape(Calculate edge count using cv2.approxPolyDP() function to approximate the shape)
        approx = cv2.approxPolyDP(
            contour, 0.06 * cv2.arcLength(contour, True), True)
```

Then utilize the drawContours() function to sketch the edge line.

```
        # 根据检测到边缘画边using drawContours() function(Draw edges based on detected boundaries using drawContours() function)
        cv2.drawContours(image, [contour], 0, (0, 0, 255), 5)
```

In this function:

The first parameter, `image`, signifies the target image.

The second parameter, `[contours]`, denotes the input contour group, with each contour group consisting of a point vector.

The third parameter, `0`, specifies which contour to draw. If negative, all contours will be drawn.

The fourth parameter, `(0, 0, 255)`, represents the color of the outline.

The fifth parameter, `5`, indicates the line width of the outline. If negative or CV_FILLED, it denotes filling the inside of the outline.

(3) Shape Discrimination

Determine the shape based on the number of recognized side lengths. If the count is 3, it's a triangle, and "Triangle" will be printed on the return screen. If it's 4, it's a rectangle, and "Rectangle" will be printed. Otherwise, it's considered circular, and "Circle" will be printed.

## 5.2 Basic Course

### 5.2.1 Computer Vision and OpenCV Introduction

* **How robots "see" the world**

For artificial intelligence, the ability to see is essential. And how robots see the world involves machine vision, an important branch of artificial intelligence.

Machine vision is the idea that the robot takes human's place to measure and make judgments. The captured target will be converted into image signal by image sensor, CMOS or CCD, and then the image signal will be transferred to the specialized image processing system which will convert the image signal into digitized signal according to the pixel distribution, brightness, color, etc.

Image system perform various operations on these signals to extract the features of the target, so as to control the device in the field based on the judgments.

Machine vision technology is commonly applied in intelligent transport system and intelligent housing system.

* **Image Recognition Introduction**

Image recognition is a crucial technique that uses computer to process and analyze the image so as to recognize different targets.

Similar to human eyes, machine image recognition starts at the point where there is huge variance or sudden change, and the features will recognized one by one. Our brain controls our eyes to collect the major features of the image and filter the redundant information, and then integrate the major features into the complete visual image.

The process of computer image recognition is no different from that of human image recognition, which is divided into four steps.

(1) Acquire information: the light signal, sound signal, etc., are converted into electric signal by the sensors to acquire the information

(2) Image preprocessing: perform smoothing, denoising, etc., on the image to highlight the major features of the image.

(3) Feature extracting and selecting: extract and select the image features, which is the pivotal step.

(4) Image classification: make the recognition rules that is design classifier based on the training result to get the main category of the features so as to improve the recognition accuracy.

Image recognition is mostly applied in remote sensing image recognition and robot vision.

* **OpenCV Introduction**

OpenCV (Open Source Capture Vision) is a computer vision library for free handling various tasks about image and video, for example display the image collected by the camera and make the robot recognize the real object.

<img class="common_img" src="../_static/media/chapter_5/section_5/media/image2.png" />

OpenCV is more eminent than PIL, the built-in image processing library in Python. OpenCV provides complete Python interfaces, and Python3.5 and opencv-python library file have been integrated in the provided image system.

* **How Images are Stored in Computer**

How the images are stored in computer after they are recognized?

In general, a picture is composed of pixels and each pixel can be represented by R, G and B components within 0-255. OpenCV stores each pixel as a ternary array making it convenient to record all information of the image. In addition, OpenCV records the data of three color channels of RGB image in the order of BGR.

Besides, images of other standards (HSV) are stored as multivariate array.

An OpenCV image is a two-dimensional or three-dimensional array. An 8-bit grayscale image (black-and-white images) is a two-dimensional array, and a 24-bit BGR image is a three-dimensional array.

For a BGR image, the first value of the element "**image\[0,0,0\]**" represents Y--axis coordinate or the row number(0 represents the top). The second value represents X-axis coordinate or column number (0 represents leftmost ). And the third value represents the color channel.

Same as Python array, these array recording images can be accessed individually to obtain the data of some color channel or capture a region of the image.

<p id="anchor_5_2_2"></p>

### 5.2.2 Build OpenCV Environment

* **Install Numpy**

Each picture involves several pixels, which results in that a large number of arrays need to be processed in the program. Numpy is a extension library for Python, which handles multi-dimensional arrays more efficiently than

Python's native array structures. Besides, it can improve the readability of codes.

Open command line terminal and then input command "**pip install**

**numpy**" to install Numpy. For more information about Numpy, please move to the folder "**[3.Basic Programming Course->3.2 Python Programming->3.2.13 Python Numpy Basic Operation](3_Basic_Programming_Course.md#python-numpy-basic-operation)**" .

```bash
pip install numpy
```

* **Install OpenCV**

OpenCV package can be obtained from Ubuntu repository. Then refresh the packages index and install the OpenCV package by typing the following commands.

(1) sudo apt update: refresh the packages index

```bash
sudo apt update
```

(2) sudo apt install python3-opencv: Install the package. During installation, input "y" to continue the execution and the complete installation may take 10s.

```bash
sudo apt install python3-opencv
```

* **Verify the Installation of OpenCV**

We can import cv2 module to print the version of OpenCV so as to verify whether the installation is successful or not.

(1) python3: enter Python

(2) import cv2: import cv2 module

(3) cv2.\_\_version\_\_: check the version

If the version of OpenCV is printed, the installation is successful.

<img class="common_img" src="../_static/media/chapter_5/section_6/media/image5.jpeg" />

### 5.2.3 OpenCV Modules and Components

* **OpenCV Component**

OpenCV is composed of several layers of modules.

(1) The bottom layer is the hardware optimization based on HAL (Hardware Acceleration Layer)

(2) Above the bottom layer are the codes contributed by other developers contained in opencv_contrib module. These codes, core of OpenCV, involves most of the high-level functionality.

(3) The next layer are language bindings and sample applications.

(4) The top layer is the interaction between OpenCV and operating system.

<img class="common_img" src="../_static/media/chapter_5/section_7/media/image2.jpeg" />

* **Specific Module of OpenCV**

(1) Core: Contain the basic structure and operation of OpenCV library.

(2) Improc: Image processing module can transform the basic image, including filtering and convolution.

(3) Highgui: Seen as lightweight Windows UI Toolkit, it is divided into imcodecs, videoio and highgui in OpenCV 3.0. It contains user-interaction function used to display the images or simple input.

(4) Video: Contain the functions for reading and writing the video streams.

(5) Calib3d: Contain the algorithm of the calibration of single, binocular and multiple cameras.

(6)Feature2d: Used for the algorithm of feature point detection, description and matching.

(7) Objdectect: Contain the algorithm of specific target detection, including human face or passengers. And it can be used to train the detector to detect other objects.

(8) Ml: Machine learning module is a comprehensive module that involves a mass of machine learning algorithms which can interact with OpenCV data type.

(9) Flann: Flann stands for Fast Library for Approximate Nearest Neighbors, which will be called by the functions in other modules for fast nearest neighbor search in large datasets.

(10) GPU: It is segmented into several cuda\* modules in OpenCV. GPU module can optimize the functions on CUDA GPU and involves the functions only applicable to GPU. Without GPU, the computing resources cannot be promoted causing that some functions cannot return good results.

(11) Photo: A new module that contains the functions of computational photography.

(12) Stitching: Also a new module that stitches sophisticated images

(13) Nonfree: It is moved to opencv_contrib/xfeatures2d in OpenCV 3.0. There are some algorithms that is protected by patent and limited in usage in OpenCV, such as SIFT. These algorithms are isolated into their own modules, therefore you need to take special measures to use them in commercial products.

(14) Contrib: It involves something new that haven't been integrated into OpenCV.

(15) Legacy: It has been removed from OpenCV 3.0. This module contains some old stuffs that haven't been completely removed.

(16) Ocl: Khronos OpenCL standard. It has been removed from OpenCV 3.0 and replaced by T-API. Similar to GPU module, it realizes Khronos OpenCL standard for open parallel programming.

Compared with GPU module, it has fewer functions, but it aims at providing the parallel devices that can run on any GPU or is powered by Khronos. However, GPU module can only run on Nvidia GPU devices for the reason that it utilizes Nvidia CUDA toolkit to develop.

### 5.2.4 Picture & Video Loading and Display

* **Image Reading and Writing**

Read image: cv2.imread(Location ，Model)

(1) Location——read the location of the image which can be the absolute path and relative path. However pay attention to the usage of the slash in different operating system.

(2) Model——model of image loading. The first model is cv2.IMREAD_COLOR used to load a color picture but will not load Alpha channel(record degree of transparency). The second one is cv2.IMREAD_GRAYSCALE which is used to load a grayscale picture. The third type is cv2.IMREAD_UNCHANGED for loading image and Alpha channel simultaneously.

(3) Display image: cv2.imshow("Name", Pic)

(4) Name——Display the box name of the image

Pic——Pictures to be displayed(The image read by cv2.imread() has

already used before) For example, create a new py file and put the picture named "**camera.png**" into the same folder. Then input the following codes. After the codes run, the image will be displayed and you can press any key to hide the image.

<img class="common_img" src="../_static/media/chapter_5/section_8/media/image2.png" />

> [!Note]
> 
> cv2 waitkey() allows users to display a window for given milliseconds or until any key is pressed. And cv2.destroyALLWindows() function will close all the windows.
> 

* **Video Reading and Writing**

Video can be seen as pictures that are switched swiftly. Therefore, video reading is the extension of the image reading and writing. Camera initialization: cv2.VideoCapture(Number)

(1) Number——Serial number of camera, 0 usually.

Read the frame of camera：cap.read()。

(2) cap—the camera that has been defined before

Release the resources of the camera：cap.release()。

For example, the camera screen will be displayed on the desktop. When q key is pressed, the camera screen will be hidden.

<img class="common_img" src="../_static/media/chapter_5/section_8/media/image3.png" />

> [!Note]
> 
> cv2.waitKey(delay) will wait for the input from the keyboard and can be used to refresh the image in the video. "**delay**" in the bracket indicates the waiting time. When a frame of picture is displayed, the program will display the next frame in "delay" ms.
> 

### 5.2.5 Image Drawing

Drawing function in OpenCV can be used to draw line, rectangle, circle, etc., and add texts to the designated position of the picture.

* **Draw Line**

Function format: `cv2.line(image,pt1,pt2,color,thickness)`

(1) `Image`: Image where the line will be drawn

(2) `pt1`: starting coordinate of the line. The coordinate is represented by a tuples consisting of two values i.e. (X,Y)

(3) `pt2`: ending coordinate of the line. The coordinate is represented by a tuples consisting of two values i.e. (X,Y).

(4) `Color`: The color of the line. And BGR is represented by a tuple. For example, (255, 0, 0) stands for blue.

(5) `Thickness`: The thickness of the line

<img class="common_img" src="../_static/media/chapter_5/section_9/media/image2.png" />

* **Draw Rectangle**

Function format: `cv2.rectangle(image,pt1,pt2,color,thickness)`

(1) `image`: The picture where the rectangle will be drawn

(2) `pt1`: vertex coordinate of the rectangle, (x,y), which is represented by a tuple consisting of two numbers.

(3) `pt2`: The diagonal vertex coordinates of pt1 and its format is similar to that of pt1.

(4) `color`: The color of the rectangle. And BGR is represented by a tuple.

For example, (255, 0, 0) stands for blue.

(5) thickness: Line thickness. The greater the value, the thicker the line. If the value is negative or cv2.FILLED, a filled rectangle will be drawn.

<img class="common_img" src="../_static/media/chapter_5/section_9/media/image3.png" />

* **Draw Circle**

Function format: `cv2.circle(image,center,radius,color,thickness)`

(1) `image`: The picture where the circle will be drawn

(2) `center`: The center of the circle, (x,y), which is represented by a tuple consisting of two numbers.

(3) `radius`: The radius of the circle.

(4) `color`: The color of the circle. BGR is represented by a tuple. For example, (255, 0, 0) stands for blue.

(5) `thickness`: Line thickness. The greater the value, the thicker the line. If the value is negative or cv2.FILLED, a filled circle will be drawn.

<img class="common_img" src="../_static/media/chapter_5/section_9/media/image4.png" />

* **Draw Polygon**

Function format: `cv2.polylines(image,pts,isClosed,color,thickness)`

(1)  `image`: The picture where the polygon will be drawn

(2)  `pts`: The vertex coordinate of the polygon. When several quadrangles are required in a picture, the shape of ndarray is (N ，4 ，2).

(3)  `isClosed`: Whether the polygon is closed or not, True generally.

(4) `color`: The color of the polygon. BGR is represented by a tuple. For example, (255, 0, 0) stands for blue.

(5) `thickness`: Line thickness. The greater the value, the thicker the line.

<img class="common_img" src="../_static/media/chapter_5/section_9/media/image5.png" />

* **Add Text**

Function format: `cv2.putText(image,text,pt,font,fontScale,color)`

(1) `image`: The image where the text is added.

(2) `text`: The text content

(3) `pt`: The coordinate of the upper left corner of the text

(4) `font`: Font of the text

(5) `fontScale`: Font size

(6) `color`: The color of the text. BGR is represented by a tuple. For example, (255, 0, 0) stands for blue.

<img class="common_img" src="../_static/media/chapter_5/section_9/media/image6.png" />

### 5.2.6 Image Basic Operation

* **Acquire and Modify the Pixel of the Image**

The value of the pixel can be acquired through the coordinate of row and column. For BGR image, an array consisting of blue, green and red values will be returned. For grayscale image, the corresponding intensity will be returned. And the pixel can be modified in this way.

(1) `img[x,y]`: Acquire the value of some pixel and return its BGR value.

(2) `img[x,y,index]`: Acquire the value of a color channel. The order of the color channel is BGR.

(3) `img[x,y]=[B,G,R]`: Modify the color channel value of this pixel.

<img class="common_img" src="../_static/media/chapter_5/section_10/media/image2.png" />

* **Acquire the Image Property**

(1) `shape`: If it is a color picture, acquire the shape of the image and return an array containing the number of row, column and channel. If it is binary

image or grayscale image, only the number of row and column will be returned. Through judging whether the returned value contains the number of channel, we can know that it is a grayscale picture or color picture.

(2) `size`: Return the pixel number of the image. The format is "**row x column x channel**" . The number of channel of the grayscale picture is 1.

(3) `dtype`: Return the data type of the picture

<img class="common_img" src="../_static/media/chapter_5/section_10/media/image3.png" />

* **Splitting and Merging of Image Channel**

(1) Splitting of Image Channel

`split`: Input the image to be split and return the picture with three individual color channels.

<img class="common_img" src="../_static/media/chapter_5/section_10/media/image4.png" />

(2) Merging of Image Channel

`merge`: Merge three individual channels, including B, G and R into BGR image with three channel.

<img class="common_img" src="../_static/media/chapter_5/section_10/media/image5.png" />

* **Color Space Conversion**

There more than 150 ways to convert colors in OpenCV. And BGR is commonly converted into GRAY and HSV. The function format is `cvtColor(img,flag)`.

(1) `img`: The image converted the color space

(2) `flag`: The converted type. For example, **cv2.COLOR_BGR2HSV** indicates that convert BGR into HSV.

<img class="common_img" src="../_static/media/chapter_5/section_10/media/image6.png" />

### 5.2.7 Image Processing---Color Space Conversion

* **Color Space Introduction**

Each frame of the picture is arranged by the pixels that are composed of three color components, including B, G and R.

Color model is also called color space which is a mathematical model using an array to describe color.

Besides the familiar RGB picture, there are other color spaces, including GRAY, Lab, XYZ, YCrCb, HSV, HLS, CIEL\*a\*b\*, CIEL\*u\*v\*, Bayer, etc.

Expertise of each color space is different. Therefore, color space conversion can improve the efficiency of tackling a specific problem.

Color space conversion refers to transform the image from one color

space to another color space. For example, convert the picture from RGB to Lab. When extracting the feature of the picture, and calculating the distance,

we usually covert the picture from RGB into gray color space. In some applications, it is necessary to convert the color space image into binary image.

Some common color spaces are listed below.

* **Common Color Space**

(1) RGB Color Space

The properties of RGB color space are as follow.

① An RGB color space is an additive color space and the colors are obtained from linear combination of R(red), G(Green) and B(Blue).

② The illumination will affect the value of each color channel and these three color channels are related. For better understanding of color space, we can divide the image into R, G and B three components

From the blue channel picture in indoor, blue is similar to white. However, from the blue channel picture in outdoor, there is distinction between blue and white. And this nonuniformity makes color-based segmentation infeasible in color space. In addition, the value of these two pictures are also different.

Therefore, there are flaws in RGB color space, including uneven color value and mixed chroma and luminance.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image2.jpeg" />

(2) Lab Color Space

Similar to RGB, Lab also has three image channels.

L：Luminance channel

a：Color channel a representing colors from green to carmine.

b：Color channel b representing colors from blue to yellow.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image3.jpeg" />

Lab is totally different from RGB color space. In RGB, colors are divided into three channels and each channel contains luminance. While in Lab, colors are divided into L channel only containing luminance, a channel and b channel.

L component: represent the luminance of the pixel. The larger L value, the greaterthe luminance.

a component: represent the range from red to green.

b component: represent the range from yellow to blue.

In OpenCV, R, G and B value in RGB color space all range from 0 to 255.

In Lab color space, L ranges from 0 to 100. When L is 0, the color is black and when it is 100, the color is white. a and b values range from -128 to 127. When both a and b are 0, the color is gray.

To better assist in your understanding of the comparison between RGB and Lab, operate on PS.

① Use eyedropper tool to get the color.

② Click the color picker at bottom left corner, the correspondence between Lab and RGB is listed below.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image4.jpeg" />

Lab color space has these features:

* A perceptually uniform color space align with the way human perceive color.

* Independent from device(capture or display)

* Widely applied in Adobe Photoshop

* It is related to the RGB color space through complex transformation equations

In OpenCV, the image converted into Lab color space is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image5.jpeg" />

(3) Ycrcb Color Space

HVS (Human Visual System) is less sensitive to color than to luminance.

In traditional RGB color space, three primary colors, RGB, bear the same importance, but luminance is overlooked.

In YCrCb color space, Y represents luminance, and Cr and Cb stand for chroma. Cr indicates red component and Cb indicates blue component.

Luminance can reflect how bright or dark a color is, which can be calculated through a weighted sum of the light intensity. The green component has the greatest impact on RGB light while the blue component the least.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image6.jpeg" />

Observations focusing on intensity and color components can be made for LAB for illumination changes. Compared with LAB, the perception difference between red and orange in outdoor is smaller, while white between three components are distinguished.

(4) HSV Color Space

HSV color space is vision perception oriented color model which is composed of these three components.

H (hue), S (saturation) and V (value)

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image7.jpeg" />

Hue: it is related to the dominant wavelength of light in the mixed

spectrum, for example "red orange yellow green blue purple" represents

different hues respectively. In the perspective of wavelength, light of different wavelength appear as different colors, i.e. different hues.

Saturation: describes the purity of a color or pertains to the amount of white light mixed with a hue. Pure spectrum is fully saturated, and dark red (red mixed with white) and light purple (purple mixed with white) is not

saturated enough. Saturation is inversely proportional to the amount of white light mixed.

Brightness: it reflect the brightness perceived by human and it is relative to the reflection of the object. For a hue, the greater the amount of white mixed in a hue, the greater the value. And the greater the amount of black mixed in a hue, the weaker the brightness.

The most distinguished feature of HSV is that it only employs single

channel to describe hue, which make it intuitive to designate a hue. But HSV colors rely on device.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image8.jpeg" />

H components in outdoor and indoor are similar, which indicates the color is complete even though the lighting changes.

S components in outdoor and indoor are also similar. V stands for brightness so that it will change as the lighting changes.

The difference of red value between indoor and outdoor is large for the reason that H component represent red by angle ranging from \[300,360\] and \[0,60\].

(5) Gray Color Space

GRAY color space generally refers to grayscale image, monochromatic image, in which each pixel is processed into 256 gray level from black to white.

These 256 gray levels are represented by the number within \[0,255\]. "0"

indicates pure black, and "255" represents white. Number from 0 to 255 denote dark gray or light gray of different brightness (shade of hue).

* **Color Conversion**

The function below is used to transform color.

```py
dst = cv2.cvtColor( src, code [, dstCn] )
```

dst represents the output image whose data type and depth are similar to the original input image. src refers to original input image.

code is the flag of color space conversion.

dstCn is the number of channel of the target picture, 0 by default.

| Flag              | Shorthand | Function                                     |
| ----------------- | --------- | -------------------------------------------- |
| cv.COLOR_BGR2BGRA | 0         | Add alpha channel forRGB                     |
| cv.COLOR_BGR2RGB  | 4         | change the order of color channels           |
| cv.COLOR_BGR2GRAY | 10        | convert color picture into gray image        |
| cv.COLOR_GRAY2BGR | 8         | convert the color picture into gray image    |
| cv.COLOR_BGR2YUV  | 82        | convert RGB colorspace into YUV colorspace   |
| cv.COLOR_YUV2BGR  | 84        | convert YUV color space into RGB color space |
| cv.COLOR_BGR2HSV  | 40        | Convert RGB colorspace into HSV colorspace   |
| cv.COLOR_HSV2BGR  | 54        | Convert HSV colorspace into RGB colorspace   |
| cv.COLOR_BGR2Lab  | 44        | Convert RGB colorspace into Lab colorspace   |
| cv.COLOR_Lab2BGR  | 56        | Convert Lab color space into RGB color space |

Take cv2.cvtColor(frame, cv2.COLOR_RGB2LAB) for example.

"frame" is the picture to be processed. "cv2.COLOR_RGB2LAB" is the designated conversion model, referring to convert the picture from RGB color space into LAB color space.

Follow the following steps to transform the pictures into some common color spaces.

(1) Operation Steps

Before operation, please move to "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.7 Image Processing---Color Space Conversion->Sample Code](https://drive.google.com/drive/folders/13gcSm-nwHg-qbhdCUJnca088PBSkGOW9?usp=sharing)**", and copy the sample routine "**color_conversion.py**" and picture "img1.jpg" into the shared folder 

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_11/media/image10.jpeg" />, and then <img src="../_static/media/chapter_5/section_11/media/image12.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/Share/Image**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Image
```

③ Input command "**python3 color_conversion.py**" and press Enter to run the code.

```bash
python3 color_conversion.py
```

(2) Program Outcome

After execution, the final processed result is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image15.jpeg" />

(3) Program Analysis

① Firstly, import the required module with import statement.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image16.jpeg" />

② Call imread() function in cv2 module to read the image to be processed.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image20.png" />

③ Next, set the size of the inserted picture. And in the bracket is the name of the picture.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image21.png" />

④ Create four functions in sequence to convert the image into Gray, Lab, Ycrcb amd HSV respectively.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image17.jpeg" />

⑤ Display the image before and after conversion respectively.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image18.jpeg" />

⑥ Lastly, close the window through the function.

<img class="common_img" src="../_static/media/chapter_5/section_11/media/image19.jpeg" />

cv2.waitKey() is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any

keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

cv2.destroyAllWindows() is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

### 5.2.8 Image Processing ---Geometric Transformation

* **Introduction**

A spatial transformation of an image is a geometric transformation of the image coordinate system. It map the coordinate of a picture to a new

coordinate of other picture. And geometric transformation will not change the pixel of the image, but rearrange the pixels on the image plane.

According to OpenCV functions, we divide mapping into scaling, flipping,affine transformation, perspective, etc.

* **Scaling**

Scaling is to adjust the size of the picture, for example zoom in or zoom out the picture. In OpenCV, cv2.resize() function is used to scale the image.

```py
dst=cv2.resize(src,dsize[,fx[,fy[,interpolation]]])
```

dst represents the output image whose type is the same as src. And its

size is dsize (when it is not 0) or can be calculated through src.size(), fx and fy.

*  src represents the original picture

* dsize stands for the size of the output image

*  fx indicates the horizontal scaling ratio

*  fy denotes the vertical scaling ratio

*  interpolation is for interpolation method.

|          Type          |                         Description                          |
| :--------------------: | :----------------------------------------------------------: |
|   cv2.INTER_NEAREST    |                nearest neighbor interpolation                |
|    cv2.INTER_LINEAR    |                     linear interpolation                     |
|    cv2.INTER_CUBIC     | Cubic spline interpolation. First, thecubic spline fitting is performed on the 4 x 4 nearest neighbors near theoriginal image, and then the cubicspline value corresponding to thetarget pixel is taken as the value of the corresponding pixel in the targetimage. |
|     cv2.INTER_AREA     | Area interpolation similar to nearestinterpolation. Sample the current pixel according to the pixels in thesurrounding area of the current pixel. |
|   cv2.INTER_LANCZOS4   |         Lanczos interpolation over 8×8 neighborhood          |
| cv2.INTER_LINEAR_EXACT |             Bit accurate bilinear interpolation              |
|     cv2.INTER_MAX      |                   Difference encoding mask                   |
| cv2.WARP_FILL_OUTLIERS | Flag, fills all of the destination image pixels. If some of them correspond to outliers in the source image, they are set to zero |
|  cv2.WARP_INVERSE_MAP  | flag, inverse transformation. Forexample, polar transformation. If flag is not set, perform transformation:dst( ρ,ϕ)=src(x,y) For example, If flag is set, perform transformation: dst(x,y)=src(ρ,ϕ) |

(1) Operation Steps

The program will scale the image.

Before operation, please copy the routine "**Scale**" in "4.OpenCV-\>Image Processing --- Geometric Transformation-\>Routine Code" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image3.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image5.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/Share/Scale**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Scale
```

③ Input command "**python3 Scale.py**" and press Enter to run the code.

```bash
python3 Scale.py
```

(2) Program Outcome

The final output picture is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image8.jpeg" />

① src: Original picture. Its size is 492\*430 pixels (width\*height)

② res1: The size of the picture after zoomed in. Its size is 590\*512 pixels (width\*height)

③ res2: The size of the picture after zoomed out. And its size is 295\*258 pixels(width\*height)

(3) Program Analysis

The routine "**Scale.py**" can be found in "**[5. OpenCV Computer Vision Lesson->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code->Scale.py](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image9.jpeg" />

Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image10.png" />

Then call `imread()` function in cv2 module to read the image that needs to be scaled.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image11.png" />

In the bracket is the name of image.

The original width of the picture is 492 pixel, and height is 430 pixel.

Parameter dsize can be used to designate the size of target image res1 and res2 (The name of the image can be customized)

The first parameter in dsize corresponds to the width after scaling. (width i.e. the number of columns which is related to parameter fx.) And the second parameter corresponds to the height after scaling. (height i.e. the number of row which is related to parameter fy)

If the value of dsize is specified, the size of the target image is determined by dsize regardless of whether the parameters fx and fy are specified.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image12.jpeg" />

Therefore, program to acquire the original dimension first, and then

directly scale the width and height. To zoom in this picture, this routine

enlarges the width of res1 to 1.2 times the original and height to 1.2 times the original. Through calculation, the width is 590 pixels (92x1.2) and the height is 516 pixels (430x1.2).

To zoom out the picture, this routine will shrink the res2 width to 0.6 times the original, and the height to 0.6 times the original. The final width is 295

pixels (492x0.6) and height is 258 pixels (430x0.6). And the image size, before and after processing, can printed.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image13.jpeg" />

* **Affine Transformation**

Affine transformation is that images can be translated, rotated, etc.

through a series of geometric transformations, while lines and parallelism can be preserved.

Linearity means that the straight lines of the image can still be preserved after affine transformation. And parallelism indicates that parallel lines can be preserved after affine transformation.

Translation and rotation are special cases of affine transformation which is realized by the function `cv2.warpAffine()` in OpenCV. This function execute transformation by a transformation matrix M (transformation matrix of translation and rotation is different)

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image14.jpeg" />

As the picture below shown, the original image O can be transformed into affine image R by a transformation matrix M.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image15.jpeg" />

The format of `cv2.warpAffine()` function is as follow.

```py
dst = cv2.warpAffine( src, M, dsize[, flags[, borderMode[, borderValue]]] )
```

* `dst`: Represent the output image after affine transformation. The type of this image is similar to that of the original image. And the actual size of the output image is finally determined by dsize.

*  `src`: Represent the original image.

*  `M`: Stand for a 2x3 transformation matrix. Various affine transformation can be realized by using different transformation matrix. And the size of the output image is finally determined by dsize.

* `flags`: Represents the interpolation method which defaults to.

INTER_LINEAR. When it is **WARP_INVERSE_MAP**, M is an inverse transformation from the target image dst to the original image src.

**borderMode**, optional parameter, represents the edge

type,BORDER_CONSTANT by default. When it is **BORDER_TRANSPARENT**, the values in the target image do not change, and these values correspond to the outliers in the original image.

* `borderValue`: Refer to border value, 0 by default.

The optional parameters of cv2.warpAffine() function can be omitted, and its final format is as follow.

```py
dst=cv2.warpAffine(src,M,dsize)
```

By transformation matrix M, transform the original image src into the target image dst.

```py
dst(,J)=sr c(M1x+M ZY+MS,MU X+M2zy+MS)
```

Therefore, the type of affine transformation relies on the transformation matrix M.

(1) Translation

Translation is the movement of the object. If the coordinates of the object translation is obtained, the following transformation matrix can be created.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image16.jpeg" />

Put the transformation matrix into the array whose type is np.float32, and assign M matrix to cv2.warpAffine() function so as to realize translation.

① Operation Steps

This routine will translate the image to right.

Before operation, please copy the routine code in "[**5.OpenCV-\>5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation-\>Routine Code**](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" to the shared folder. 

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

* Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image18.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image20.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

*  Input command "**cd /mnt/hgfs/Share/Translation/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Translation/
```

* Input command "**python3 Translation.py**" and press Enter to run the routine.

```bash
python3 Translation.py
```

② Program Outcome

The final output picture is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image23.jpeg" />

③ Program Analysis 

The routine "**Translation.py**" can be found in "**[5. OpenCV Computer Vision Lesson->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code->TranslaTion](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image24.png" />

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image25.png" />

* Then call `imread()` function in cv2 module to read the image that needs to be translated.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image26.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image27.png" />

* As mentioned before, if the coordinate of the object translation can be obtained, the transformation matrix can be created.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image16.jpeg" />

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image28.png" />

After setting, the picture before and after translation can be displayed through imshow function.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image29.png" />

*  Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image30.png" />

cv2.waitKey() is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any

keyboard input. If there is, the ASCII value of the key is returned. -1 will be

returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

cv2.destroyAllWindows() is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

(2) Rotation

Both translation and rotation are the examples of the affine transformation, and employ cv2.warpAffine function to realize affine transformation. But their transformation matrix is different. When rotating the image with function

`cv2.warpAffine()`, obtain the transformation matrix with function cv2.getRotationMatrix2D().

The function format is

```py
retval=cv2.getRotationMatrix2D(center, angle, scale)
```

* center refers to the center of rotation.

* angle stands for rotation angle. When it is positive, the image will be rotated counterclockwise. When it is negative, the image will be

rotated clockwise.

* scale means scaled size

The rotation angle θ can be obtained from matrix M.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image31.jpeg" />

OpenCV has improved this matrix to provide scaling rotation and adjustable rotation center, as the picture shown below.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image32.jpeg" />

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image33.jpeg" />

The above matrix represents a rotation around center.x and center.y by θ degrees.

For example, set the function as below to rotate the image around the image center counterclockwise by 45 degree, and zoom out the image 0.6 times the original. Then call this function to generate the matrix.

```py
M=cv2.getRotationMatrix2D((height/2,width/2),45,0.6)
```

① Operation Steps

This routine will rotate the image 90 degree counterclockwise.

Before operation, please copy the routine "**Revolve**" in "[5.OpenCV->5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

* Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image35.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image37.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

* Input command "**cd /mnt/hgfs/Share/Revolve/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Revolve/
```

* Input the command "**python3 Revolve.py**" and press Enter to run the routine.

```bash
python3 Revolve.py
```

② Program Outcome

The output picture is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image40.jpeg" />

③ Program Analysis

The routine "**Revolve.py**" can be found in "**[5. OpenCV Computer Vision Lesson->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code->Revolve](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image41.jpeg" />

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image42.png" />

* Then call `imread()` function in cv2 module to read the image that needs to be rotated.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image43.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image44.png" />

* The image will rotate around the image center 90 degree counterclockwise. And its size remains the same.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image45.png" />

* Output the original image center

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image46.png" />

* After setting, we can call imshow function to display the pictures before and after rotation.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image47.png" />

* Lastly, call function below to close the window, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image48.png" />

cv2.waitKey() is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait **n** ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

cv2.destroyAllWindows() is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

* **Perspective Transformation**

Affine transformation are rotation, translation and scaling in 2D space, while perspective transformation is in 3D space.

Perspective transformation is realized by function cv2.warpPerspective(), and the function format is as follow.

```py
dst=cv2.warpPerspective(src, M, dsize[, flags[,borderMode[, borderValue]]])
```

* dst represents the output image after perspective transformation, whose type is the same as the original picture. And its size is determined by dsize.

*  src represents the image to be processed.

*  M stands for a 3x3 transformation matrix

* dsize indicates the dimension of the output image.

* flags represents the interpolation method which defaults to

INTER_LINEAR. When it is **WARP_INVERSE_MAP**, M is an inverse transformation from the target image dst to the original image src.

**borderMode**, optional parameter, represents the edge

type,BORDER_CONSTANT by default. When it is

**BORDER_TRANSPARENT**, the values in the target image do not change, and these values correspond to the outliers in the original image.

(1) Operation Steps

This routine will perform perspective transformation.

Before operation, please copy the routine "**Perspective**" in "[5.OpenCV->5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image50.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image52.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/Share/Perspective/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Perspective/
```

③ Input command "**python3 Perspective.py**" and press Enter to run the routine.

```bash
python3 Perspective.py
```

(2) Program Outcome

The final output picture is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image55.jpeg" />

(3) Program Analysis

The routine "**Perspective.py**" can be found in "**[5. OpenCV Computer Vision Lesson->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code->Perspective](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image56.png" />

① Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image57.png" />

② Then call `imread()` function in cv2 module to read the image for perspective transformation.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image58.png" />

③ Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image59.png" />

④ In this example, specify four vertices pts1 of the parallelogram in the original image, and specify four vertices pts2 of the rectangle in the target image. Next, generate the transformation matrix M with

`dst=cv2.warpPerspective(img,M,(cols,rows))`. Next, employ

`dst=cv2.warpPerspective(img,M,(cols,rows))` statement to convert parallelogram to rectangle.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image60.png" />

⑤ After setting, the picture before and after translation can be displayed through imshow function.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image61.png" />

⑥ Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image62.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

`cv2.destroyAllWindows()` is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

* **Remap**

Remap is that the pixels are mapped from one picture to the corresponding positions in another image according to the rules to form a new image.

As the pixel coordinates of the original image do not correspond to that of target image, in general, we describe the position (x, y) of each pixel by remapping.

g(x,y)=f(h(x,y))

g() refers to target image, f() is the original image and h(x,y) is the image after remapping. Take the below function for example. And **image I** will be remapped under the following conditions.

h(x,y)=(I.cols-x,y)

The image will flip in the x direction. cv2.remap() function in OpenCV makes it more convenient and free to remap. And its format is as follow.

```py
dst= cv2.remap(src,map1,map2,interpolation[,borderMode[,borderValue]])
```

*  dst represents the output image whose type and size are the same as the original picture.

* src represents the original image

* There are two possible values of map1 parameter. It represents a map of (x,y), or x value of (x,y) of CV_16SC2 , CV_32FC1, CV_32FC2 type.

* There are also two possible values of map2 parameter. When map1 represents (x,y), its value is none.

When map1 represents x value of (x,y), its value is the y value of (x,y) in CV_16UC1, CV_32FC1 type.

> [!Note]
> 
> **map1 refers to the column where the pixel is located, and map2 refers to the row where the pixel is located. So usually, map1 is written as mapx and map2 as mapy for better understanding.**

* Interpolation is for interpolation method.

* borderMode refers to border value. When it is

BORDER_TRANSPARENT, the pixel of target image corresponding to outliers in the original image will not be modified.

* borderValue refers to border value, 0 by default.

(1) Copy Pixel

① Operation Steps

All pixels in the target image are mapped to the pixels on the 100th row and 200th column in the original image.

Before operation, please copy the routine "**Remap**" in "[5.OpenCV->5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

* Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image64.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image66.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.
  * Input command "**cd /mnt/hgfs/Share/Remap/**" and press Enter to enter the shared folder.


```bash
cd /mnt/hgfs/Share/Remap/
```

* Input command "**python3 copy.py**" and press Enter to run the routine.

```bash
python3 copy.py
```

② Program Outcome

A pure-colored picture will be output.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image69.jpeg" />

③ Program Analysis

The routine "**copy.py**" can be found in "**[5. OpenCV Computer Vision Lesson->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation-\>Routine Code-\>Remap](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**".

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image71.png" />

* Then call `imread()` function in cv2 module to read the image that needs to be scaled.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image72.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image73.png" />

* mapx and mapy separately set the x axis and y axis coordinate. Map all the pixels on the target image to the pixels on 100th row, 200th column of the original image.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image74.png" />

* After setting, the picture before and after the pixels are copied can be displayed through imshow function. Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image75.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

`cv2.destroyAllWindows()` is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

(2) Copy the Whole Image

① Operation Steps

Besides the pixels can be copied, the whole image can also be copied.

For example, copy the whole original picture to the right.

Before operation, please copy the routine "**Remap**" in "**[5.OpenCV->5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

* Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image77.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image79.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

* Input command "**cd /mnt/hgfs/Share/Remap/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Remap/
```

* Input command "**python3 copy_all.py**" and press Enter to run the routine.

```bash
python3 copy_all.py
```

② Program Outcome

Correspond all the pixels of the original image to those of original image. The final output image is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image81.jpeg" />

③ Program Analysis

The routine "**copy_all.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image82.jpeg" />

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image83.png" />

* Then call `imread()` function in cv2 module to read the image.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image84.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image85.png" />

* mapx and mapy separately set the x axis and y axis coordinate.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image86.jpeg" />

* After setting, the picture before and after the pixels are copied can be displayed through imshow function. Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image87.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

`cv2.destroyAllWindows()` is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

(3) Rotate Around X Axis

If make the image flip around x axis,

① x axis coordinate remains unchanged.

② The y-axis coordinate after rotation is symmetric with respect to x axis.

Or：

① map1 remains unchanged

② map2= total number of row - 1 - current row number

(4) Operation Steps

With`cv2.remap()` function, the pixels can be remapped, and also be

flipped and then remapped. Ensure the x axis coordinate remains unchanged and y-axis coordinate after rotation is symmetric with respect to x axis.

Before operation, please copy the routine "**Remap**" in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image89.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image91.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input the command "**cd /mnt/hgfs/Share/Remap/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Remap/
```

③ Input command "**python3 x_rotation.py**" and press Enter to run the routine.

```bash
python3 x_rotation.py
```

(5) Program Outcome

The final output image is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image93.jpeg" />

(6) Program Analysis

The routine "**x_rotation.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image94.jpeg" />

① Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image95.png" />

② Then call` imread()` function in cv2 module to read the image that needs to be scaled.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image96.png" />

③ Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image97.png" />

④ mapx and mapy separately set the x axis and y axis coordinate. map1 remains unchanged, and map2 = "**total number of row - 1 - current row number**"

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image98.jpeg" />

⑤ After setting, the picture before and after can be displayed through imshow function. Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image99.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

`cv2.destroyAllWindows()` is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

(7) Rotate Around Y Axis

If make the image flip around y axis,

① y axis coordinate remains unchanged.

② The x-axis coordinate after rotation is symmetric with respect to y axis.

Or：

① Map2 remains unchanged

② map2 = "total number of column - 1 - current column number"

(8) Operation Steps

Before operation, please copy the routine "**Remap**" in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2. 8 Image Processing --- Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)**" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image101.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image103.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/Share/Remap/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Remap/
```

③ Input command "**python3 copy_all.py**" and press Enter to run the routine.

```bash
python3 copy_all.py
```

(9) Program Outcome

The final output image is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image105.jpeg" />

(10) Program Analysis

The routine "**y_rotation.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image106.jpeg" />

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image107.png" />

* Then call **imread()** function in cv2 module to read the image that needs to be scaled.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image108.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image109.png" />

* mapx and mapy separately set the x axis and y axis coordinate. mapy remains unchanged, and mapx = "**total number of column** - **1** - **current column number**"

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image110.jpeg" />

* After setting, the picture before and after can be displayed through imshow function. Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image111.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms).

The function will wait n ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

`cv2.destroyAllWindows()` is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

(11) Rotate Around XY Axis

If make the image rotate around x axis and y axis,

① The x-axis coordinate after rotation is symmetric with respect to y axis.

② The y-axis coordinate after rotation is symmetric with respect to x axis.

Or：

① map1 = "**total number of row** - **1** - **current row number**"

② map2= "**total number of row** - **1** - **current row number**"

(12) Operation Steps

Before operation, please copy the routine "**Remap**" in "4.OpenCV-\>Lesson 8 Image Processing --- Geometric Transformation-\>Routine Code" to the shared folder.

> [!Note]
> 
> **Note: The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

* Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image113.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image115.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

* Input command "**cd /mnt/hgfs/Share/Remap/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Remap/
```

* Input command "**python3 xy_rotation.py**" and press Enter to run the routine.

```bash
python3 xy_rotation.py
```

(13) Program Outcome

The final output picture is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image117.jpeg" />

(14) Program Analysis

The routine "**xy_rotation.py**" can be found in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image118.png" />

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image119.png" />

* Then call `imread()` function in cv2 module to read the image that needs to be scaled.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image120.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image121.png" />

* mapx and mapy separately set the x axis and y axis coordinate. The value of mapy is changed as "total number of row - 1 - current row number", and mapx = "total number of column - 1 - current column number"

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image122.png" />

* After setting, the picture before and after can be displayed through imshow function. Lastly, close the window through the function, and you can press any key to exit the program.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image123.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any keyboard input. If there is, the ASCII value of the key is returned. -1 will be returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

`cv2.destroyAllWindows()` is used to delete the window. If there is no parameter

in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

(15) Compress Image

Compressing image is to compress the original image by half.

(16) Operation Steps

Before operation, please copy the routine "**Scale**" in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing--- Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

* Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_12/media/image125.jpeg" />, and then <img src="../_static/media/chapter_5/section_12/media/image127.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

* Input command "**cd /mnt/hgfs/Share/Remap/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/Remap/
```

* Input command "**python3 half_size.py**" and press Enter to run the code.

```bash
python3 half_size.py
```

(17) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image129.jpeg" />

(18) Program Analysis

The routine "**half_size.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Geometric Transformation->Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image130.jpeg" />

* Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image131.png" />

* Then call `imread()` function in cv2 module to read the image.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image132.png" />

* Return the number of row, column and channel of the image pixel to rows, cols and ch.

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image133.png" />

* mapx and mapy separately set the x axis and y axis coordinate and double the X axis

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image134.jpeg" />

* After setting, the picture before and after can be displayed through imshow function. Lastly, close the window through the function, and you can press any key to exit the program

<img class="common_img" src="../_static/media/chapter_5/section_12/media/image135.png" />

`cv2.waitKey()` is a keyboard binding function. Its time unit is milliseconds (ms). The function will wait n ms set in bracket to check if there is any

keyboard input. If there is, the ASCII value of the key is returned. -1 will be

returned if there is no keyboard input. Generally we set it to 0, the function will wait for keyboard input endlessly.

cv2.destroyAllWindows() is used to delete the window. If there is no parameter in the bracket, all the windows will be deleted. If you input the specific value of the window, the designated window will be removed.

<p id="anchor_5_2_9"></p>

### 5.2.9 Image Processing - Smoothing

* **Image Noise**

During collecting, processing and transferring, the digital image will be disturbed by different noises, which leads to low-quality image, obscure image and disappeared image feature. And image smoothing is to improve the image by removing noise, and salt-and-pepper noise as well as Gauss noise are common.

(1) Salt-and-pepper Noise

Salt-and-pepper noise is also known as pulse noise which is white dots and black dots appearing randomly, like there are black pixels in bright area and white pixels in dark area.

At left is the original picture and the right is the picture with salt-and-pepper noise.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image2.png" />

Gauss Noise

Gauss noise is a term from signal processing theory denoting a kind of signal noise that has a probability density function (pdf) equal to that of the normal distribution (which is also known as the Gaussian distribution).

Commonly, it can be suppressed by mathematical statistics.

At left is the original picture and the right is the picture with Gauss noise.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image3.jpeg" />

(2) Image Smoothing

From the perspective of signal, image smoothing is to filter the high frequency part of the signal and reserve the low frequency part.

Based on filter, filtering can be divided into mean filtering, Gaussian filtering and median filtering.

① Mean Filtering

The idea of mean filtering is simply to take the mean of all the pixels of the image that is assign the mean of all the pixels in the unit of a square to the center pixel.

Take the picture below as example. In picture (a), the center pixel value is

"**226**" and the mean of all the pixels is " **122**" obtained from the equation below, and " **122**" is the new center pixel value.

(40 + 107 + 5 + 198 + 226 + 223 + 37 + 68 + 193) ÷ 9 = 122

Replace the original center pixel value by " 122" as the picture (b) shown.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image4.png" />

The algorithm of the mean filtering is simple and its calculation speed is fast. However, the details of the image are destroyed during removing noise resulting in low definition.

② Gauss Filtering

The weighted mean is calculated by multiplying each value by the corresponding weight, adding the sum, and then dividing the sum by the number of the values.

Gauss filtering is to obtain the weighted mean of all the pixels of the image that is assign the weighted mean in the unit of a square to the center pixel.

Take the picture below as example. In picture (a), the center pixel value is "**226**" and the weighted mean of all the pixels is " **164**" obtained from the

equation below, and " **164**" is the new center pixel value.

40 × 0.05 + 107 × 0.1 + 5 × 0.05 + 198 × 0.1 + 226 × 0.4 + 223 × 0.1 + 37 × 0.05 + 68 × 0.1 + 193 × 0.05 = 164

Replace the original center pixel value by " 122" as the picture (c) shown.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image5.png" />

③ Median Filtering

The median is the middle value when a data set is ordered from least to greatest.

Median filtering is to take the median of all the pixels of the image that is assign the median in the unit of square to the center pixel.

Take the picture below as example. In picture (a), the center pixel value is "226" and the medium of the pixels is " **107**" , and " **107**" is the new center pixel value.

Replace the original center pixel value by " 107" as the picture (b) shown.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image6.png" />

* **Operation Steps**

This routine will execute mean filtering, Gauss filtering and median filtering separately.

Before operation, please copy the routine "**filtering.py**" in"4.OpenCV-\>Lesson 9 Image Processing --- Smoothing-\>Routine Code" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

(1) Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_13/media/image7.png" />, and then <img class="common_img" src="../_static/media/chapter_5/section_13/media/image9.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

(2) Input command "**cd /mnt/hgfs/Share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/
```

(3) Input command "**python3 filtering.py**" and press Enter to run the routine.

```bash
python3 filtering.py
```

* **Program Outcome**

The final output image is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image12.png" />

* **Program Analysis**

The routine "**filtering.py**" can be found in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.9 Image Processing --- Smoothing-\>Routine Code](https://drive.google.com/drive/folders/11slaURSB_O0QIQzI0Obi9w-DFVRJEZk5?usp=sharing)" .

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image13.jpeg" />

(1) Image Processing

① Import Module

Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image14.png" />

② Read Images

Then call `imread()` function in cv2 module to read the image that needs to be filtered.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image15.png" />

In the bracket is the image name.

③ Mean Filtering

Call blur() function in cv2 module to perform mean filtering on the specific image.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image23.png" />

The format of blur() function is as follow.

```py
cv2.blur(scr, ksize)
```

The first parameter `scr` is the input image.

The second parameter `ksize` is the size of convolution kernel.

④ Gauss Filtering

Call GaussianBlur() function in cv2 module to perform Gauss filtering on the specific image.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image16.jpeg" />

The format of GaussianBlur() function is as follow.

```py
cv2.GaussianBlur(src, ksize, sigmaX, sigmaY, borderType)
```

The first parameter `src` is the input image.

The second parameter `ksize` is the size of Gaussian convolution kernel.

Both the height and width of the convolution kernel must be positive number and odd number.

The third parameter `sigmaX` is the horizontal standard deviation of Gaussian kernel.

The fourth parameter `sigmaY` is the vertical standard deviation of Gaussian kernel, **0** by default.

The fifth parameter `borderType` is the type of border filling.

⑤ Median Filtering

Call medianBlur() function in cv2 module to perform median filtering on the specific image.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image24.png" />

The format of medianBlur() function is as follow

```py
cv2.medianBlur(src, ksize)
```

The first parameter "**scr**" is the input image.

The second parameter "**ksize**" is the size of the convolution kernel.

(2) Image Display

① Create Custom Image

Call figure() function in matplotlib.pyplot module to create a custom image for displaying the final output image.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image17.png" />

The format of the figure() function is as follow.

```py
matplotlib.pyplot.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>, clear=False, **kwargs)
```

The first parameter `num` is the only identifier of the image i.e. the serial number of the picture (number) or the name (string).

The second parameter `figsize` is the width and height of the image in inch.

The third parameter `dpi` is the resolution of the image i.e. the number of pixels by inch

The fourth parameter `facecolor` is the background color.

The fifth parameter `edgecolor` is the frame color

The sixth parameter `frameon` determines whether to draw the picture, and it is `True` by default.

The seventh parameter `FigureClass` is used to select the custom figure when generating the image

The eighth parameter `clear` determines whether to clear all the original images.

The ninth parameter `kwargs` represents other properties of the image.

② Modify matplotlib Configuration

matplotlib is plotting library of Python. User can access and modify

matplotlib configuration options through parameter dictionary `rcParams`.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image18.png" />

The codes above are used to manipulate the display of the normal characters.

③ Set the Parameter of Image Display

Call subplot(), imshow() and title() functions in matplotlib.pyplot modules to designate the position, color and headline of the subplot in the Figure.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image19.png" />

* subplot() function is used to set the position of the subplot, and the function format is as follow.

```py
matplotlib.pyplot.subplot(nrows, ncols, index, **kwargs)
```

The first parameter `nrows` and the second parameter `ncols` respectively are the number of row and column of subplot.

The third parameter `index` is the index position. Index starts at 1 in the upper left corner and increases to the right.

When both the row and column are less than " **10**", these two values can be abbreviated to an integer. For example, the meaning of "subplot(1, 4, 1)" and "subplot(141)" are the same, both representing the image is divided into one row and four columns, and the subplot is in the first place i.e. 1st row, 1st column.

2） imshow() function is used to set the color of subplot, and its format is as follow.

```py
matplotlib.pyplot.imshow(X, cmap=None)
```

The first parameter `X` is the image data.

The second parameter `cmap` is the colormap, RGB(A) color space by default.

3） title() function is used to set the title of the subplot. The parameter in the bracket is the name of the subplot and the function format is as follow.

```py
matplotlib.pyplot.title(label, fontdict=None, loc=None, pad=None, *
y=None, **kwargs)
```

The first parameter `label` is the title composed of string.

The second parameter `fontdict` is the property of the font, and the current parameter refers to dictionary.

The third parameter `loc` is the position of the title. It can be `left` , `center`

or `right`, and `center` by default.

The fourth parameter `pad` is the padding distance (inside margin) between the tile and the subplot, "**6.0**" by default.

The fifth parameter "**y**" is the vertical distance between the title and the subplot, and the unit is the percentage of the height of the subplot. The default value is "None", that is, the position of the title is automatically determined to avoid overlapping with other elements. "**1.0**" means the title is at the top of the subplot.

The sixth parameter `**kwargs` is the text object keyword property, which is used to determine the appearance of the text, such as font, text color, etc.

(4) SetAxis Tick

Call xticks() and yticks() function in matplotlib.pyplot module to set the tick and tag of X and Y axis. As the coordinate axis is not required in image display in this routine, the list is set as none that is the coordinate axis will not be displayed.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image20.png" />

The format of xticks() function is as follow.

```py
xticks(ticks=None, labels=None, **kwargs)
```

When the parameter is none, the function will return the current tick and tag of X axis. Otherwise the function is used to set the current tick and label of X axis.

The first parameter `ticks` is a list of the positions of the X-axis ticks. If the list is empty, the X-axis ticks will be cleared.

The second parameter `labels` is the label of X-axis tick. Only when parameter "ticks" is not none, can this parameter be passed.

The third parameter `**kwargs` is used to control the appearance of the tick and label.

The format of yticks() is the same as that of xticks(). The difference lies in the controlled object.

(5) Display Image

Call show() function in matplotlib.pyplot module to display the image on the window.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image21.png" />

The complete codes of image display part are as follow.

<img class="common_img" src="../_static/media/chapter_5/section_13/media/image22.png" />

### 5.2.10 Image Processing---Edge Detection

* **Edge Detection Introduction**

Edge detection is fundamental technique in image processing and

computer vision, which aims at identifying edges in a digital image at which the image brightness changes sharply. Sharp changes in image usually reflect important events and changes in properties. The edge is as the picture shown.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image2.jpeg" />

Edge detection greatly reduces the amount of data, removes irrelevant information, and preserves the important structural properties of the image. Edge detection is divided into two types.

(1) Based on search: The boundary is detected by finding the maximum and minimum values in the first derivative of the image, and it usually locates in the direction with the largest gradient. The representative algorithms are the Sobel operator and the Scharr operator.

(2) Based on zero-crossing: the boundary is found by searching the second-order derivative zero-crossing of the image, which is usually the

Laplacian zero-crossing point or the zero-crossing point represented by the nonlinear difference, and the representative algorithm is the Laplacian operator.

* **Canny Edge Detection**

Canny Edge Detection is a popular edge detection algorithm. It was developed by John F. Canny in 1986 and considered as the best algorithm of edge detection. Canny edge detection will go through 4 steps, including Noise

Reduction, Finding Gradient Magnitude and Direction of the Image, Non-maximum Suppression and Hysteresis Threshold ing.

(1) Noise Reduction

Since edge detection is susceptible to noise in the image, first step is to remove the noise in the image. For detailed operation, please refer to the file in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.9 Image Processing---Smoothing](#anchor_5_2_9)**" .

(2) Finding Gradient Magnitude and Direction of the Image

In math, gradient is a vector, indicating that the directional derivative of the function at a certain point reaches the maximum along this direction, that is, the function changes the fastest along the direction at this point, and the rate of change is the greatest.

In the image, gradient represents the degree and direction of gray value change, and the edge refers to the position where the gray intensity changes the most. Gradient direction is always perpendicular to edges.

Sobel filter is used to calculate the magnitude and direction of the gradient.

The Sobel operator GX is the first derivative in the horizontal direction, which is used to detect the edge in the Y-axis direction, while GY is the the first derivative in the vertical direction, which is used to detect the edge in the X-axis direction.

The formula for calculating the gradient size is as follows

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image3.png" />

The formula for calculating the gradient direction is as follows

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image4.png" />

(3) Non-Maximum Suppression

Non-Maximum Suppression (NMS) is that reserve local maximum and suppress all the values apart from local maximum. In simple terms, all the pixels of the image will be detected. If the gradient intensity of a point is greater than the pixels in the positive and negative directions of its gradient direction, the point is retained; otherwise, the point is suppressed.

Canny edge detection algorithm perform non-maximum suppression along the gradient direction not the edge direction.

(4) Hysteresis Threshold ing

This stage decides which are really edges. For this, we need two threshold values, "**minVal**" and `maxval`. Any edges with intensity gradient more than maxVal are sure to be edges and those below minVal are sure to be non-edges. Those who lie between these two thresholds are classified edges or non-edges based on their connectivity. If they are connected to "sure-edge" pixels, they are considered to be part of edges. Otherwise, they are also discarded.

* **Operation Steps**

This routine will perform the edge detection.

Before operation, please copy the routine "**edge_detection.py**"and sample picture "**luna.jpg**" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing --- Edge Detection-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

(1) Open virtual machine and start the system. Click " <img class="common_img" src="../_static/media/chapter_5/section_14/media/image5.png"/>", and then "<img class="common_img" src="../_static/media/chapter_5/section_14/media/image7.png" /> " or press "**Ctrl+Alt+T**" to open command line terminal.

(2) Input command "**cd /mnt/hgfs/Share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/
```

(3) Input command "**python3 edge_detection.py**" and press Enter to run the routine.

```bash
python3 edge_detection.py
```

* **Program Outcome**

The final output image is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image10.png" />

* **Program Analysis**

The routine "**edge_detection.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Edge Detection-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image11.jpeg" />

(1) Image Processing

① Import Module

Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image12.png" />

② Read Image

Call `imread()` function in cv2 module to read the image

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image13.png" />

The parameter in the bracket is the name of the image.

③ Set Threshold

Set two thresholds to determine the final edge.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image14.png" />

④ Edge Detection

Call Canny() function in cv2 module to perform edge detection on the specific image.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image15.png" />

The format of `Canny()`function is as follow.

```py
Canny( image, threshold 1, threshold2)
```

The first parameter `image` is the input image.

The second parameter `threshold1` is the low threshold `minVal`

The third parameter `threshold2` is high threshold `maxVal` .

(2) Image Display

① Create Custom Figure

Call figure() function in matplotlib.pyplot module to create a custom figure for displaying the final output image.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image16.png" />

The format of the figure() function is as follow.

```py
figure(num=None, figsize=None, dpi=None, facecolor=None,
edgecolor=None, frameon=True, FigureClass=<class
'matplotlib.figure.Figure'>, clear=False, **kwargs)
```

The first parameter `num` is the only identifier of the image i.e. the serial number of the picture (number) or the name (string).

The second parameter `figsize` is the width and height of the image in inch.

The third parameter `dpi` is the resolution of the image i.e. the number of pixels by inch

The fourth parameter `facecolor` is the background color.

The fifth parameter `edgecolor` is the frame color

The sixth parameter `frameon` determines whether to draw the picture, and it is `True` by default.

The seventh parameter `FigureClass` is used to select the custom figure when generating the image

The eighth parameter `clear` determines whether to clear all the original images.

The ninth parameter `**kwargs` represents other properties of the image.

② Modify matplotlib Configuration

matplotlib is plotting library of Python. User can access and modify

matplotlib configuration options through parameter dictionary `rcParams`.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image17.png" />

The codes above are used to manipulate the display of the normal characters.

③ Set the Parameter of Image Display

Call subplot(), imshow() and title() functions in matplotlib.pyplot modules to designate the position, color and headline of the subplot in the Figure.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image18.png" />

* subplot() function is used to set the position of the subplot, and the function format is as follow.

```py
subplot(nrows, ncols, index, **kwargs)
```

The first parameter `nrows` and the second parameter `ncols` respectively are the number of row and column of subplot.

The third parameter `index` is the index position. Index starts at 1 in the upper left corner and increases to the right.

When both the row and column are less than " **10**", these two values can be abbreviated to an integer. For example, the meaning of "subplot(1, 2, 1)" and "subplot(121)" are the same, both representing the image is divided into one row and 2 columns, and the subplot is in the first place i.e. 1st row, 1st column.

* imshow() function is used to set the color of subplot, and its format is as follow.

```py
imshow(X, cmap=None)
```

The first parameter "**X**" is the image data.

The second parameter `cmap` is the colormap, RGB(A) color space by default.

* title() function is used to set the title of the subplot. The parameter in the bracket is the name of the subplot and the function format is as follow.

```py
title(label, fontdict=None, loc=None, pad=None, *, y=None, **kwargs)
```

The first parameter "label" is the title composed of string.

The second parameter "fontdict" is the property of the font, and the current parameter refers to dictionary.

The third parameter "loc" is the position of the title. It can be "left", "center" or "right", and "center" by default.

The fourth parameter "pad" is the padding distance (inside margin) between the tile and the subplot, "6.0" by default.

The fifth parameter "y" is the vertical distance between the title and the subplot, and the unit is the percentage of the height of the subplot. The default value is "None", that is, the position of the title is automatically determined to avoid overlapping with other elements. " 1.0" means the title is at the top of the subplot.

The sixth parameter "\*\*kwargs" is the text object keyword property, which is used to determine the appearance of the text, such as font, text color, etc.

④ SetAxis Tick

Call xticks() and yticks() function in matplotlib.pyplot module to set the tick and tag of X and Y axis. As the coordinate axis is not required in image display in this routine, the list is set as none that is the coordinate axis will not be displayed.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image19.png" />

The format of xticks() function is as follow.

```py
xticks(ticks=None, labels=None, **kwargs)
```

When the parameter is none, the function will return the current tick and tag of X axis. Otherwise the function is used to set the current tick and label of X axis.

The first parameter `ticks` is a list of the positions of the X-axis ticks. If the list is empty, the X-axis ticks will be cleared.

The second parameter `labels` is the label of X-axis tick. Only when parameter "ticks" is not none, can this parameter be passed.

The third parameter `**kwargs` is used to control the appearance of the tick and label.

The format of yticks() is the same as that of xticks(). The difference lies in the controlled object.

⑤ Display Image

Call show() function in matplotlib.pyplot module to display the image on the window.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image20.png" />

The complete codes of image display part are as follow.

<img class="common_img" src="../_static/media/chapter_5/section_14/media/image21.png" />

### 5.2.11 Image Processing --- Morphological Processing

* **Morphology Introduction**

Morphology is one of the most widely used techniques in image processing. It is mainly used to extract image components that are meaningful for describing the shape of an area, so that the most essential shape features of the target object can captured in subsequent recognition, such as boundaries and connected areas. In addition, techniques such as thinning, pixelation, and burr trimming are often used in image preprocessing and postprocessing, which can greatly strengthen the image.

The basic idea of morphology is to use a special structural element to measure or extract the corresponding shape or feature in the input image for further image analysis and target recognition.

* **Morphological Transformation**

(1) Erosion and Dilation

Both erosion and dilation are the basic and important morphological operation, and also the foundations of multiple advanced morphological processing. Many morphological algorithms are composed of these two.

① Structuring Element

Structuring element is required in erosion and dilation. A two-dimensional structuring element can be seen as a two-dimensional matrix element which is "**0**" or " **1**" .

② Erosion

Erosion works to remove small and meaningless object, and the whole process is divided into three steps.

* Build an image matrix upon the structuring element and determine its origin. Take the element at the upper left corner as the origin, and mark it with dark color.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image2.jpeg" />

* Overlay the structuring element on the pending image. If the value of the pixel in the image corresponding to the elements whose value is " **1**" in the structuring elements are all " 1", the pixel at the corresponding position of the origin is assigned as "**1**", otherwise it is "**0**".

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image3.jpeg" />

* Make the structuring elements move on the pending image in order until all the images are processed completely.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image4.jpeg" />

③ Dilation

Dilation can enlarge the edge of the image and pad the edge of the target object or non-target pixel. The operations are divided into three steps.

* Build an image matrix upon the structuring element and determine its origin. Take the element at the upper left corner as the origin, and mark it with dark color.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image2.jpeg" />

* Overlay the structuring element on the pending image. If at least one of the value of the pixel in the image corresponding to the elements whose value is " **1**" in the structuring elements is " 1" , the pixel at the corresponding position of the origin is assigned as "**1**", otherwise it is "**0**".

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image5.jpeg" />

* Make the structuring elements move on the pending image in order until all the images are processed completely.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image6.jpeg" />

(2) Opening and Closing

In opening and closing, erosion and dilation are executed in sequence.

① Open Operation

Opening indicates that erosion is executed first and dilation follows. It is useful in separating objects, removing small area, removing highlight under dark background.

② Close Operation

In closing, dilation is executed first and erosion follows. It plays an

important role in eliminating holes, that is, filling closed areas and deleting dark areas under a bright background.

(3) Top Hat and Bottom Hat

① Top Hat Operation

It is the difference between input image and the image after opening (Top hat operation= input image - image after opening), and it can obtain areas with brighter gray in the original image

② Bottom Hat Operation (Black Hat)

It is the difference between input image and the image after closing (Top hat operation= input image - image after closing), and it can obtain areas with darker gray in the original image

* **Operation Steps**

This routine will perform erosion, dilation, opening, closing, top hat operation and bottom hat operation on the designated image.

Before operation, please copy the routine "**example_org.jpg**" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing --- Geometric Transformation-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" to the shared folder.

> [!Note]
> 
> **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**

(1) Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_15/media/image8.jpeg" />, and then <img src="../_static/media/chapter_5/section_15/media/image10.png"/>

**or** press "**Ctrl+Alt+T**" to open command line terminal.

(2) Input command "**cd /mnt/hgfs/Share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/Share/
```

(3) Input command "**python3 morphology operations.py**" and press Enter to run the routine.

```bash
python3 morphology operations.py
```

* **Program Outcome**

The final output image is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image13.png" />

* **Program Analysis**

The routine "**morphology_operations.py**" can be found in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.8 Image Processing---Morphological Processing-\>Routine Code](https://drive.google.com/drive/folders/1gXZBxAakvGOHBCHmVQCu1WvmUXokL4zY?usp=sharing)" .

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image14.jpeg" />

(1) Image Processing

① Import Module

Firstly, import the required module through import statement.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image15.png" />

② Read the Image

Then call `imread()` function in cv2 module to read the pending image.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image16.png" />

The parameter in the bracket is the name of the image

③ Create Nuclear Structure

Call ones() function in numpy module to create the nuclear structure i.e.

array required in the operation.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image25.png" />

The format of ones() function is as follow.

```py
np.ones(shape, dtype=None, order='C')
```

The first parameter `shape` is a integer or integer tuple used to define the size of the array. If it designates the variable of the integer, one--dimensional array will be returned. If it designate integer tuple, the array in given shape will be returned.

The second parameter `dtype` refers to the data type of the array, `float` by default.

The third parameter `order` is used to designate the storing order of the returned array elements in storage.

④ Erosion and Dilation

Call erode() and dilate() function in cv2 module to perform erosion and dilation on the specific image.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image17.jpeg" />

The format of erode() function is as follow.

```py
cv2.erode(src, kernel, iteration)
```

The first parameter `src` is the input image.

The second parameter `kernel` is the size of the kernel.

The third parameter is the umber of iteration.

The meaning of the parameter in dilate() function is the same as that of erode() function.

⑤ Open Operation and Close Operation

Call morphologyEx() function in cv2 module to perform open operation, close operation, top hat operation and bottom hat operation on the specific image.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image18.jpeg" />

The format of morphologyEx() function is as follow.

```py
cv2.morphologyEx(img, op, kernel)
```

The first parameter `img` indicates the input image

The second parameter `op` represents the operation type.

|   Operation Type   |        Meaning         |
| :----------------: | :--------------------: |
|   cv2.MORPH_OPEN   |     open operation     |
|  cv2.MORPH_CLOSE   |    close operation     |
| cv2.MORPH_GRADIENT | morphological gradient |
|  cv2.MORPH_TOPHAT  |   top hat operation    |
| cv2.MORPH_BLACKHAT |  bottom hat operation  |

The third parameter indicates the size of the frame.

(2) Image Display

① Create Custom Figure

Call figure() function in matplotlib.pyplot module to create a custom figure for displaying the final output image.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image19.png" />

The format of the figure() function is as follow.

```py
matplotlib.pyplot.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>, clear=False, **kwargs)
```

The first parameter `num` is the only identifier of the image i.e. the serial number of the picture (number) or the name (string).

The second parameter `figsize` is the width and height of the image in inch.

The third parameter `dpi` is the resolution of the image i.e. the number of pixels by inch

The fourth parameter `facecolor` is the background color.

The fifth parameter `edgecolor` is the frame color.

The sixth parameter `frameon` determines whether to draw the picture, and it is `True` by default.

The seventh parameter `FigureClass` is used to select the custom figure when generating the image

The eighth parameter `clear` determines whether to clear all the original images.

The ninth parameter `**kwargs` represents other properties of the image.

② Modify matplotlib Configuration

matplotlib is plotting library of Python. User can access and modify

matplotlib configuration options through parameter dictionary `rcParams`.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image20.png" />

The codes above are used to manipulate the display of the normal characters.

③ Set the Parameter of Image Display

Call subplot(), imshow() and title() functions in matplotlib.pyplot modules to designate the position, color and headline of the subplot in the Figure.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image21.png" />

* subplot() function is used to set the position of the subplot, and the function format is as follow.

```py
matplotlib.pyplot.subplot(nrows, ncols, index, **kwargs)
```

The first parameter `nrows` and the second parameter `ncols`

respectively are the number of row and column of subplot.

The third parameter `index` is the index position. Index starts at 1 in the upper left corner and increases to the right.

When both the row and column are less than " **10**", these two values can be abbreviated to an integer. For example, the meaning of `subplot(3, 3, 1)` and `subplot(331)` are the same, both representing the image is divided into three rows and three columns, and the subplot is in the first place i.e. 1st row, 1st column.

* imshow() function is used to set the color of subplot, and its format is as follow.

```py
matplotlib.pyplot.imshow(X, cmap=None)
```

The first parameter "**X**" is the image data.

The second parameter `cmap` is the colormap, RGB(A) color space by default.

* title() function is used to set the title of the subplot. The parameter in the bracket is the name of the subplot and the function format is as follow.

```py
matplotlib.pyplot.title(label, fontdict=None, loc=None, pad=None, *
y=None, **kwargs)
```

The first parameter `label` is the title composed of string.

The second parameter `fontdict` is the property of the font, and the current parameter refers to dictionary.

The third parameter `loc` is the position of the title. It can be `left`, `center`

or `right`, and `center` by default.

The fourth parameter `pad` is the padding distance (inside margin) between the tile and the subplot, "**6.0**" by default.

The fifth parameter "**y**" is the vertical distance between the title and the

subplot, and the unit is the percentage of the height of the subplot. The default value is "**None**", that is, the position of the title is automatically determined to

avoid overlapping with other elements. "**1.0**" means the title is at the top of the subplot.

The sixth parameter `**kwargs` is the text object keyword property, which is used to determine the appearance of the text, such as font, text color, etc.

④ Set Axis Tick

Call xticks() and yticks() function in matplotlib.pyplot module to set the tick and tag of X and Y axis. As the coordinate axis is not required in image display in this routine, the list is set as none that is the coordinate axis will not be displayed.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image22.png" />

The format of xticks() function is as follow.

```py
matplotlib.pyplot.xticks(ticks=None, labels=None, **kwargs)
```

The first parameter `ticks` is a list of the positions of the X-axis ticks. If the list is empty, the X-axis ticks will be cleared.

The second parameter `labels` is the label of X-axis tick. Only when parameter "ticks" is not none, can this parameter be passed.

The third parameter `lkwargs` is used to control the appearance of the tick and label.

The format of yticks() is the same as that of xticks(). The difference lies in the controlled object.

⑤ Display Image

Call show() function in matplotlib.pyplot module to display the image on the window.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image23.png" />

The complete codes of image display part are as follow.

<img class="common_img" src="../_static/media/chapter_5/section_15/media/image24.png" />

### 5.2.12 Image Processing ---Thresholding

* **Image Binaryzation**

Image binaryzation is to set the gray values of image pixels as two values, 0 (represent black) and 255 (represent white) generally, which will make the image turn black-and-white.

Commonly, it will process the grayscale image, and then set a threshold to divide the image into two parts, including the part greater than the threshold and the part smaller than the threshold. Next, assign different pixel values to these two parts of image.

Image binaryzation facilitates the further processing of the image, makes the image simple, reduces the amount of data and highlights the target contour you are interested in.

Under different situation, there are three ways to execute threshold ing,

including Global Threshold ing, Adaptive Threshold ing and Otsu Threshold ing

* **Global Thresholding**

Global threshold ing will process the whole image according to the set threshold.

(1) Operation Steps

> [!Note]
>
> * **Before operation, please copy the routine "threshold_demo.py" and sample picture "**test.jpg**" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.12 Image Processing --- Thresholding-\>Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
>

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_16/media/image2.png" />, and then <img src="../_static/media/chapter_5/section_16/media/image4.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 threshold_demo.py**" and press Enter to run the routine.

```bash
python3 threshold_demo.py
```

(2) Program Outcome

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img class="common_img" src="../_static/media/chapter_5/section_16/media/image7.jpeg"/> | <img class="common_img" src="../_static/media/chapter_5/section_16/media/image8.jpeg"/> |

The final output picture is as shown above.

(3) Code Analysis

The routine "**threshold_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.12 Image Processing --- Thresholding-\>Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image9.jpeg" />

**Import Module**：Import cv2 module

**Read Picture**：Call imread function to read picture and the parameter stands for the name of the picture.

**Color Space Conversion:** call cvtColor function to convert the image into GRAY color space, and the parameter indicates the picture and conversion mode.

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image10.png" />

**Thresholding:** use threshold function to execute. The specific format and parameters are as follow.

```py
threshold(src, thresh, maxval, type)
```

① The first parameter `src` is the pending image.

② The second parameter `thresh` is the set threshold.

③ The third parameter `maxval` will be set only when the type is

`THRESH_BINARY` or `THRESH_BINARY_INV` . It refers to the new value assigned when the gray values of the picture pixels are greater (smaller) than the threshold

④ The fourth parameter `type` represents the type of threshold ing.

* cv2.THRESH_BINARY: the part greater than threshold is set as maxval, otherwise 0.

* cv2.THRESH_BINARY_INV: the part greater than threshold is set as 0, and the rest is maxval.

* cv2.THRESH_TRUNC: the part greater than threshold is set as threshold, and the part smaller than threshold remains unchanged.

* cv2.THRESH_TOZERO: the part greater than threshold is set as threshold, and the part smaller than threshold is set as 0.

* cv2.THRESH_TOZERO_INV: the part greater than threshold is set as 0, and the part smaller than threshold remains unchanged.

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image11.png" />

**Display image**: Call imshow function to display image and the parameter in bracket refers to the title of the window and displayed image.

**Close window:** waitKey function will wait until the keyboard is pressed, and then execute destroyAllWindows function to close the window.

* **Adaptive Thresholding**

Adaptive threshold ing is the method where the threshold value is calculated for smaller regions to process the image.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "adaptiveThreshold_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.12 Image Processing --- Thresholding-\>Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_16/media/image12.png" />, and then <img src="../_static/media/chapter_5/section_16/media/image14.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 adaptiveThreshold_demo.py**" and press Enter to run the routine.

```bash
python3 adaptiveThreshold_demo.py
```

(2) Program Outcome

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img class="common_img" src="../_static/media/chapter_5/section_16/media/image17.jpeg"/> | <img class="common_img" src="../_static/media/chapter_5/section_16/media/image18.jpeg"/> |

The final output image is as above.

(3) Code Analysis

For a picture with balanced color, its threshold is usually set as 127.

However, when the color of the image is out of balance, setting the threshold as 127 will make the output image bad. Therefore we need to turn to other threshold ing methods.

The routine "**adaptiveThreshold_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.12 Image Processing---Thresholding-\>Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image19.png" />

**Import Module**: Import cv2 module

**Read Image:** Call imread function to read picture and the parameter stands for the name of the picture

**Color Space Conversion:** call cvtColor function to convert the image into GRAY color space and the parameter indicates the picture and conversion mode.

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image20.png" />

**Thresholding:** use **adaptiveThreshold** function to execute. The specific format and parameters are as follow.

```py
adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C)
```

① The first parameter `src` refers to the pending image.

② The second parameter `maxValue` will be set only when the type is "cv2.THRESH_BINARY" or "cv2.THRESH_BINARY_INV" . It refers to the new value assigned when the gray values of the picture pixels are greater (smaller) than the threshold

③ The third parameter `adaptiveMethod` decides how the adaptive threshold value is calculated. And the specific methods are as follow.

* cv2.ADAPTIVE_THRESH_MEAN_C: set the weight value of all pixels in the neighborhood as the same.

* cv2.ADAPTIVE_THRESH_GAUSSIAN_C: Obtain the weight value of all the pixels in the neighborhood through Gaussian formula. The weight value is associated with the distance between pixels in each neighbor and target pixel. The shorter the distance, the greater the weight value. The longer the distance, the smaller the weight value.

④ The fourth parameter `thresholdType` indicates the type of the

threshold ing, which is combined with **maxValue** to use. This parameter only can be set as `cv2.THRESH_BINARY` or `cv2.THRESH_BINARY_INV`.

⑤ The fifth parameter `blockSize` represents the size of the neighbour area. It is generally set as 3, 5, 7, ect.

⑥ The sixth parameter "**C**" is a constant. The threshold is mean or weight value minus this constant.

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image21.png" />

**Display image:** Call imshow function to display image and the parameter in bracket refers to the title of the window and displayed image.

**Close window:** waitKey function will wait until the keyboard is pressed, and then execute destroyAllWindows function to close the window.

* **Otsu Thresholding**

Appropriate threshold will be automatically calculated.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "Otsu_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.12 Image Processing --- Thresholding-\>Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_16/media/image22.png" />, and then <img class="common_img" src="../_static/media/chapter_5/section_16/media/image24.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 Otsu_demo.py**" and press Enter to run the routine.

```bash
python3 Otsu_demo.py
```

(2) Program Outcome

<img src="../_static/media/chapter_5/section_16/media/image27.jpeg"/>

<img src="../_static/media/chapter_5/section_16/media/image28.png"/>

The final output picture is as above.

(3) Code Analysis

The Otsu threshold ing, also known as the maximum inter-class variance method, is a method where the inter-class variance is calculated by assigning pixels into two or more classes. When the variance reaches the maximum value, the class dividing line i.e. the gray value is used as the image segmentation threshold.

The routine "**Otsu_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.12 Image Processing---Thresholding-\>Routine Code](https://drive.google.com/drive/folders/15R1Tm3S4D0VMIm59xVef1YZMumzpkdvn?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_16/media/image29.png" />

Otsu threshold ing is to pass a parameter "**cv2.THRESH_OTSU**" in the parameter type of **threshold** function, so as to realize Otsu threshold segmentation.

In `cv2.threshold(img, 0, 255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)`，parameter thresh needs to set as 0, and parameter type should be set as

`cv2.THRESH_BINARY+cv2.THRESH_OTSU`

### 5.2.13 Image Processing---Contour Introduction and Feature

* **Contour Introduction**

Contour is defined as the line joining all the points along the boundary of an image that are having the same color or intensity. Contour is useful tool for shape analyzing as well as object detection and recognition.

For higher accuracy, binaryzation will be performed first. After the binary image is obtained, search the contour that is find the white object under the black background. Therefore, our target is the white object and the background is black.

* **Search and Draw Contour**

After the object is found, search for the contour points and draw the contour.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "contours_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_17/media/image2.png" />, and then <img src="../_static/media/chapter_5/section_17/media/image4.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 contours_demo.py**" and press Enter to run the routine.

```bash
python3 contours_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image7.jpeg" />

The final output picture is as above.

(3) Code Analysis

The routine "**contours_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image8.png" />

**Import Module**：Import cv2 module

**Read Picture**：Call imread function to read picture and the parameter stands for the name of the picture.

**Color Space Conversion:** call cvtColor function to convert the image into GRAY color space, and the parameter indicates the picture and conversion mode.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image9.png" />

**Thresholding:** use threshold function to execute. The specific format and parameters are as follow.

`threshold(src, thresh, maxval, type)`

① The first parameter `src` is the pending image.

② The second parameter `thresh` is the set threshold.

③ The third parameter `maxval` will be set only when the type is

`THRESH_BINARY` or `THRESH_BINARY_INV` . It refers to the new value assigned when the gray values of the picture pixels are greater (smaller) than the threshold

④ The fourth parameter `type` represents the type of threshold ing. cv2.THRESH_BINARY indicates the part greater than threshold is set as maxval, otherwise 0

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image10.png" />

**Search for contour: findContours** function is employed to search the contour, and the specific format and parameters are as follow.

```py
findContours(img,mode,method)
```

* The first parameter `img` represents the pending picture.

* The second parameter `mode` represents the detection mode of the contour.

RETR_EXTERNAL = 0: only the outermost contour will be detected

RETR_LIST = 1: detect all the contours without building hierarchy and all the contours will be put into one list

RETR_CCOMP = 2，detect all the contour and divide them into two layers

RETR_TREE = 3 ，Detect layer by layer from right to left according to the tree-shaped storage contour

* The third parameter `method` stands for the method used to search the contour. The specific method is as follow.

CHAIN_APPROX_NONE, Preserve all the dots on the contour

CHAIN_APPROX_SIMPLE: Compress the horizontal, vertical and oblique part that is only keep their corner coordinate. For example, 4 dots are enough to save the information about the rectangle contour.

> [!Note]
> 
> **In OpenCV4.2 or above, this function will only return two values, including "contours" and "hierarchy". And "binary" will not be returned.**
> 

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image11.png" />

**Contour drawing**: adopt drawContours function to draw the contour, and its specific format and parameters are as follow.

```py
wContours(image, contours, contourIdx, color, thickness)
```

① The first parameter `image` represents the image whose contour will be drawn.

② The second parameter `contours` represents the coordinate of all contours are found.

③ The third parameter `contoursIdx` indicates the serial number of the contour drawing. -1 means that all the contours will be drawn.

④ The fourth parameter `color` refers to the color of the contour.

⑤he fifth parameter `thickness` represents the width of the contour. -1 means that the contour will be padded.

* **Contour Feature Moment**

Feature moment is global feature of a contour and a picture. The moment contains the geometric features in different types of the corresponding objects. There are three types of moments, including spatial moment, central moment and normalized central moment

* Spatial moment: it is also called geometric moment about the area and perimeter of the image, including zero-order moment: m00, first-order moment: m10, m01, second-order moment: m20, m11, m02, third-order moment: m30, m21, m12, m03

*  entral moment: For the higher--order image, the moment will vary with the position, which can be fixed by central moment. The invariance of translation can be obtained by subtracting the mean value, so we can compare whether two objects at different positions are consistent, that is, the central moment features translation invariance.

It includes two-order central moments: mu20, mu11 and mu02, as well as three-order central moment: mu30, mu21, mu12 and mu03.

* Normalized central moment: Apart from translation, some pictures will be scaled. Even though the image is scaled, its features can be detected.

Normalized central moment obtain the scale invariance by dividing by the dimension of the object.

It includes two-order Hu moment: nu20, nu11 and nu02, and three-order Hu moment: nu30, nu21, nu12 and nu03. Following, based on the obtained contour, calculate the feature moment, area and perimeter of the contour.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "moments_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_17/media/image12.png" />, and then <img src="../_static/media/chapter_5/section_17/media/image14.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 moments_demo.py**" and press Enter to run the routine.

```bash
python3 moments_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image17.jpeg" />

The unit of the area and perimeter is pixel. The outermost contour of the image will be calculated

(3) Code Analysis

The routine "**moments_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image18.png" />

**Take out the outermost contour:** take the first contour with index 0 in the contour list

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image19.png" />

**Feature moment calculation**: use **moments** function to calculate the feature moment, and the function format and parameters are as follow.

```py
moments(array,binaryImage)
```

① The first parameter `array` is the contour point.

② The second parameter `binaryImage` is set as False by default. If it is True, all non-zero pixels will be treated as 1, which is equivalent to image binaryzation.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image20.png" />

Area calculation: adopt `countourArea` function to calculate the area, and its format and parameters are as follow.

**countourArea(contour):** "**contour**" is a contour in the contour list.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image21.png" />

Perimeter calculation: employ **arcLength** function to calculate the perimeter, and its specific format and parameter is as follow.

**arcLength(curve,closed)**

* The first parameter `curve` represents the contour.

* The second parameter `closed` decide whether the contour is closed or not. If it is closed, set is as **True**, otherwise False.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image22.png" />

* **Polygon Approximation**

The searched `contours` maybe too complex and not smooth,

`approxPolyDP` function can be adopted to appropriately approximate the polygon curve, which is polygon approximation.

This function uses polygons to approximate the contour, utilizing the

Douglas-Peucker algorithm (DP). The principle of the DP algorithm is simple. Its core is to continuously find the farthest point of the polygon to form a new polygon until the shortest distance is less than the specified accuracy.

Next, analyze the object contour with polygon approximation.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "approx_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
>

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_17/media/image23.png" />, and then <img src="../_static/media/chapter_5/section_17/media/image25.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 approx_demo.py**" and press Enter to run the routine.

```bash
python3 approx_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image28.png" />

The contour after polygon approximation will try to fit the figure as much as possible.

(3) Code Analysis

The routine "**approx_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing---Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image29.png" />

**Take the contour:** take the second contour with index 1 in the contour list

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image30.png" />

**Polygon approximation:** apporxPolyDP function is adopted. The specific format is as follow.

```py
apporxPolyDP(curve, epsilon, closed)
```

1\) The first parameter `curve` is the contour to be searched.

2\) The second parameter `epsilon` indicates accuracy. The smaller the number, the lower the accuracy and the more consistent with the

pattern contour.

3\) The third parameter `closed` decide whether the contour is closed or not. If it is closed, set is as **True**, otherwise False.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image31.png" />

* **Contour Convex Hull**

Convex Hull will look similar to contour approximation, but it is the convex polygon in the outermost of the object. Convex hull refers to a polygon that

completely contains the original contour and consists only of points on the

contour. Every part of the convex hull is convex, that is, the line connecting any two points in the convex hull is inside the convex hull. In the convex hull, the interior angle of any three consecutive points is less than 180.

Next, analyze the object contour through contour convex hull.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "hull_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_17/media/image32.png" />, and then <img src="../_static/media/chapter_5/section_17/media/image34.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 hull_demo.py**" and press Enter to run the routine.

```bash
python3 hull_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image37.png" />

The convex hull will connect the vertices of the contour.

(3) Code Analysis

The routine "**hull_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing---Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)**"

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image38.png" />

**Convex hull:** use **convexHull** function to draw the convex hull. The function format and parameters are as follow.

```py
convexHull(points, clockwise,)
```

① The first parameter `points` is the contour to be searched.

② The second parameter `clockwise` refers to the drawing direction. When it is True, the convex hull will be draw clockwise. When it is False, the convex hull will be drawn counterclockwise.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image39.png" />

* **Circumscribed Rectangle**

The bounding rectangle is divided into the minimum bounding rectangle with rotation angle and the regular circumscribed rectangle.

**Minimum bounding rectangle:** it is drawn with minimum area, so it considers the rotation.

**Regular bounding rectangle:** frame the object with the rectangle.

Next, draw the regular bounding rectangle and minimum bounding rectangle.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "rect_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing --- Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_17/media/image40.png" />, and then <img src="../_static/media/chapter_5/section_17/media/image42.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 rect_demo.py**" and press Enter to run the routine.

```bash
python3 rect_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image45.png" />

The green one is the regular bounding rectangle, and the blue one is the minimum bounding rectangle.

(3) Code Analysis

The routine "**rect_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.13 Image Processing---Contour Introduction and Feature-\>Routine Code](https://drive.google.com/drive/folders/1RQ83IHUOkKzb9RbGf57BVaX2x9K84ahh?usp=sharing)**"

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image46.png" />

**Acquire minimum bounding rectangle:** adopt `minAreaRect` function to get the minimum bounding rectangle. And the function format and parameters are as follow.

**minAreaRect( points ): "points"** refers to contour, and the returned value

contains the starting coordinate, width, height and angle.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image47.png" />

**Obtain the regular bounding rectangle:** use `boundingRect` function to realize. The function format and parameters are as follow.

`boundingRect（array)`: `array` refers to contour and the returned value `Rect` contains the starting coordinate, width and height.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image48.png" />

**Get the vertex coordinate of the minimum bounding rectangle:**

`boxPoints` function will be employed. The function format and parameters are as follow.

`boxPoints(rect)`: refers to the rectangle whose vertex coordinate needs to be obtained. Its data type belongs to floating point number.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image49.png" />

`Number rounding`: use **int0** function to execute. The function format and parameters are as follow.

`int0(date)`: "date" is the data to be rounded.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image50.png" />

`Draw rectangle`: rectangle function will be adopted to draw the rectangle.

The function format and parameters are as follow.

```py
rectangle(src,pt1,pt2,color,thickness)
```

① The first parameter `src` refers to the image to draw the contour.

② The second parameter `pt1` represents one of the vertices of the rectangle.

③ The third parameter `pt2` refers to the diagonal vertices of pt1

④ The fourth parameter `color` represents the color of the rectangle.

⑤ The fifth parameter `thickness` represents the width of the drawn rectangle. "-1" indicates padding rectangle.

<img class="common_img" src="../_static/media/chapter_5/section_17/media/image51.png" />

### 5.2.14 Image Processing---Feature Matching

* **Brute-Force Matching**

The feature descriptor is to describe the key point with a set of vectors

after the key point is calculated. It includes not only the key point, but also the pixels around the key point that has made a contribution. It serves as the basis for target matching, and enables the key points to have more invariant

characteristics, such as illumination changes, 3D viewpoint changes, etc.

Each feature descriptor in one set of features is matched with the nearest feature descriptor of the other set, then the obtained distances are sorted, and finally the feature with the shortest distance is selected as the matching point for the two.

Next, use brute force matching to match the features of the two images.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "bf_demo.py" and sample picture "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.14 Image Processing --- Feature Matching-\>Routine Code](https://drive.google.com/drive/folders/1O6lCcndS21gZvN6T8qsxtjONa8ErKjtv?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_18/media/image2.png"/>, and then <img src="../_static/media/chapter_5/section_18/media/image4.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 bf_demo.py**" and press Enter to run the routine.

```bash
python3 bf_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image7.png" />

Take out one part of the original image, and then match features between these two images.

(3) Code Analysis

The routine "**bf_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.14 Image Processing --- Feature Matching-\>Routine Code](https://drive.google.com/drive/folders/1O6lCcndS21gZvN6T8qsxtjONa8ErKjtv?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image8.jpeg" />

**Import Module**：Import cv2 module

**Read Picture**：Call imread function to read picture and the parameter stands for the name of the picture.

**Initialized detector:** use ORB_create constructor for initialization.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image9.png" />

**Detect feature and descriptor**: **detectAndCompute** function will be used to detect. And the specific format and parameter are as follow.

detectAndCompute(src,mask)

① The first parameter `src` is the image to process the threshold.

② The second parameter `mask` is the image in which the object is black and the rest is white. If the mask is not required, set the parameter as None.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image10.png" />

**Create BFMatcher object:** BFMatcher belongs to features2d module and inherits from DescriptorMatcher. The function format is as follow.

**static Ptr\<BFMatcher\> create( int normType , bool crossCheck )**

① The first parameter `normType` can be set as NORM_L1, NORM_L2, NORM_HAMMING or NORM_HAMMING2. The HOG descriptors of SIFT and SURF correspond to the Euclidean distances L1 and L2; the BRIEF descriptors of ORB and BRISK correspond to the Hamming distance HAMMING; HAMMING2 corresponds to the ORB algorithm when WTA_K = 3 or 4.

② Euclidean distance: it is defined as the distance between two points in n-dimensional space.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image11.jpeg" />

Hamming distance: It is computer's XOR operation suitable for binary string descriptors, such as BRIEF descriptors. Its definition is as follow.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image12.jpeg" />

③ The second parameter "**crossCheck**" is set as "**FALSE**" by default. If set as TRUE, the matching is valid only when the features in the two groups match with each other. In other words, only when the x point descriptor in

group A and the y point in group B are the best matching points for each other, the matching is effective.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image13.png" />

**Match descriptor: detectAndCompute** function will be adopted, and its specific format and parameters are as follow.

**match(queryDescriptors,trainDescriptors)**

① The first parameter `queryDescriptors` is the image feature vector to be matched.

② The second parameter `trainDescriptors` is the image feature vector that needs to be matched.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image14.png" />

**Draw matches: drawMatches** function will used. The specific format and parameters are as follow.

`drawMatches(src1,kp1,src2,kp2,match,matchesMask,flags)`

① The first parameter `src1` is the matching image 1.

② The second parameter `kp1` is the feature of image 1.

③ The third parameter `src2` is the matching image 2.

④ The fourth parameter `kp2` is the feature of the image 2.

⑤ The fifth parameter `match` is the set of matching points to be drawn.

⑥ The sixth parameter `matchesMask` determines which images to draw. If it is set as None, all the images will be drawn.

⑦ The seventh parameter `flags` represents the drawing flag. 0 indicates that all the features will be drawn, and 2 indicates that only the matched

feature will be drawn. 4 stands for the drawing styles.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image15.png" />

**Display image**：Call imshow function to display image and the parameter in bracket refers to the title of the window and displayed image.

**Close window**: waitKey function will wait until the keyboard is pressed, and then execute destroyAllWindows function to close the window.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image16.png" />

* **Nearest Neighbor Matching**

FLANN (Fast Library for Approximate Nearest Neighbors) is a FLANN is a open-source library for performing fast approximate nearest neighbor

searches in high dimensional spaces.

The nearest neighbor matching operator FlannBased Matcher based on the FLANN library is much more efficient than BFMatcher in the field of large feature datasets or some real-time processing.

Next, adopt nearest neighbor matching to match the features of the two images.

(1) Operation Steps

> [!Note]
> 
> * **Before operation, please copy the routine "flann_demo.py" and sample pictures "test.jpg" and "test1.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.14 Image Processing --- Feature Matching-\>Routine Code](https://drive.google.com/drive/folders/1O6lCcndS21gZvN6T8qsxtjONa8ErKjtv?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
> 

① Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_18/media/image17.png" />, and then <img src="../_static/media/chapter_5/section_18/media/image19.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

② Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

③ Input command "**python3 flann_demo.py**" and press Enter to run the routine.

```bash
python3 flann_demo.py
```

(2) Program Outcome

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image22.png" />

(3) Code Analysis

The routine "**flann_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.14 Image Processing---Feature Matching-\>Routine Code](https://drive.google.com/drive/folders/1O6lCcndS21gZvN6T8qsxtjONa8ErKjtv?usp=sharing)**" .

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image23.jpeg" />

**Set FLANN parameter**: FlannBased Matcher function will be adopted. And its format and parameters are as follow.

FlannBased Matcher（IndexParams,SearchParams）(The two parameters in the bracket refers to the type of the dictionary)

① The first parameter `IndexParams` the algorithm designated to use.

② The second parameter `SearchParams` is the number of times the tree in the specified index should be traversed recursively. Higher values

provide better accuracy, but also take more time.

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image24.jpeg" />

Nearest neighbor matching: knnMathch function will be used, And the function format and parameters are as follow.

`knnMathch(queryDescriptors,trainDescriptors,k)`

The first two parameters are consistent with match, and "k" is the best number of matches to be returned.)

<img class="common_img" src="../_static/media/chapter_5/section_18/media/image25.png"/>

### 5.2.15 Image Processing---Corner Detection

* **Corner Introduction**

(1) Corner Definition

The corner is defined as the intersection of two edges, or a feature with two main directions in the neighborhood. In general, corner is the point on the edge curve with maximum curvature, or the point with large variation in

intensity in the image.

(2) Detection Idea

Define a tiny local window in the image, then move this window in all directions, which will leads to three results, including flat areas, edges and corners.

① If the image intensity of window doesn't change as the window is moving in all directions, inside the window are all flat areas, and corner isn't involved.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image2.png" />

② If the image intensity of window change greatly as the window is moving in one (some) direction(s), but keeps still in other directions, there maybe "**edges**" inside the window.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image3.png" />

③ f the image intensity of window change greatly in all direction, there maybe "corners" inside the window.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image4.png" />

(3) Harris Corner Detection Formula

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image5.jpeg" />

And：

① det(M)= λ1 λ2。

② trace(M)= λ1+ λ2。

③ λ1 and λ2 are the feature values of the moment M. 

The region type can be judged based on these features:

When \|R\| is small, which happens when λ1 and λ2 are small, the region is flat.

When R\<0, which happens when λ1\>\>λ2 or vice versa, the region is edge.

When R is large, which happens when λ1 and λ2 are large and λ1∼ λ2, the region is a corner.

* **Operation Steps**

Next, detect the corners of the image through Harris corner detection.

> [!Note]
>
> * **Before operation, please copy the routine "corners_demo.py" and sample pictures "test.jpg" in "[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.15 Image Processing --- Corner Detection-\>Routine Code](https://drive.google.com/drive/folders/1NwJC42HS2v2_ZeMiYduB32P1bmpq8xb5?usp=sharing)" to the shared folder.**
> * **The input command should be case sensitive and the keywords can be complemented by "Tab" key.**
>

(1) Open virtual machine and start the system. Click <img src="../_static/media/chapter_5/section_19/media/image6.png" />, and then <img class="common_img" src="../_static/media/chapter_5/section_19/media/image8.png" /> or press "**Ctrl+Alt+T**" to open command line terminal.

(2) Input command "**cd /mnt/hgfs/share/**" and press Enter to enter the shared folder.

```bash
cd /mnt/hgfs/share/
```

(3) Input command "**python3 bf_demo.py**" and press Enter to run the routine.

```bash
python3 bf_demo.py
```

* **Program Outcome**

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image11.png" />

* **Code Analysis**

The routine "**corners_demo.py**" can be found in "**[5. OpenCV Computer Vision Course->5.2 Basic Course->5.2.15 Image Processing --- Corner Detection-\>Routine Code](https://drive.google.com/drive/folders/1NwJC42HS2v2_ZeMiYduB32P1bmpq8xb5?usp=sharing)**"

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image12.jpeg" />

(1) Import Module: Import cv2 and nmupy module.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image13.png" />

(2) Create corner detection function：`harris(image)：`the parameter in the bracket is the image that needs to process

(3) color space conversion：`cvtColor(src,mode)`

The first parameter `src` refers to the image to convert.

The second parameter `mode` is the conversion mode of the color space.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image14.png" />

(4) Corner Detection：`cornerHarris(src,blockSize,apertureSize,k)` The first parameter `src` is the image for detection.

The second parameter `blockSize` is the size of the domain pixel. The third parameter `apertureSize` is the size of the used window. The fourth parameter "**k**" is a free parameter ranging \[0.04 ，0.06\].

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image15.png" />

(5) Acquire the array of the same type：`empty(shape,dtype)`

The first parameter `shape` represents the shape of the returned array defined by the integer or a tuple of integers.

The second parameter "dtype" refers to the data type defining the type of the returned array.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image16.png" />

(6) Normalization: Normalization is to process the data and then limit within the required range. The size of the output image of Harris corner

detection after normalization is the same as the original image. Pixel value of each point corresponds to the probability that the point is corner. The greater the value, the more likely it is a corner.

Function format: `normalize(src,dst,alpha,beta,normType)`

The first parameter `src` is the input array.

The second parameter `dst` is the output array after processing. The third parameter `alpha` is the minimum value of normalization

The fourth parameter `beta` is the maximum value of normalization

The fifth parameter `normType` indicates the types of normalization as below.

① NORM_MINMAX: The value of the array is translated or scaled to a specified range. Linearly normalized is commonly used.

② NORM_INF: it is the C-norm of the normalized array (the maximum value of the absolute value)

③ NORM_L1: L1-norm of the normalized array (sum of absolute values)

④ NORM_L2: (Euclidean) L2-norm of the normalized array

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image17.png" />

(7) Circle the corner：use a loop to traverse the normalized image array and draw a circle in the corner area.

Function format：`circle(src,point,radius,color,thickness)` The first parameter `src` is the image to be drawn.

The second parameter `point` is the center of the drawn circle The third parameter `radius` refers to the radius of the circle

The fourth parameter `color` is the set color

The fifth parameter `thickness` refers to the line thickness of the circle. When it is negative number, it is solid circle.

<img class="common_img" src="../_static/media/chapter_5/section_19/media/image18.png" />

(8) Read image, process image, display image and close the window:

Read image: Use `imread(image)` function to read the image, and the parameter in the bracket is the name of the picture

Process image: call `harris(image)` function to process the image and the image parameters will be passed in.

Display image: employ `imshow(title,src)` function to display the output image. The parameters in the bracket are the title of window and the displayed image.

Close window: Adopt waitKey function to wait for the key to be pressed, and then call `destroyAllWindows` function to close the display window.