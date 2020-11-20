# Cam2Ndk
For Machine learning or images processing (`OpenCV`) applications on Android, is often divided into two planes part Java and part NDK(`c++`). There are various reasons that such application should be implemented in native code(`c++`) is better. With NDK Camera 2 APIs released as part of Android 7(`API version 24`), its quite comfortable handling camera completely in the native code and hence the integration with OpenCV and likes would be quite fast and easy. 

There are two parts to this repo:
1. **ndk_so:** This is standalone `c++` code for camera control and there is no need to use Android Studio (c++ folks would surly love this), you can build it from command line. You just need to download the version of NDK (cross compilation toolchain for android devices) you need (anything above `API version 24`) and set the appropriate path in the `Makefile`. Or if you already have NDK installed using Android Studio? It must be somewhere here -> `~/Library/Android/sdk/ndk/21.2.6472646` (on my mackbook). 

Once you build the code it will generate `lib/libndksamplecam.so`. Push this shared object to `jnilibs` folder, in ndk-cam project. Currently the `Makefile` is quite rudementary and build for ABI type `arm64-v8a`, similarly more targets can be added. 

2. **ndk-cam:** This is mere a minimilistic boiler plate code to create `textureView` and render onto the screen. 


# Where do we capture Image? 
There is a callback function `imageCallback` inside object `NDKCamera`, it gives you captured image for further processing, this image can further be converted to OpenCV `Mat` object or any other object that your frameworks requires. There is not Java -- C++ boundary to jump and if you plane and projects can be shipped just by delivering shared object `.so` and `JAR` is not really required. 
