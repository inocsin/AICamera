cmake_minimum_required(VERSION 3.4.1)

add_library(
             native-lib
             SHARED
             src/main/cpp/native-lib.cpp
             )
find_library(
          android-lib
          android
          )

include(AndroidNdkModules)
android_ndk_import_module_cpufeatures()

add_library(
    caffe2
    STATIC
    IMPORTED
    )
set_target_properties(
    caffe2
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/armeabi-v7a/libCAFFE2_CPU.a
    )

add_library(
    glog
    SHARED
    IMPORTED
    )
set_target_properties(
    glog
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/${ANDROID_ABI}/libglog.so
    )

add_library(
    protobuf
    SHARED
    IMPORTED
    )
set_target_properties(
    protobuf
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/${ANDROID_ABI}/libprotobuf.a
    )

add_library(
    NNPACK
    STATIC
    IMPORTED
    )

set_target_properties(
    NNPACK
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/${ANDROID_ABI}/libnnpack_log.a
    )

add_library(
    nnpack_ukernels
    STATIC
    IMPORTED
    )

set_target_properties(
    nnpack_ukernels
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/${ANDROID_ABI}/libnnpack_ukernels.a
    )

add_library(
    pthread
    STATIC
    IMPORTED
    )

set_target_properties(
    pthread
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/${ANDROID_ABI}/libpthreadpool.a
    )

add_library(
    cpufeature
    STATIC
    IMPORTED
    )

set_target_properties(
    cpufeature
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_LIST_DIR}/src/main/jniLibs/${ANDROID_ABI}/libcpufeatures.a
    )

include_directories( src/main/cpp )

find_library(
     log-lib
     log
     )

target_link_libraries(
                       native-lib
                       -Wl,--whole-archive
                       caffe2
                       -Wl,--no-whole-archive
                       glog
                       protobuf
                       NNPACK
                       cpufeatures
                       cpufeature
                       nnpack_ukernels
                       pthread
                       ${log-lib}
                       ${android-lib})
