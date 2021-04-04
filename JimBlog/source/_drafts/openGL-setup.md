---
title: openGL_setup
tags: 
- OpenGL
- Shader
- Pipeline
- ComputerGraphics
- C++
---

# Note
Try to learn OpenGL and computer graphics from run through one great online resource I found on youtube[傅老師](https://www.youtube.com/watch?v=1TVFHIQKCR0&list=PL0luF_aDUOooIB56NOFVTS4ahMzBHS_6z).
The main text book material the teacher used in this tutorial series is [LearnOpenGL](https://learnopengl.com/)

```c
#define GLEW_STATIC
#include <GL/glew.h>  //#include <glad/glad.h>
#include <GLFW/glfw3.h> 
```

> Be sure to include `GLAD` or `GLEW` before `GLFW`. The include file for GLAD includes the required OpenGL headers behind the scenes(like GL/gl.h) so be sure to include `GLAD` before other header files that require OpenGL (like GLFW)

```c
int main()
{
    glfwInit();     // Initialize GLFW
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    //glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);  // <- mac user
 
    return 0;
}
```

Setup the glfwWindow at version `3.3` by using 
- glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3)
- glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3)

We also tell GLFW we want to explicitly use the core-profile. Telling GLFW we want to use the core-profile means we'll get access to a smaller subset of OpenGL features without backwards-compatible features we no longer need.

## CreateWindow
```c
GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);
if (window == NULL)
{
    std::cout << "Failed to create GLFW window" << std::endl;
    glfwTerminate();
    return -1;
}
glfwMakeContextCurrent(window);
```

### Init GLAD
GLAD manages **function pointers** for OpenGL so we want to initialize GLAD before we call any OpenGL function:
```c
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
{
    std::cout << "Failed to initialize GLAD" << std::endl;
    return -1;
}    
```

We pass GLAD the function to load the address of the OpenGL function pointers which is `OS-specific`.
GLFW gives us `glfwGetProcAddress` that defines the correct function based on which OS we're compiling for.

### Init GLEW

```c
glewExperimental = true;
if (glewInit() != GLEW_OK) {
    printf("Init GLEW failed.");
    glfwTerminate();
    return -1;
}
```

## Viewport
Before we can start rendering we have to do one last thing. We have to tell OpenGL the size of the rendering window so OpenGL knows how we want to display the data and coordinates with respect to the window. We can set those dimensions via the `glViewport` function.
```c
glViewport(0, 0, 800, 600);
```


