cmake_minimum_required(VERSION 3.20)
project(RDRVRRegressionTests LANGUAGES CXX)

# Standalone: no game, OpenXR runtime, vcpkg packages or RedHook SDK needed.
# rig_snapshot uses Windows VirtualQuery and MSVC structured exceptions.
if(NOT MSVC OR NOT WIN32)
    message(FATAL_ERROR "These tests require Windows and the MSVC C++ toolchain")
endif()
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
enable_testing()
foreach(name rig_snapshot body_facing steering holster_record)
    add_executable(${name}_test ${name}_test.cpp)
    target_compile_features(${name}_test PRIVATE cxx_std_20)
    target_compile_definitions(${name}_test PRIVATE WIN32_LEAN_AND_MEAN NOMINMAX)
    target_compile_options(${name}_test PRIVATE /W4 /WX /permissive- /utf-8)
    target_include_directories(${name}_test PRIVATE ../src)
    add_test(NAME ${name} COMMAND ${name}_test)
endforeach()
