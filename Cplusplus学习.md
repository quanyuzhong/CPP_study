# Cplusplus学习





## 使用vscode的g++相关编译链接命令

1. `g++ hello.cpp Log.cpp -o hello`:首先，检查用户是否在编译命令中包含了所有相关的源文件，比如Log.cpp。用户当前的编译命令是g++ hello.cpp -o hello，如果Log和InitLog的实现都在Log.cpp中，那么必须将Log.cpp一起编译，命令应该是g++ hello.cpp Log.cpp -o hello。用户可能没有添加Log.cpp到编译命令中，导致链接器找不到这两个函数的实现。

## C plus工作流程

1. 当编译器收到一个源文件时它做的第一件事就是预处理你所有的预处理指令（preprocessor statement）--->预处理指令就是带#的命令。*被称为预处理指令是因为它们发生在真正的编译之前* **将该文件的内容复制到其他文件中去**
2. mian函数是进入程序的入口，当我们要运行我们的程序，计算机会从该函数里的代码开始执行
3. 我们的程序是一行一行被执行的
4. main函数可以允许你不返回任何类型的值，如果你不返回任何值，它会返回0，**但这只适合于main函数**

```c++
std::cout << "Hello  world!" << std::endl;
std::cin.get();
```

* `<<`是**被重载的符号**：想象成**函数**=**运算符** 
* 把`Hello World`字符串传入这个`cout`这个符号，这个特定的符号把**包含的内容打印在控制台std中**
* 又传入一个`endl`到`std`中：**endl就是告诉我么的控制台前进到另一行！**
* `cin.get()`：扽带我们输入回车然后才会接着执行下一行代码

```c++
#include <iostream>

int main()
{
    std::cout << "Hello World!" << std::endl;
    std::cin.get();
}

```

1. 带#的预处理指令会在**编译文件之前被评估**--->其实就是在iostream文件里面的所有内容拷贝到这个文件`main.cpp`中去--->现在就是为了使用`cout,cin`这两个函数
2. 编译器会把我们的C++代码转化成实际的机器码
3. 项目中每个c++文件都会产生一个obj文件

## 编译器如何进行工作

从`text`到可执行文件`binary`，基本上由两个主要操作需要发生！

### compiling(编译)

目标：C++编译器唯一要做的就是**把文本文件text file变成中继格式object file**

1. **Pre-processing**:编译器会遍历所有的预处理语句：`include,define if,if def`

   * `# include`：指定要包含的文件，编译器的预处理器部分会打开该文件，读取该内容，并将其粘贴到**你编写`include`语句的文件中**

     *这里演示是将main函数中的`}`删除,用一个只包`}`的头文件代替*

     ```c++
     #include <iostream>
     #include "../include/Log.h"
     int main()
     {
         Log("Hello World!");
         std::cin.get();
     #include "../include/test.h"
     ```

2. **编译过程**：C++ 语法错误的检查，就是在这个阶段进行。在检查无误后，g++ 把代码翻译成汇编语言。

**汇编代码中生成的是和CPU架构相关的汇编指令，不同CPU架构采用的汇编指令集不同，生成的汇编代码也不一样：**

```c++
g++ -S main.ii -o main.s
```

3. **g++ 汇编阶段：生成目标代码 *.o**:到编译阶段，代码还都是人类可以读懂的。汇编这一阶段，正式将汇编代码生成机器可以执行的目标代码，也就是二进制码。

### Linking(链接)

**找到每个符号和函数的位置并将它们链接在一起：每个文件都将独立地被编译成一个单独的目标文件作为翻译单元，它们彼此之间没有任何联系，Linking实际上将这些目标文件链接到一个程序中，这就是链接器的主要目的**

1. ```c++
   static int Multiply(int a,int b);
   ```

   其中这个`static`的意义是告诉**链接器**这个函数只在这个文件中使用，不会在其他文件中得到引用！

2. ```c++
   #######################Log函数####################
   void Logr(const char* message){
     std::cout << message << std::endl;
   }
   #######################main函数###################
   #include<iostream>
   
   void Log(const char* message);
   
   static int Multiply(int a,int b){
     Log("Multiply");
     return a+b;
   }
   
   int main(){
     //std::cout << Multiply(5,8) << std::endl;
     std::cin.get();
   }
   ```

   就算在`main`函数中，注释掉了`Multiply`函数中关于`Log`函数的错误，但是还是会有链接报错(*在`Multiply`函数没有添加`static`时*)：**即使在主函数`main`没有用到这个函数，但是不代表这个函数没有作用，它在技术上未来仍然可以使用：因此依然会出现链接报错**

3. 两个同名的函数和签名**必须有相同的参数和相同的返回值**

4. 如果在多个文件有相同的函数，链接器不知道究竟链接到那一个函数，这是**模凌两可**。

## 变量

变量是存储数据的载体，我们想要更改、修改、读取和写入的数据，我们都需要将这些数据存储在**一个叫做变量的东西中**。

1. 变量允许我们命名存储在内存中的一段数据
2. Cplusplus**不同的原始数据类型之间唯一的区别是大小**，这个变量占用多大的内存
3. 数据类型的实际大小取决于编译器，因此它可能有所不同，具体取决于你使用的编译器！

4. 常见的数据类型的大小：

* `int`：4个bytes的数据，有正数和负数
* `unsigned int`：4个bytes的数据，只有正数
* `char`：1个byte的数据
* `short`：2个bytes的数据
* `long`：取决于编译器，通常是4个bytes的数据
* `long long`：通常是8个bytes的数据
* `bool`：0或1，除了0以外的任何数字都表示真，但是由于我们无法真正寻址bit而是寻址byte,因此`bool`类型占用一个字节

**类型只是我们方便使用而构造的虚构，最重要的还是内存**

**你可以为上面五项都使用`unsigned`，这样在实际操作的时候最前面的一个比特就不用来作为符号位**

**除了`char`其余数据类型，cout都会将其看成是一个数值而非一个字符**

```cpp
char a = 65;
std::cout << a << std::endl;
//输出的是'A'

##################

short a = 65;
std::cout << a << std::endl;
//输出的是65

/*因此cout会将char类型的数值自动识别成一个字符*/
```

5. 我们可以使用`sizeof`函数来表示该数据类型使用多少个字节来表示！

## 函数

1. 函数的三要素：类型、参数、返回值
2. 函数的目的是为了**避免重复写自己重复的代码！**
3. 不要过分地使用函数，实际上它会让你的程序变慢，每次调用函数都需要保护现场等工作

*我们每次调用一个函数时，编译器都会生成一个调用指令，我们都需要为该函数创建整个堆栈框架，这意味着我们必须将参数之类的东西推送到堆栈上，我们还需要把返回地址推送到堆栈上，然后实际上跳转到二进制文件到不同部分，以便开始执行， ***会在内存中反复跳转！**

4. 具有返回类型的函数是需要返回值，但是`main`函数是一个特殊函数，只有主函数不受这种必须返回值的限制，如果你不写返回值他会默认你返回0
5. 函数分为**声明**和**定义**
   1. 声明写到单独的头文件中
   2. 定义写到一个CPP文件



## Header(头文件)

1. 在一个文件中，CPP实际上为了不犯错误**，我们需要告诉该函数确实存在，只是存在在另外的文件定义，这就是函数声明需要出现的地方！**

2. 添加`header guards`表头保护：

```c++
#pragma once
```

**表头保护是指防止一些在头文件的结构体在调用的时候被重复定义，因此使用表头保护来识别是否已经被定义，防止重复定义**

```c++
###########Log.h##############
struct Play{
  
};
############common.h##########
#include"Log.h"
###########main.cpp###########
#include"Log.h"
#include"common.h"
//这个时候会报错指示Play结构体被重定义了
./Log.h:5:8: error: redefinition of 'Play'
```

3. 尖括号和引号：
   1. 尖括号：适用于编译器包含路径！-->标准库
   2. 引号：适用于所有内容！

## VsCode如何进行调试

VsCode主要有两个文件:

### tasks.json文件

VS Code中的任务可以配置为运行脚本和启动进程，因此许多现有工具都可以在VS Code中使用，而无需输入命令行或编写新代码。

**在`VS Code`中可以自定义一些task（任务），这些任务会帮你自动化执行一些东西。任务的配置文件是`tasks.json`。我们希望定义一个编译程序的task，以后调试（`debug`）之前都会自动执行这个task。**

```json
{
  // Tasks in VS Code can be configured to run scripts and start processes
  // so that many of these existing tools can be used from within VS Code 
  // without having to enter a command line or write new code.
  // Workspace or folder specific tasks are configured from the tasks.json file in the .vscode folder for a workspace.
  "version": "2.0.0",
  "tasks": [
    {
      // The task's label used in the user interface.
      // Terminal -> Run Task... 看到的名字
      "label": "g++ compile",
      // The task's type. For a custom task, this can either be shell or process.
      // If shell is specified, the command is interpreted as a shell command (for example: bash, cmd, or PowerShell).
      // If process is specified, the command is interpreted as a process to execute.
      "type": "shell",// shell: 输入命令
      // The actual command to execute.
      // 因为g++已经在环境变量中了，所以我们这里写命令就行不用写g++的绝对路径
      "command": "g++",
      "args": [
        "${file}", // 表示当前文件（绝对路径）
        // 在这里添加你还需要链接的.cpp文件
        "-o",
        "${fileDirname}/${fileBasenameNoExtension}.out",
        "-W",
        "-Wall",
        "-g",
        "-std=c++17",
      ],
      // Defines to which execution group this task belongs to.
      // It supports "build" to add it to the build group and "test" to add it to the test group.
      // Tasks that belong to the build/test group can be executed by running Run Build/Test Task from the Command Palette (sft cmd P).
      // Valid values:
      //   "build",
      //   {"kind":"build","isDefault":true}, 
      //   "test",
      //   {"kind":"test","isDefault":true}, 
      //   "none".
      "group": {
        "kind": "build",
        "isDefault": true, // Defines if this task is the default task in the group.
      },
      // Configures the panel that is used to present the task's output and reads its input.
      "presentation": {
        // Controls whether the executed command is echoed to the panel. Default is true.
        "echo": true, // 打开可以看到编译的命令，把命令本身输出一次
        // Controls whether the terminal running the task is revealed or not. Default is "always".
        //   always: Always reveals the terminal when this task is executed.
        //   silent: Only reveals the terminal if the task exits with an error or the problem matcher finds an error.(会显示错误，但不会显示警告)
        //   never: Never reveals the terminal when this task is executed.
        "reveal": "silent", // 控制在集成终端中是否显示。如果没问题那我不希望终端被切换、如果有问题我希望能看到编译过程哪里出错，所以选silent(可能always会好一些)
        // Controls whether the panel takes focus. Default is false.
        "focus": false, // 我的理解是：是否将鼠标移过去。因为这个是编译任务，我们不需要输入什么东西，所以选false
        // Controls if the panel is shared between tasks, dedicated to this task or a new one is created on every run.
        "panel": "shared", // shared:不同任务的输出使用同一个终端panel（为了少生成几个panel我们选shared）
        // Controls whether to show the `Terminal will be reused by tasks, press any key to close it` message.
        "showReuseMessage": true, // 就一句话，你想看就true，不想看就false
        // Controls whether the terminal is cleared before executing the task.
        "clear": false, // 还是保留之前的task输出信息比较好。所以不清理
      },
      // Other two choices: options & runOptions (cmd I to use IntelliSense)
      "options": {
        // The current working directory of the executed program or script. If omitted Code's current workspace root is used.
        "cwd": "${workspaceFolder}",// 默认就是这个，删掉也没问题
      },
      // problemMatcher: 用正则表达式提取g++的输出中的错误信息并将其显示到VS Code下方的Problems窗口
      // check: https://code.visualstudio.com/docs/editor/tasks#_defining-a-problem-matcher
      "problemMatcher": {
        "owner": "cpp",
        "fileLocation": "absolute",
        "pattern": {
          "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
          "file": 1,
          "line": 2,
          "column": 3,
          "severity": 4,
          "message": 5,
        },
      },
      // 官网教程 https://code.visualstudio.com/docs/cpp/config-clang-mac#_build-helloworldcpp 
      // 提到了另一种problemMatcher，但试了之后好像不起作用，甚至还把我原本的电脑搞出了一些问题……
    },
    {
      "label": "Open Terminal.app",
      "type": "shell",
      "command": "osascript -e 'tell application \"Terminal\"\ndo script \"echo now VS Code is able to open Terminal.app\"\nend tell'",
      "problemMatcher": [],
      "group": "none",
    }
  ]
}
```



### Launch.json

Visual Studio Code的关键功能之一是其出色的调试支持。VS Code的内置调试器有助于加速您的编辑、编译和调试循环。

VS Code将调试配置信息保留在位于工作区（项目根文件夹）的.vscode文件夹中的launch.json文件中。

**如果你需要`debug`，那么`VS Code`提供了这样的平台。`debug`的配置文件是`launch.json`。**

##  如何在Visual Studio进行Debug

### 设置断点

1. 设置断点本质是让这个程序在执行到这个位置的时候停下来以便于**我们检查内存！**

   * *能够查看内存对于诊断程序中的问题非常有用！可以检查每个变量在执行程序时被设置的值，不应该设置这个值，本质上是错误的*
   * 在Visual Studio上你可以按`F9`来对该行设置断点，也可以点击该行左边的小圈圈！

   * 在进行调试的时候，请确保你的电脑处于调试模式，而不要处于发布模式

![image-20250301211033300](/Users/quayuzhong/Desktop/截图图片/image-20250301211033300.png)

2. **如果处于`release`模式，编译器会改变代码的顺序，你的断点可能无法被命中，因为你的程序已经重新排列了** 

   * `跳出F11`：是跳出当前函数，如果是主函数则直接结束`DeBug`

   * `逐过程F10`:在当前函数一步一步一个语句执行，即使这个语句是一个函数，也直接执行

   * `逐语句F9`:真正的一条语句一条语句执行！

3. 黄色箭头标注的地方实际上这条语句还没有被执行！
4. 如果在`for循环`中执行多次已经判断出该循环语句不会出现问题了，如何快速跳到循环之外的其他语句中去？

**在循环语句外的语句设置一个断点然后执行`F5`即可**

#### 设置断点的操作面板

1. **查看内存视图**：用来查看整个程序的内存！它显示我们程序的所有内存！

![image-20250301213057891](/Users/quayuzhong/Desktop/截图图片/image-20250301213057891.png)

![image-20250301213124063](/Users/quayuzhong/Library/Application Support/typora-user-images/image-20250301213124063.png)

* 最左边是**内存地址**
* 中间是**实际数据以十六进制表示的实际值格式**
* 最右边是这些数字的`asky 解释`

**程序实际上只是由内存组成的** 

### 读取内存 

### 

### Debug模式和release模式

Debug模式说明在编译时，编译器不会对我们的代码进行任何的优化；而release模式会对我们的代码进行优化以便在程序运行的时候能够加快执行速度

* 打断点和看内存使用采用`Debug`模式
* 在实际生成应用程序的时候采用`release`模式

## 如何在Visual Studio上设置进行C++项目编写 

1. 首先介绍一下在创建项目的时候，会有一个`filter`文件:它会创建一系列虚拟文件夹但不会在硬盘目录中表现出来 ：**这是一种虚拟组织方案**，其中的`Header Files`，`REsource Files`都是类似这样的 
2. 我们可以采用一个按钮来得到实际我们的项目文件以便在实际项目中方便区分

![image-20250302093118758](/Users/quayuzhong/Desktop/截图图片/image-20250302093118758.png)

3. **为Visual Studio设置项目目录结构**
4. 删除 **解决方案**

## 条件语句(CONDITION AND BRANCH)

运行条件语句实际上进行两个操作：

1. 评估实际条件
2. 根据评估结果执行分支语句

在应用程序中（即一系列机器指令），则通过条件指令跳转相应的机器指令的位置:**说明在硬件中实际上分支和跳转语句会造成一部分开销**

*如果你想要编写非常快的代码，你可能在写代码的时候要减少`if语句`的使用*

### 反汇编视图

![image-20250301232319009](/Users/quayuzhong/Desktop/截图图片/image-20250301232319009.png) 

![image-20250301232345024](/Users/quayuzhong/Desktop/截图图片/image-20250301232345024.png)

可以看到我们每一条C++指令如何与汇编语言相对应。

实际上我们可以逐步执行执行此汇编程序，甚至可以查看CPU中此寄存器实际包含的内容！ 

## LOOPs（循环）

1. while
2. for
3. Do...while

本质上一样的，可以做相同的事情

## Control Flow（控制流语句）(continue,break,return)

1. Continue:只能在循环中使用，表示转到此循环中的下一次迭代 
2. break:退出循环，可以在循环和switch语句中使用。
3. return：完全退出你的函数， **对于有返回值的函数必须返回其返回值否则会报错**，return可以在代码的任何地方使用

## 指针(Pointers)

1. 指针是一个存储**内存地址**的整数，告诉我们指定的内存在哪里

**内存是计算机可以为我提供最重要的资源，它用于一切，因此掌握指针能够帮助我们更好地控制该内存的使用！**

不同类型的指针只是虚构的，为了方面查看， **不同类型的指针本质上都是大小相同的一个整数用于存储内存地址**，但是指定好指针的数据类型可以在使用`*ptr`赋值时告诉编译器这个数据的意义是一个整数或什么

2. 在计算机中 **为每一个字节设置了一个地址**
3. 指针本质上也是 **变量**
4. 双指针和三指针：就是指向指针的指针 
5. 计算机的字节序是**反向的**，小端模式还是大端模式，我忘记了

### 原始指针

1. `*ptr`：访问该内存地址的数据





## 引用(References)

```cpp
int a = 5;
int& ref = a;
ref = 2;
std::cout << ref << std::endl;
//这时候我们能够发现a和ref都变成了2
```



1. 它们实际上 **不占用内存空间，只是为已有的变量设置了一个别名，在实际的编译当中不会出现两个变量，只会保留其中的一个原始变量**
2. 实际上就是给变量取别名，但是占用的是同一个内存空间！
3. 有点类似于指针！
4. 当你创建一个引用时，不像变量可以暂时不初始化，**你必须立刻将它分配给某个东西**，之后这个引用就特定是这个变量的副本，不可以再是其他变量的副本了

```cpp
int a= 5;
int b = 8;
int& ref = a;//之后ref一直是a的副本
ref = b;
std::cout << a <<std::endl;
std::cout << b <<std::endl;
//输出 8 8 
//这里只是将b的值赋给了ref(a)，本质上ref还是a的副本

//但是指针比引用更加灵活
int a= 5;
int b = 8;
int* ref = &a;//之后ref一直是a的副本
*ref = 1;
ref = &b;
*ref = 2;
std::cout << a <<std::endl;
std::cout << b <<std::endl;
//这里输出的是 1 2
//ref从指向a的内存地址变成了指向b的内存地址，不会像引用创建之后就固定和一个变量永久绑定
```

5. **综上所述，引用能做的任何事情本质上指针都能做，如果你能够使用`references`代替`pointer`一定要这样做，因为更干净，更适合阅读**

```cpp
void AddoneValue(int value)
{
  value++;
}

int a = 5;
AddoneValue(a);
std::cout << a << std::endl;
//你可以发现值还是等于5
//因为引用这个函数相当于在你把a = 5的值传递给了一个新创建的变量value,使value的值也等于5，然后value++等于6，然后函数结束后会释放使用该函数的内存空间
void AddoneValue(int* value)
{
  value++;
}

int a = 5;
AddoneValue(&a);
std::cout << a << std::endl;
//可以发现值是等于6
//因为引用这个函数相当于你把a所在的内存空间传递给了一个新创建的指针变量value，这个value指向的内存变量和a所指向的内存变量是同一个内存变量，因此使用`*value`相当于修改该内存地址的值和a的值是相同的，所以最后输出a的值等于6
void AddoneValue(int& value)
{
  value++;
}

int a = 5;
AddoneValue(a);
std::cout << a << std::endl;
//可以发现值是等于6
//因为引用这个函数相当于你创建了一个a的副本，这个value本质上就是a本身，而且不占用内存空间，即没有新创建一个value的变量，因此修改value相当于就是修改a,因此最后输出a的值等于6
```

**综上所述，引用能做的任何事情本质上指针都能做，如果你能够使用`references`代替`pointer`一定要这样做，因为更干净，更适合阅读**



## 运算符

C和C++的运算符实际上是以某种方式在 标准库中实现 

1. 在绝大多数的原始数据结构中，判断两个整数是否相等时，实际上就是 获取它们所占用的内存，然后比较字节，比较字节中的每一位是否相等！



## 类

1. 类是一种 **对数据(variables)进行分组的方式或者函数(Function)结合在一起**

2. 实际上是在 **创建一种新的变量类型**

3. 类组成的 **实际变量** 称为 **对象** ，新的对象变量称为 **实例** 

4. 在默认情况下，类将所有内容设置成私有， **这意味着只有该类中的函数才可以访问s**

   * 如果我们想让类在主函数中被访问，我们需要 **将其公开** ：公开意味着  我们可以在类之外的任何地方访问这些变量

     ```cpp
     class Player{
     public:
       int x, y;
       int speed;
     };
     ```

### 日志类

我们将程序的信息打印到`console`中去 ;控制我们发送给控制台日志的级别

* `error`：只有`error`级别可以显示
* `warning`:只有`warning`和`error`级别的日志信息可以被打印出来，`trace`级别的不行
* `trace`：所有级别都可以

1.  用于代码的编写和调试

### 类和结构体的区别

1. 本质上没有区别，只是在可见性方面 **类默认是private，而结构体默认是public**

2. 在课程举了一个例子：如果你使用`#define struct class`则你使用`struct`也是默认 **private**

3. 既然class能够满足struct的一切需求，只是在可见性方面有区别，那为什么要保留struct呢：

   **因为结构体是 C 语言中的一个遗留特性，可以将变量组合成一个用户定义的类型，但不能包含函数（因为 C 语言没有像方法这样的面向对象特性）。**但是CPP中`struct`是可以包含函数的，只是为了C和CPP的向后兼容性

4. 什么时候使用结构体什么时候使用类:

   1. 当你是只想在结构中表示一些数据，优先采用结构体
   2. 如果有一个充满`Function`或者需要继承的东西，我会使用类



## static关键字

1. 一种是在类或结构体之外使用`static`关键字

**在程序链接linker时该静态的符号将只具备内部含义，它只在你定义它的内部的翻译单元可见，其他翻译单元对该变量时不可见的，它只能被链接在该翻译单元内部**

2. 另一种是在类或结构体内部使用`static`关键字

   1. **该类实际上与该类所有的实例共享这个变量的内存，这意味着你创建的该类或结构体的所有实例中，只会有一个该静态变量的实例，类似的事情也适用于类中的静态方法**
   2. 这意味着你如果在一个实例中更改了这个静态变量的值，在其他所有的实例中该值也会被同时修改！ 
   3. 它们实际上并不属于他们所属的类 ，它们就像我们在名为`entity`的命名空间内创建了这两个变量！
   4. 它和任何创建新类实例或类似内容时的任何分配都没有关系！

   5.  这个非常有用当你想要跨类拥有变量时， **如果你想要一条数据，在所有实体实例之间共享它**
   6. 静态类中的静态方法只能访问静态变量： **因为静态方法没有实例**

3. 在实际写代码中，可以使用`s_Vareable`来约定它是静态变量！ 
4. 这有点像在类中将变量声明定义为私有，其他翻译单元都不会看到这个`s_Variable`，链接器在全局范围内看不到它，

## extern关键字

代表这个变量将在外部的翻译单元寻找这个变量的实际定义， **即它不在自己的这个翻译单元找寻具体这个变量的实际定义！**