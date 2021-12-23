LibCamera bindings for OpenCV

LCCV (*LibCamera bindings for OpenCV*) is a small wrapper library that provides access to the Raspberry Pi camera in OpenCV.

### IMPORTANT WARNING: 

***This is a very early version, so expect to have several bugs.***

***Note that the API still might have some changes!***

***Please help with the development by reporting the bugs and issues you encounter, committing bugfixes, and proposing ideas!***

Context

-------

In Raspbian Bullseye, the Raspberry Pi camera framework was completely rebased from MMAL to the LibCamera library - thus breaking most of the previous camera dependencies.

Raspbian comes with the handy `libcamera-apps` package that duplicates the old `raspistill` and `raspivid` applications, with some added functionnality, like the possibility of adding postprocessing routines to the capturing process.

However this is still limited, as it doesn't allow full integration of the camera in your software.

LCCV aims to provide a simple to use wrapper library that allows you to access the camera from a C++ program and capture images in cv::Mat format.

Features and limitations

------------------------

LCCV is heavily based on Raspbian's `libcamera-apps` source code. It is aimed to offer full control over the camera, so the original options class was kept instead of a new one based on OpenCV's VideoCapture class. Note that only the camera parameters are available, other parameters and functions, like previewing, cropping and post-processing were stripped from the library.

Prerequisites

-------------

- Raspbian Bullseye
- Development libraries (gcc/clang, cmake, git)
- libcamera (with development packages)
- OpenCV (with development packages)

Install everything using the following command:

    sudo apt install build-essential cmake git libcamera-dev 


License

-------

The source code is made available under the simplified [BSD 2-Clause license](https://spdx.org/licenses/BSD-2-Clause.html).
