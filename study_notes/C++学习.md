## 函数

### 引用变量

用来在函数参数中引用原变量

```c++
int& r = count;
void swap(int& n1, int& n2){}
```

![heheheh](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308070929490.png)

使用引用传递时，形参和实参的类型必须是相同的

```c++
void increment(double& n){}
int main(){
    double x = 1;
    int y = 1;
    increment(x); //类型相同，引用传递
    increment(y); //类型不同，值传递
    
    return 0;
}
```

1. 使用引用传递的时候，实际参数必须是一个变量
2. 使用值传递的时候，传入的数值可以是一个数值、一个变量或者是一个表达式，甚至是另一个函数的返回值

## 数组

### 一维数组和C字符串

数组声明时，数组大小必须是常量表达式

```c++
const int size = 4;
double List[size]; //必须将size声明为常量
```

如果多个数组的元素类型相同，则可以在同一条语句中声明

```c++
double list1[10], list2[20];
```

数组初始化语句

```c++
double List[4] = {1.9, 3.8, 7.9, 6.9};
```

复制数组，只能在两个数组之间，逐个元素地进行复制,不可以直接list = myList

```c++
for (int i = 0; i < size; i++)
{
    list[i] = myList[i];
}
```



### 多维数组

## 指针及动态内存管理

### 空指针

定义：指针变量指向内存中编号为0的空间

用途：用来初始化指针变量（int* p = NULL;）

空指针指向的内存是不可以访问的

### 野指针

指针变量指向非法的内存空间

```c++
int* p = (int*)0x1100;
```



### 常量指针

```c++
const int* p = &a;
```

`p` 无法修改所指向变量的值，但是指针可以指向其他变量的地址

### 指针常量

```c++
 int* const p = &a
```

`p` 是一个不可变的指针，一旦被初始化为变量 `a` 的地址，它就不能再指向其他地址,但是它所指向的内存地址中的值可以被修改

## 内存分区模型

C++程序执行时，将内存划为4个区域，不同区域存放的数据，**赋予不同的生命周期，方便灵活编程**

程序编译后，生成了exe可执行程序，未执行前分为这两个区：

1. ### 代码区

   存放CPU执行的机器指令，特点：

   - 代码区是共享的,对于频繁被执行的程序，只需要在内存中有一份代码
   - 是只读的，防止程序意外修改它的指令

2. ### 全局区

   存放全局变量和静态变量；

   包含常量区（存放字符串常量和其他常量）

   **该区域的数据在程序结束后由操作系统释放**

3. ### 栈区

   存放函数的参数值和局部变量等

   不要返回局部变量的地址，栈区开辟的数据由编译器自动分配和释放

4. ### 堆区

   由程序员分配和释放

   C++中利用new在堆区开辟内存

new返回的是该数据类型的指针

```c++
int* p = new int(10);
...
delete p;
```

## 引用

基本语法：

```c++
int a = 10;
int &b = a; //引用必须初始化，且一旦初始化之后就不可以更改
```

引用的本质是一个指针常量

```c++
int a = 10;
int &b = a; //自动转换为 int* const b = &a;
b = b + 1; //自动转换为 *b = *b + 1;
```

打印语句使用：

```c++
    int myNumber = 42;
    std::cout << "My number: " << myNumber << std::endl;
    printf("My number: %d\n", myNumber);
    qDebug() << "My number:" << myNumber;
    console.log("My number:", myNumber);
```



## 其他

##### 计算函数的耗时

```c++
#include <iostream>
#include <chrono>

    // 获取当前时间点
    auto start = std::chrono::high_resolution_clock::now();

    // 调用目标函数
    myFunction();

    // 获取结束时间点
    auto end = std::chrono::high_resolution_clock::now();

    // 计算时间差
    std::chrono::duration<double> duration = end - start;

    // 输出执行耗时（秒）
    std::cout << "Function execution time: " << duration.count() << " seconds" << std::endl;
    
    // 或者选择计算输出结果单位为毫秒
    auto duration = std::chrono::duration_cast<std::chrono::milliseconds>(end - start);

    // 输出执行耗时（毫秒）
    std::cout << "Time in milliseconds: " << duration.count() << " milliseconds" << std::endl;

```

##### VS2022在项目中使用动态库

###### 1. **添加包含目录**：

- 右键点击项目，选择 **属性**。

- 在 **C/C++** -> **常规** -> **附加包含目录** 中，添加DLL项目的头文件路径，路径推荐使用相对路径，这样无论项目文件位置如何变化，都可以自动适应，例如：

  ```c++
  // $(SolutionDir)是VS2022的预定义宏，它代表 当前解决方案（.sln 文件）所在的目录
  $(SolutionDir)src\include
  ```

###### 2. **添加库目录**：

- 在 **链接器** -> **常规** -> **附加库目录** 中，添加包含lib文件的路径。例如：

  ```c++
  $(SolutionDir)path\to\myLibrary\lib
  ```

###### 3. **链接动态库**：

- 在 **链接器** -> **输入** -> **附加依赖项** 中，添加 `myLibrary.lib`等（这是静态链接库文件，它是生成 DLL 时编译的静态库），使用分号（`;`）来分隔多个库文件

  ```c++
  myLibrary.lib;redis++.lib
  ```

###### 4. **确保 DLL 在运行时可用**：

- 将 `myLibrary.dll` 文件放置在可执行文件（`.exe`）的同一目录下，这是最直接的方法

- 如果Dll文件很多或者需要经常修改，则可以配置`PATH`的值，操作系统在运行可执行文件时，会按照 `PATH` 环境变量的值查找可执行文件或动态链接库（DLL）

- 在**调试** -> **环境** 中，添加`PATH`的值，使用分号（`;`）来分隔多个路径，注意选择的是Debug模式还是Release模式，例如

  ```c++
  // 如果选择的是Debug模式
  PATH=$(SolutionDir)lib\debug;$(SolutionDir)src\redis++\lib\debug
  
  // 如果选择的是Release模式
  PATH=$(SolutionDir)lib\release;$(SolutionDir)src\redis++\lib\release
  ```

  
