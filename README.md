GLFW WebGPU Extension
=====================

This is an extension for the great [GLFW](https://www.glfw.org/) library for using it with **WebGPU native**. It was written as part of the [Learn WebGPU for native C++](https://eliemichel.github.io/LearnWebGPU) tutorial series.

 - [Overview](#overview)
 - [Usage](#usage)
 - [Example](#example)
 - [License](#license)

Overview
--------

This extension simply provides the following function:

```C
WGPUSurface glfwGetWGPUSurface(WGPUInstance instance, GLFWwindow* window);
```

Given a GLFW window, `glfwGetWGPUSurface` returns a WebGPU *surface* that corresponds to the window's back-end. This is a process that is highly platform-specific, which is why I believe it belongs to GLFW.

Usage
-----

Just copy `glfw3webgpu.h` and `glfw3webgpu.c` to your project's source tree. Your project must link to an implementation of WebGPU (providing `webgpu.h`) and of course to GLFW.

Example
-------

Thanks to this extension it is possible to simply write a fully cross-platform WebGPU hello world:

```C
#include "glfw3webgpu.h"

#include <GLFW/glfw3.h>
#include <webgpu.h>
#include <stdio.h>

int main(int argc, char* argv[]) {
	// Init WebGPU
	WGPUInstanceDescriptor desc;
	desc.nextInChain = NULL;
	WGPUInstance instance = wgpuCreateInstance(&desc);

	// Init GLFW
	glfwInit();
	glfwWindowHint(GLFW_CLIENT_API, GLFW_NO_API);
	GLFWwindow* window = glfwCreateWindow(640, 480, "Learn WebGPU", NULL, NULL);

	// Here we create our WebGPU surface from the window!
	WGPUSurface surface = glfwGetWGPUSurface(instance, window);
	printf("surface = %p", surface);

	// Terminate GLFW
	while (!glfwWindowShouldClose(window)) glfwPollEvents();
	glfwDestroyWindow(window);
	glfwTerminate();

	return 0;
}
```

**NB** The linking process depends on the implementation of WebGPU that you are using. You can find detailed instructions for the `wgpu-native` implementation in [this Hello WebGPU chapter](https://eliemichel.github.io/LearnWebGPU/getting-started/hello-webgpu.html). You may also check out [`examples/CMakeLists.txt`](examples/CMakeLists.txt).

License
-------

```
MIT License
Copyright (c) 2022-2023 Élie Michel and the wgpu-native authors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
