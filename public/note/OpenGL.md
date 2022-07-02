[cs主界面](cs.md)

# OpenGL

## 环境配置

### VS介绍

Visual Studio作为IDE，封装了大部分的编译链接细节。但是为了配置方便，需要了解其中的细节。VS的默认编译器为msvc，一般分为32和64位版本。下面的配置务必注意下载的位数和对应的地址。`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Tools\MSVC\xx`是msvc默认安装时候所在的目录。

- `include`文件夹就是头文件文件夹 ^0e91a2
- `lib`是动态库的文件夹（注意里面x64是本机64位，x86是本机32位 ^c1de5e
- `bin`是编译程序库（注意host64和host86等区别） ^ed90c2

### GLAD

- [glad下载](https://glad.dav1d.de/)
- 选择gl版本，选择模式Profile为Core，点击生成Generate，下载zip文件
- include文件夹中的头文件放入[include](#%5E0e91a2)
- 将src目录下的文件（只有一个glad.c）拷贝到工程目录并添加到工程中

```cpp
#include <glad/glad.h>
```

### GLFW

- [glfw下载](http://www.glfw.org/download.html)下载预编译好的二进制包
- 编译生成的库文件glfw3.lib放入[lib](#%5Ec1de5e)
- include文件夹中的头文件放入[include](#%5E0e91a2)
- 在需要的VS项目当中，进入工程属性选择链接器选项卡-输入，在附加依赖项中添加glfw3.lib

```cpp
#include <GLFW/glfw3.h>
```

### GLM

几何变换库

- [glm下载](https://glm.g-truc.net/0.9.9/index.html)
- include文件夹中的头文件放入[include](#%5E0e91a2)

```cpp
#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>
#include <glm/gtc/type_ptr.hpp>
```

### freetype

加载字体文件库

- [FreeType win版本下载](https://github.com/ubawurinna/freetype-windows-binaries)
- 编译生成的库文件freetype.lib放入[lib](#%5Ec1de5e)
- include文件夹中的头文件放入[include](#%5E0e91a2)
- 附加依赖项加freetype.lib

```cpp
#include <ft2build.h>
#include FT_FREETYPE_H
```

### OpenCV

通用的，强大的图像处理库。

- [OpenCV下载](https://opencv.org/releases/)
- 选择一个地方解压，找到当中的build文件夹
- x64/vc15/lib中的文件放入[lib](#%5Ec1de5e)
- include文件夹中的头文件放入[include](#%5E0e91a2)
- bin文件夹当中的ddl文件（动态链接库）放入[bin](#^ed90c2)
- 附加依赖项加对应的lib文件

```cpp
#include <opencv2/opencv.hpp>
using namespace cv;
```