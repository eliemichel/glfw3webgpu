# This example comes from https://eliemichel.github.io/LearnWebGPU
cmake_minimum_required(VERSION 3.11.0...3.25.0)
project(glfw3webgpu-examples VERSION 1.0.0 LANGUAGES C)

include(FetchContent)
include(utils.cmake)

# Dependencies
if (EMSCRIPTEN)
  
  add_library(glfw INTERFACE)
  target_link_options(glfw INTERFACE -sUSE_GLFW=3)

  add_library(webgpu INTERFACE)
  target_link_options(webgpu INTERFACE -sUSE_WEBGPU=1)
  function(target_copy_webgpu_binaries)
  endfunction()

else (EMSCRIPTEN)

  FetchContent_Declare(
    glfw-light
    URL      https://eliemichel.github.io/LearnWebGPU/_downloads/6873a344e35ea9f5e4fc7e5cc85d3ab8/glfw-3.4.0-light.zip
    URL_HASH MD5=e6684142080457e9227e4f78a6f84fc2
  )
  FetchContent_Declare(
    webgpu
    GIT_REPOSITORY https://github.com/eliemichel/WebGPU-distribution.git
    GIT_TAG        7ed61fb96d972a4c61ccc79af1139eba2a0ce4c4  # fetch
  )
  FetchContent_MakeAvailable(glfw-light webgpu)

endif (EMSCRIPTEN)

# The glfw3webgpu target
# NB: We specify a second argument only because this is an out-of-tree
# directory, no need to do this in your case.
add_subdirectory(.. glfw3webgpu)

# Example
add_executable(hello-glfw3webgpu hello-glfw3webgpu.c)
target_link_libraries(hello-glfw3webgpu PRIVATE glfw webgpu glfw3webgpu)
set_target_properties(hello-glfw3webgpu PROPERTIES CXX_STANDARD 17)
target_treat_all_warnings_as_errors(hello-glfw3webgpu)
target_copy_webgpu_binaries(hello-glfw3webgpu)
