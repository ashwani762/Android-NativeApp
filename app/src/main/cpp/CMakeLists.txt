cmake_minimum_required(VERSION 3.6.0)

add_library(native_app_glue STATIC
        ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

set(CMAKE_SHARED_LINKER_FLAGS
        "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall")
add_definitions("-DGLM_FORCE_SIZE_T_LENGTH -DGLM_FORCE_RADIANS")

add_subdirectory(glm)

add_library(helloworld-c SHARED
        helloworld-c.cpp)

target_include_directories(helloworld-c PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${ANDROID_NDK}/sources/android/native_app_glue)

target_link_libraries(helloworld-c
        android
        native_app_glue
        atomic
        EGL
        GLESv2
        glm
        log
        OpenSLES)