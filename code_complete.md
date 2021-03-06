# C/C++代码编写
==================
第一课  堆栈与队列
==================

一、数据结构的基本概念
----------------------

1. 逻辑结构
~~~~~~~~~~~

1) 集合结构(集)：结构中的数据元素除了“属于同一个集合”之外，没有任何关系。

2) 线性结构(表)：结构中的数据元素具有一对一的前后关系。

3) 树型结构(树)：结构中的数据元素具有一对多的父子关系。

4) 网状结构(图)：结构中的数据元素具有多对多的交叉映射关系。

2. 物理结构
~~~~~~~~~~~

1) 顺序结构：结构中的数据元素存放在一段连续的地址空间中。

随机访问方便，空间利用率低，插入删除不便。

2) 链式结构：结构中的数据元素存放在彼此独立的地址空间中，每个独立的空间被称为节点，节点除了保存数据以外，还保存另一个或几个相关节点的地址。

空间利用率高，插入删除方便，随机访问不便。

3. 逻辑结构与物理结构关系
~~~~~~~~~~~~~~~~~~~~~~~~~

+------+------+--------+------+
|      |  表  |   树   |  图  |
+------+------+--------+------+
| 顺序 | 数组 | 顺序树 |      |
+------+------+--------+ 复合 |
| 链式 | 链表 | 链式树 |      |
+------+------+--------+------+

二、数据结构的基本运算
----------------------

1. 创建/销毁
2. 插入/删除
3. 获取/修改
4. 排序/查找

三、数据结构的基本实现
----------------------

1. 堆栈
~~~~~~~

1) 基本特征：后进先出。

2) 基本操作：压入(push)、弹出(pop)。

3) 实现要点：初始化空间、栈顶指针、判空判满。

范例：

基于数组的实现 - sa.h、sa.c、sa_test.c
基于链表的实现 - sl.h、sl.c、sl_test.c
老鼠走迷宫     - sl.h、sl.c、maze.c

2. 队列
~~~~~~~

1) 基本特征：先进先出——FIFO。

2) 基本操作：压入(push)、弹出(pop)。

3) 实现要点：初始化空间、前指针front弹出后指针rear压入、循环使用、判空判满。

范例：

基于数组的实现 - qa.h、qa.c、qa_test.c
基于链表的实现 - ql.h、ql.c、ql_test.c
基于堆栈的实现 - sl.h、qs.h、sl.c、qs.c、qs_test.c

综合运用堆栈和队列：

逆波兰法求解四则运算表达式 - sl.h、ql.h、rpn.h、sl.c、ql.c、rpn.c、rpn_test.c
###### 1. 实现单向链表，并翻转单向链表
```cpp
6 int main(int argc, char const **argv)
{  
  int a[5]  = {1,2,3,4,5};    
  int * ptr = (int * )(&a+1);    
  printf("%d, %d", * (a+1), * (ptr-1));
}
```
* 2, 5. \*(a+1）就是a[1]，\*(ptr-1)就是a[4], &a+1不是首地址+1，系统会认为加一个a数组的偏移，是偏移了一个数组的大小（本例是5个int） int \*ptr=(int \*)(&a+1); 则ptr实际是&(a[5]),也就是a+5 原因如下： &a是数组指针，其类型为 int (\*)[5]; 而指针加1要根据指针类型加上一定的值，不同类型的指针+1之后增加的大小不同 a是长度为5的int数组指针，所以要加 5\*sizeof(int) 所以ptr实际是a[5] 但是prt与(&a+1)类型是不一样的(这点很重要) 所以ptr-1只会减去sizeof(int\*) a,&a的地址是一样的，但意思不一样，a是数组首地址，也就是a[0]的地址，&a是对象（数组）首地址，a+1是数组下一元素的地址，即a[1],&a+1是下一个对象的地址，即a[5].

###### 2. 以下代码有什么问题？
```cpp
int main(int argc, char const **argv)
{  
  char a;
  char * str = &a;
  strcpy(str, "hello");
  printf(str);
  return 0;
}
```
* 没有为str分配内存空间，将会发生异常，问题出在将一个字符串复制进一个字符变量指针所指地址。虽然可以正确输出结果，但因为越界进行内在读写而导致程序崩溃。

###### 3. int (\*s[10])(int) 表示的是什么？
* int (\*s[10])(int) 函数指针数组，每个指针指向一个int func(int param)类型的函数

###### 4. const 有什么用途？（请至少说明两种）
1. 可以定义 const 常量.
2. const可以修饰函数的参数、返回值，甚至函数的定义体。被const修饰的东西都受到强制保护，可以预防意外的变动，能提高程序的健壮性。

###### 5. 在C++ 程序中调用被 C编译器编译后的函数，为什么要加 extern “C”声明？
* C++语言支持函数重载，C语言不支持函数重载。函数被C++编译后在库中的名字与C语言的不同。假设某个函数的原型为： void foo(int x, int y); 该函数被C编译器编译后在库中的名字为_foo，而C++编译器则会产生像_foo_int_int之类的名字。C++提供了C连接交换指定符号extern“C”来解决名字匹配问题。

###### 6.编写strcpy函数,已知strcpy函数的原型是char * strcpy(char * strDest, const char * strSrc);	其中strDest是目的字符串，strSrc是源字符串。
1. 不调用C++/C的字符串库函数，请编写函数 strcpy.
```cpp
char * strcpy(char * strDest, const char * strSrc);
{
  assert((strDest!=NULL) && (strSrc !=NULL));
  char * address = strDest;
  while( (*strDest++ = * strSrc++) != '\0' )
    NULL ;
  return address ;
}
```
2. strcpy能把strSrc的内容复制到strDest，为什么还要char * 类型的返回值？
 * 为了实现链式表达式。例如:int length = strlen( strcpy( strDest, “hello world”) );

**注意：**
1. 一个位段必须存储在同一存储单元（即字）之中，不能跨两个单元。如果其单元空间不够，则剩余空间不用，从下一个单元起存放该位段。
2. 可以通过定义长度为0的位段的方式使下一位段从下一存储单元开始。
3. 可以定义无名位段。
4. 位段的长度不能大于存储单元的长度。
5. 位段无地址，不能对位段进行取地址运算。
6. 位段可以以%d，%o，%x格式输出。


预备知识
---
本课程不需要特别的预备知识。如果您学过编程语言（C、Java、Lisp、Javascript等），理解起课程代码来会快一些；但这并非必需的；如果没有编程经验，那也不要紧，只不过是同时学习两样东西（编程语言+OpenGL）而已。

为了让代码尽量简单，我特意用“傻瓜式C++”编写了所有课程代码。代码中没有模板（template）、类或指针。也就是说，即使您只懂Java也能理解所有内容。

忘记一切
---
像前面讲的那样，本课程无需预备知识；不过，请您先把glBegin()这类“旧版OpenGL”的东西忘掉吧。
多数网上教程还在讲“旧版OpenGL”（OpenGL 1、2）。但在这里，您将学到新版OpenGL（OpenGL 3、4）。所以，为了不让您的脑袋乱成一锅粥，请先把过时的知识清空吧。

生成课程中的代码
---
所有课程代码都能在Windows、Linux、和Mac上生成，过程大体相同：

1. **更新驱动** ！！赶快更新吧。别怪我没提醒您哟。
2. 下载C++编译器
3. 安装CMake
4. 下载全部课程代码
5. 用CMake创建工程
6. 编译工程
7. 试试这些例子！

各平台的详细过程如下。您可能要根据实际情况做些调整。若不确定，请按照Windows平台说明操作。

###在Windows上生成###

1. 更新驱动。请直接去NVIDIA或者AMD的官网下载。若不清楚GPU的型号，请在：控制面板->系统和安全->系统->设备管理器->显示适配器 查看。如果是Intel集成显卡，一般由OEM（Dell、HP…）提供驱动。
2. 建议用Visual Studio 2010 Express来编译。[这里](http://www.microsoft.com/express/Downloads/#2010-Visual-CPP)可以免费下载。若您偏爱MinGW，推荐使用[Qt Creator](http://qt-project.org/)。IDE可根据个人喜好选择。下列步骤是按Visual Studio讲解的，其他IDE差别不大。
3. 从[这里](http://www.cmake.org/cmake/resources/software.html)下载安装CMake
4. [下载课程源码](http://www.opengl-tutorial.org/download/)，解压到```C:\Users\XYZ\Projects\OpenGLTutorials\```
5. 启动CMake。将第一栏路径指向刚才解压缩的文件夹；不确定就选包含CMakeLists.txt的文件夹。第二栏填CMake输出路径。例如```C:\Users\XYZ\Projects\OpenGLTutorials-build-Visual2010-32bits\```，或者```C:\Users\XYZ\Projects\OpenGLTutorials\build\Visual2010-32bits\```。注意，此处可随意填写，不一定要和源码在同一文件夹。

![CMake](http://www.opengl-tutorial.org/assets/images/tuto-1-window/CMake.png)

6. 点击Configure。由于是首次configure工程，CMake会让您选择编译器。根据步骤1选择。如果您的系统是Windows 64 bit的，选64 bit。不清楚就选32 bit。
7. 再点Configure直至红色警告全部消失。点Generate。Visual Studio工程创建完毕。到这里就不再需要CMake了，可以卸载掉。
8. 打开```C:\Users\XYZ\Projects\OpenGLTutorials-build-Visual2010-32bits\```会看到Tutorials.sln文件，用Visual Studio打开它。

![directories](http://www.opengl-tutorial.org/assets/images/tuto-1-window/directories.png)

在*Build*菜单中，点*Build All*。每个课程代码和依赖项都将编译。生成的可执行文件会出现在 ```C:\Users\XYZ\Projects\OpenGLTutorials\```。这一步一般没什么问题。

![visual_2010](http://www.opengl-tutorial.org/assets/images/tuto-1-window/visual_2010.png)

9. 打开```C:\Users\XYZ\Projects\OpenGLTutorials\playground```，运行playground.exe，会弹出一个黑色窗口。

![empty_window](http://www.opengl-tutorial.org/assets/images/tuto-1-window/empty_window.png)

也可以在Visual Studio中运行任意一课的代码，但必须先设置工作目录：右键点击Playground，选择Debugging、Working Directory、Browse，设置路径为```C:\Users\XYZ\Projects\OpenGLTutorials\playground\```。验证一下。再次右键点击Playground，“Set as startup project”。按F5就可以调试了。

![StartupProject](http://www.opengl-tutorial.org/assets/images/tuto-1-window/StartupProject.png)

###在Linux上生成###

Linux版本众多，这里不可能列出所有的平台。可根据实际情况自行调整，也不妨看一下发行版文档。

1. 安装最新驱动。强烈推荐闭源的二进制驱动；不开源但是好用。如果发行版不提供自动安装，试试[Ubuntu指南](http://help.ubuntu.com/community/BinaryDriverHowto)。
2. 安装必需的编译器、工具和库。完整清单如下： *```cmake make g++ libx11-dev libgl1-mesa-dev libglu1-mesa-dev libxrandr-dev libxext-dev```*。命令行是```sudo apt-get install``` 或者 ```su && yum install```。
3. [下载课程源码](http://www.opengl-tutorial.org/download/)并解压到 ```~/Projects/OpenGLTutorials/```
4. 输入如下命令 :
- ```cd ~/Projects/OpenGLTutorials/```
- ```mkdir build```
- ```cd build```
- ```cmake ..```
5. build目录下多了一个刚刚创建的makefile文件
6. 键入“make all”。每个课程代码和依赖项都会被编译。生成的可执行文件在 ```~/Projects/OpenGLTutorials/```。应该不会出什么错误。
7. 打开```~/Projects/OpenGLTutorials/playground```，运行./playground会弹出一个黑色窗口。

> **提示**：推荐使用[Qt Creator](http://qt-project.org/)作为IDE。值得一提的是，Qt Creator原生支持CMake，调试很方便。如下是QtCreator使用说明：

1. 在QtCreator中打开Tools->Options->Compile->Execute->CMake
2. 设置CMake路径。比如 ```/usr/bin/cmake```
3. File->Open Project；选择 tutorials/CMakeLists.txt
4. 选择生成目录，最好选择tutorials文件夹外面
5. 还可以在参数栏中设置 ```-DCMAKE_BUILD_TYPE=Debug```。验证一下。
6. 点击下面的锤子图标。现在教程可以从```tutorials/```文件夹启动了。
7. 要想在QtCreator中运行教程源码，点击Projects->Execution parameters->Working Directory，选择着色器、纹理和模型所在目录。以第二课为例：```~/opengl-tutorial/tutorial02_red_triangle/```

###在Mac上生成###
Mac OS不支持OpenGL 3.3。最近，搭载MacOS 10.7 Lion和兼容型GPU的Mac机可以跑OpenGL 3.2了，但3.3还不行；所以我们用2.1移植版的课程代码。除此外，其他步骤和Windows类似（也支持Makefiles，此处不赘述）：

1. 从Mac App Store安装XCode
2. [下载 CMake](http://www.cmake.org/cmake/resources/software.html)，安装.dmg。无需安装命令行工具。
3. [下载课程源码](http://www.opengl-tutorial.org/download/)（2.1版！！）解压到如```~/Projects/OpenGLTutorials/```
4. 启动CMake （Applications->CMake）。将第一栏路径指向刚才解压缩的文件夹，不确定就选包含CMakeLists.txt的文件夹。第二栏填CMake输出路径。例如```~/Projects/OpenGLTutorials_bin_XCode/```。注意，这里可以随便填，不一定要和源码在同一文件夹。
5. 点击Configure。由于是首次configure工程，CMake会让您选择编译器。选择Xcode。
6. 再点Configure直至红色警告全部消失。点Generate。Xcode项目创建完毕。到这里就不再需要CMake了，可以卸载掉。
7. 打开```~/Projects/OpenGLTutorials_bin_XCode/```会看到Tutorials.xcodeproj文件：打开它。
8. 选择一课，在Xcode的Scheme面板上点击Run按钮编译和运行：

![Xcode-projectselection](http://www.opengl-tutorial.org/assets/images/tuto-1-window/Xcode-projectselection.png)

###关于Code::Blocks的说明###
由于C::B和CMake中各有一个bug，您必须在Project->Build->Options->Make commands中手动设置编译命令，如下图所示：

![CodeBlocksFix](http://www.opengl-tutorial.org/assets/images/tuto-1-window/CodeBlocksFix.png)

同时您还得手动设置工作目录：Project->Properties->Build targets->tutorial N->execution working dir（即```src_dir/tutorial_N/```）。

运行课程例子
---

一定要在正确的目录下运行课程例子：您可以双击可执行文件；如果想用命令行，请用cd命令切换到正确的目录。

若想从IDE中运行程序，别忘了看看上面的说明——先正确设置工作目录。

如何学习本课程
---
每课都附有源码和数据，可在```tutorialXX/```找到。不过，建议您先不要改动这些工程，而是将它们作为参考；推荐您在```playground/playground.cpp```中做试验，怎么折腾都行。要是弄乱了就去粘一段课程代码，一切就会恢复正常。

我们会在整个教程中提供代码片段。不妨一边看教程，一边把代码复制到playground里调试。动手实验才是王道。单纯看别人写好的代码学不了多少东西。即便只是粘贴一下代码，也会碰到不少问题。

打开一个窗口
---
终于到了写OpenGL代码的时刻！
呵呵，其实还没到真正写OpenGL代码的时刻。有些教程喜欢讲一些“底层”的细节，好让您清楚每一步的原理。这些内容往往索然无味，而且用处也不大。因此，我们直接把窗口、键盘消息等细节交给第三方库GLFW来处理。您也可以使用Windows的Win32 API、Linux的X11 API，或Mac的Cocoa API；或者SFML、FreeGLUT、SDL等库，请参见[链接页](http://www.opengl-tutorial.org/miscellaneous/useful-tools-links/)。

开工啦。从处理依赖库开始：我们要用一些基本库在控制台显示消息：
```cpp
// Include standard headers
#include <stdio.h>
#include <stdlib.h>
```

然后是GLEW库。其原理我们以后再说。
```cpp
// Include GLEW. Always include it before gl.h and glfw.h, since it's a bit magic.
#include <GL/glew.h>
```
我们使用GLFW库处理窗口和键盘消息，把它也包含进来：
```cpp
// Include GLFW
#include <GL/glfw.h>
```
下文中的GLM是个很有用3D数学库，我们暂时用不到，但很快就会派上用场。GLM库很好用，但也没什么神奇的，您不妨自己试着写一个。添加“using namespace”，这样就可以不用写“glm::vec3”，直接写“vec3”。
```cpp
// Include GLM
#include <glm/glm.hpp>
using namespace glm;
```
把这些#include都粘贴到playground.cpp。编译时编译器报错，说缺少main函数，那就创建一个 呗：
```cpp
int main(){
```
首先初始化GLFW ：
```cpp
// Initialise GLFW
if( !glfwInit() )
{
fprintf( stderr, "Failed to initialize GLFW\n" );
return -1;
}
```
```cpp
glfwOpenWindowHint(GLFW_FSAA_SAMPLES, 4); // 4x antialiasing
glfwOpenWindowHint(GLFW_OPENGL_VERSION_MAJOR, 3); // We want OpenGL 3.3
glfwOpenWindowHint(GLFW_OPENGL_VERSION_MINOR, 3);
glfwOpenWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE); //We don't want the old OpenGL

// Open a window and create its OpenGL context
if( !glfwOpenWindow( 1024, 768, 0,0,0,0, 32,0, GLFW_WINDOW ) )
{
fprintf( stderr, "Failed to open GLFW window\n" );
glfwTerminate();
return -1;
}

// Initialize GLEW
glewExperimental=true; // Needed in core profile
if (glewInit() != GLEW_OK) {
fprintf(stderr, "Failed to initialize GLEW\n");
return -1;
}

glfwSetWindowTitle( "Tutorial 01" );
```
生成并运行。一个窗口弹出后立即关闭了。可不是嘛，还没设置等待用户按Esc键再关闭呢：
```cpp
// Ensure we can capture the escape key being pressed below
glfwEnable( GLFW_STICKY_KEYS );

do{
// Draw nothing, see you in tutorial 2 !

// Swap buffers
glfwSwapBuffers();

} // Check if the ESC key was pressed or the window was closed
while( glfwGetKey( GLFW_KEY_ESC ) != GLFW_PRESS &&
glfwGetWindowParam( GLFW_OPENED ) );
```
第一课就到这啦！第二课会教大家绘制三角形。

> &copy; http://www.opengl-tutorial.org/

> Written with [Cmd Markdown](https://www.zybuluo.com/mdeditor).
