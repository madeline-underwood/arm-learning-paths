---
title: Create a simple program
weight: 6

### FIXED, DO NOT MODIFY
layout: learningpathall
---

## Create and build a simple program

You'll now build a simple program that runs inference on all three submodules directly on an Android device. 

The program takes a text prompt as input and generates an audio file as output.
```bash
cd $WORKSPACE/audio-stale-open-litert/app
mkdir build && cd build
```

Build the FlatBuffers tools (used by TensorFlow Lite):
```console
mkdir flatc-native-build && cd flatc-native-build
cmake ../tensorflow/lite/tools/cmake/native_tools/flatbuffers
cmake --build .
```

Ensure the NDK path is set correctly and build with cmake:
```bash
cmake -B build -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake \
	       -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-26 \
 	       -DTF_INCLUDE_PATH=$WORKSPACE/tensorflow_src \
 	       -DTF_LIB_PATH=$WORKSPACE/tensorflow_src/audio-gen-build/tensorflow-lite/ \
 	       -DFLATBUFFER_INCLUDE_PATH=$WORKSPACE/tensorflow_src/flatc-native-build/flatbuffers/include

cmake --build build
```

After the SAO example builds successfully, a binary file named `audiogen_main` is created. 

Now use adb (Android Debug Bridge) to push the necessary files to the device:

```bash
adb shell
```

Create a directory for all the required resources:
```bash
cd /data/local/tmp
mkdir audiogen
```
Push all necessary files into the `audiogen` folder on Android:
```bash
cd sao_litert
adb push runner/build/audiogen_main /data/local/tmp/audiogen
adb push dit.tflite /data/local/tmp/audiogen
adb push autoencoder_model.tflite /data/local/tmp/audiogen
adb push conditioners_tflite/conditioners_float32.tflite /data/local/tmp/audiogen
```

Finally, run the program on your Android device:

```bash
adb shell
```

