# Cam2Ndk
For Machine learning or images processing (`OpenCV`) applications on Android, it's often divided into two planes: part of it in JAVA and other part in `c++`(using NDK). There are various **Good** reasons that such applications should be implemented in native code(`c++` planes alone). With NDK Camera 2 APIs released as part of Android 7(`API version 24`) it has become a blessing for developers and delight for C/C++ lovers.

Example: image processing can be done much efficiently using OpenCV compared to what is available at JAVA plane ( getPixel()/setPixel(), Glide Image Transformation, ColorMatrix, Renderscript )

There are two parts to this repo:
1. **ndk_so:** Its a standalone `c++` code for camera control and there is no need to use Android Studio (c++ folks would surely love it), just build it from the command line. For this you need to download the version of NDK (cross compilation toolchain for android devices) you need (anything above `API version 24`) and set the appropriate path in the `Makefile`. Or if you already have NDK installed using Android Studio? It must be somewhere here -> `~/Library/Android/sdk/ndk/21.2.6472646` (on my macbook). 

Once you build the code it will generate `lib/libndksamplecam.so`. Drop this shared object to `jnilibs` folder, in ndk-cam project. Currently the `Makefile` is quite rudimentary and build for ABI type `arm64-v8a`, similarly more targets can be added. 

2. **ndk-cam:** This is merely a minimalistic boiler plate code to create `textureView` and render onto the screen. 


# Where do we capture Image? 
There is a callback function `imageCallback` inside the object `NDKCamera`, it gives you captured images for further processing, this image can further be converted to OpenCV `Mat` object or any other object that your frameworks requires. There is no Java -- C++ boundary to jump to and project delivery/shipping just by delivering shared object `.so` and `JAR` is not really required. 


# NDK Installation: 
To know what is NDK and how to install it, please follow the [article](https://medium.com/@tomdeore/opencv-on-android-tiny-with-optimization-enabled-932460acfe38), for us **Step-1** is only required. 

# OpenCV Integration:
I have integrated OpenCV version 3.4.3, What you need to do is to fetch the OpenCV Android SDK and set the appropriate path for Andoird header files (using `-I`) and staticlibs (using `-L` and `-l`) in the `Makefile`. In case you need strip down version of OpenCV and not all the modules of OpenCV check out my [blog](https://mdeore.medium.com/opencv-on-android-tiny-with-optimization-enabled-932460acfe38). Mind you, you cannot just pick static `.a` libray from OpenCV, check this [thread](https://github.com/opencv/opencv/issues/14825)

# TFLite (.so) for Android:
Check this [tensorflow documentation](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/g3doc/guide/android.md) for details as to how to build standalone `.so` for C++ project. Often TF documentation is broken due to changes, the command to create `libtensorflowlite.so` is as:

32bit armeabi-v7a:
```
bazel build -c opt --config=monolithic --config=android_arm //tensorflow/lite:libtensorflowlite.so
```
64bit arm64-v8a:
```
bazel build -c opt --config=monolithic --config=android_arm64 //tensorflow/lite:libtensorflowlite.so
```
Currently, there is no straightforward way to extract all header files needed, so you must include all header files in tensorflow/lite/ from the TensorFlow repository. Additionally, you will need header files from FlatBuffers and Abseil.

### NOTE: `--config=monolithic` is important to mention while building the shared object, else all the dependencies won't get bundled together. 
