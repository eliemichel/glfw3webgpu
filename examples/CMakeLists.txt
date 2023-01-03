# This example comes from https://eliemichel.github.io/LearnWebGPU
cmake_minimum_required(VERSION 3.11.0...3.25.0)
project(glfw3webgpu-examples VERSION 1.0.0 LANGUAGES C)

include(FetchContent)
include(utils.cmake)

# Dependencies
FetchContent_Declare(
  glfw-light
  URL      https://eliemichel.github.io/LearnWebGPU/_downloads/eed380b810a75153b7402fee040e2d7c/glfw.zip
  URL_HASH MD5=e4ab5737d99a06b20156780be057da42
)
FetchContent_Declare(
  webgpu
  URL      https://eliemichel.github.io/LearnWebGPU/_downloads/f877ac8b0fa3ded16a7485afd02f1762/wgpu-native-for-any.zip
  URL_HASH MD5=b506e45e4b49facc84832d1c49597870
)
FetchContent_MakeAvailable(glfw-light webgpu)

# The glfw3webgpu target
add_library(glfw3webgpu STATIC ../glfw3webgpu.c)
target_include_directories(glfw3webgpu PUBLIC ..)
target_link_libraries(glfw3webgpu PUBLIC glfw webgpu)

# Example
add_executable(hello-glfw3webgpu hello-glfw3webgpu.c)
target_link_libraries(hello-glfw3webgpu PRIVATE glfw webgpu glfw3webgpu)
set_target_properties(hello-glfw3webgpu PROPERTIES CXX_STANDARD 17)
target_treat_all_warnings_as_errors(hello-glfw3webgpu)
target_copy_webgpu_binaries(hello-glfw3webgpu)
