# CAMERAL-NETWORK
THREE STEPS TO CREATE A CAMERAL NETWORK.

> Currently, the camera has only initially completed capturing images at 30fps and outputting yuyv format. Then the format is converted, yuyv is converted to 264. Finally, it is transmitted to the target host using rtp.

Project introduction (development platform ubuntu 18)

### 1. The libraries used are v4l2, x264, jrtplib

#### 1.1 v4l2

* ubuntu comes with it, so you don’t need to install it.

#### 1.2 x264

* `git clone http://git.videolan.org/git/x264.git`
* `./configure --enable-shared`
* `make` If something goes wrong, install some required libraries first. Just run the following command.
```
sudo apt-get install build-essential git-core checkinstall texi2html libfaac-dev \
libopencore-amrnb-dev libopencore-amrwb-dev libsdl1.2-dev libtheora-dev \
libvorbis-dev libx11-dev libxfixes-dev zlib1g-dev
```
* `make install`

#### 1.3 jrtplib

* `git clone https://github.com/j0r1/JRTPLIB.git`
* `cmake ./`
* `make` 
* `make install`

### 2. Catalog Introduction

* include: header file directory
    * `config.h` configuration file
    * `ddebug.h` debug print output

* res: res: contains some resources

* test:Test directory, which contains the tests of the entire process. Configure the test flow through test_min_level and test_max_level. You can see the code in ./test/test_main.cpp for details.
    * Camera capture test
    * yuyv converted to yuv420p test
    * yuv420p converted to 264 test
    * 264 transmission test

* *.cpp file: source code

### 3. Compile and run

* If you want to test in segments, comment out the run() of the main function in main.cpp. On the contrary, if it is running, uncomment it.
* `make`
* Run (the following three steps are in no particular order)
    * Connect to the local camera
    * `./main`
    * Open the software of the user terminal that receives 264 streams, such as vlc.

### 4. Local testing process

* Comment out the run() function in main.cpp
* `make`
* `./main x,y` x is the start item of the test, y is the end item of the test
* After that, the corresponding video file (100 frames) will be generated under the res directory, and the following is the playback command

```
ffplay -f rawvideo -pixel_format yuyv422 -video_size 640x480 ./res/image.yuyv
ffplay -f rawvideo -pixel_format yuv420p -video_size 640x480 ./res/image.yuv
ffplay -stats -f h264 ./res/image.264
```


## TODO: Three steps can create a camera to the user terminal display, suitable for scenarios such as: car camera

### 1.  Camera configuration

* Function name: `camera_struct camera(camera_struct *cam);`

* Set parameters, such as frequency, captured image format, etc.

### 2. Format conversion

* Function name: `format_struct convert_format(format_struct *for);`

* Set the conversion target format, such as 264, jpeg, m3u8, etc.。

### 3. Transmission

* `Function name: tran_struct transmit(tran_strcut *tran);`

* Select the transmission protocol, such as rtp, rtmp, flv, etc.




