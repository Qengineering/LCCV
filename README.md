# Libcamera API wrapper for OpenCV RPi Bullseye 64OS
![output image]( https://qengineering.eu/images/CameraWall.webp )<br/>
## Libcamera C++ API wrapper for OpenCV on a Raspberry Pi 4 with 64-bit Bullseye OS
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>
In the new Debian 11, Bullseye, you have libcamera as the default camera stack. This C++ code is a example on how to conect the libcamera API in OpenCV. It requires less CPU-resources compared to GStreamer.<br/>

| Size | FPS | Gstreamer | LCCV |
| ---- | :-----:  | :-----:  | :-----:  |
| 640 X 480 | 15 | 24% | **10%** |
| 640 X 480 | 30 | 25% | **17%** |
| 1024 x 768 | 15 | 41% | **16%** |
| 1280 x 960 | 15 | 42% | **18%** |
| 2592 x 1944 | 15 | 50% | **48%** |
| 3280 x 2464 | 15 |  - | **51%** |

A time consuming process can be a full size preview on the screen, especially at high resolutions.

------------

## Dependencies.<br/>
To run the application, you have to:
- A Raspberry Pi 4. 
- Raspbian's libcamera-apps source code installed (```$ sudo apt install libcamera-dev```)
- OpenCV 64 bit installed. [Install OpenCV 4.5](https://qengineering.eu/install-opencv-4.5-on-raspberry-64-os.html) <br/>
- Code::Blocks installed. (```$ sudo apt-get install codeblocks```)
- A working Raspicam

------------

## Installing the app.
To extract and run the app in Code::Blocks <br/>
$ mkdir *MyDir* <br/>
$ cd *MyDir* <br/>
$ wget https://github.com/Qengineering/LCCV/archive/refs/heads/main.zip <br/>
$ unzip -j master.zip <br/>
Remove master.zip, LICENSE and README.md as they are no longer needed. <br/> 
$ rm main.zip <br/>
$ rm LICENSE <br/>
$ rm README.md <br/> <br/>
Your *MyDir* folder must now look like this: <br/> 
```
├── example
│   ├── takephoto.cpp
│   └── takevideo.cpp 
├── include
│   ├── lccv.hpp
│   ├── libcamera_app.hpp
│   └── libcamera_app_options.hpp
├── LVVC.cbp
└── src 
    ├── lccv.cpp
    ├── libcamera_app.cpp 
    └── libcamera_app_options.cpp 
```

------------

## Running the app.
To run the application load the project file LCCV.cbp in Code::Blocks.<br/> 

A remark. The main loop looks somewhat odd with its if-else statements, where a `break` is usually applied when a frame is missed.<br/>
```
    int ch=0;
    while(ch!=27){
        if(!cam.getVideoFrame(image,1000)){
            std::cout<<"Timeout error"<<std::endl;
        }
        else{
            cv::imshow("Video",image);
            ch=cv::waitKey(10);
        }
    }
```
It is necessary because frames are discarded during the first startup of the camera, which exceeds the timeout limit of 1 sec.<br/><br/>
![output image]( https://qengineering.eu/images/LCCVtime.png )<br/>

Many thanks to [kbarni](https://github.com/kbarni) for this hard work on the original repo!<br/><br/>

------------

## Extract of [_kbarni_](https://github.com/kbarni) original readme.

LibCamera bindings for OpenCV

LCCV (*LibCamera bindings for OpenCV*) is a small wrapper library that provides access to the Raspberry Pi camera in OpenCV.

IMPORTANT WARNING: 

*This is a very early version, so expect to have several bugs.*<br/>
*Note that the API still might have some changes!*<br/>
*Please help with the development by reporting the bugs and issues you encounter, committing bugfixes, and proposing ideas!*<br/>

### Context

-------

In Raspbian Bullseye, the Raspberry Pi camera framework was completely rebased from MMAL to the LibCamera library - thus breaking most of the previous camera dependencies.

Raspbian comes with the handy `libcamera-apps` package that duplicates the old `raspistill` and `raspivid` applications, with some added functionnality, like the possibility of adding postprocessing routines to the capturing process.

However this is still limited, as it doesn't allow full integration of the camera in your software.

LCCV aims to provide a simple to use wrapper library that allows you to access the camera from a C++ program and capture images in cv::Mat format.

Features and limitations

------------------------

LCCV is heavily based on Raspbian's `libcamera-apps` source code. It is aimed to offer full control over the camera, so the original options class was kept instead of a new one based on OpenCV's VideoCapture class. Note that only the camera parameters are available, other parameters and functions, like previewing, cropping and post-processing were stripped from the library.
