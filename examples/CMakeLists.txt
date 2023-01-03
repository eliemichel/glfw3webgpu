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
  GIT_REPOSITORY https://github.com/eliemichel/WebGPU-binaries.git
  GIT_TAG        main
)
FetchContent_MakeAvailable(glfw-light webgpu)

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
