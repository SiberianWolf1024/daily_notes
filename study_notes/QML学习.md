工程的目录结构编辑

在.pro文件中配置

头文件：

![9763ffec2b08bec9d8c45dd21fedf6b](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308141553315.png)

源文件：

![33b704ab370ae183abd5775570f1141](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308141553113.png)

设置应用程序窗口的图标

在main.cpp文件中：

```c++
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QtQml>
#include <QIcon>

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);

    // 设置应用程序图标
    app.setWindowIcon(QIcon("path/to/your/icon.png"));

    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/main.qml"));

    return app.exec();
}
```



**Qt项目工程打包问题**

  因为我的Qt项目在cpp文件中调用了一个C程序的代码，所以使用网上通用的Qt命令行windeployqt方法打包后的.exe文件点击根本没反应

  解决方法：

找到release版本的项目构建目录，将里面的.exe文件拷贝

![sccwc](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081446399.png)

![image-20231108144720824](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081447869.png)

找到Qt的安装目录，找到对应的编译器（我这里是msvc2019_64）目录中,将整个msvc2019_64文件夹拷贝出

![image-20231108144919107](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081449154.png)

![image-20231108144941633](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081449677.png)

进入桌面的msvc2019_64的bin目录，将之前拷贝的exe文件粘贴至此，尝试是否能运行，如果不能运行，则需要往bin目录中加入相关依赖的动态库，如果可以成功运行则进入下一步

点击bin目录中的.exe文件运行，并一直保存运行状态不要关闭，选中bin文件夹中除了你的.exe文件之外的所有文件，删除，出现弹出后选择跳过：

![image-20231108145436110](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081454185.png)

剩下的文件都是要用到的，删除完重启.exe文件看看是否还能运行，能运行则保持运行状态继续删除外面的include文件夹，删完再尝试重启.exe文件是否能运行，按照这个方法尝试把能不影响启动运行的文件全部删除，保留剩下的必要的文件，我这里是还剩下这四个文件夹，到此就完成了。

![image-20231108145651671](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081456708.png)



**Qt项目打开串口时打开失败，使用GetLastError函数打印错误信息**

```c++
// 打开串口的代码
    hSerial = CreateFile(glo_configs.com_port, GENERIC_READ | GENERIC_WRITE, 0, NULL, 
        OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL | FILE_FLAG_OVERLAPPED, NULL); // 重叠方式

    //以下都是打印错误信息的代码
    if (hSerial == INVALID_HANDLE_VALUE) {
        DWORD dwError = GetLastError();
        LPWSTR lpErrorMsg;

        // 获取错误信息字符串
        FormatMessageW(FORMAT_MESSAGE_ALLOCATE_BUFFER | FORMAT_MESSAGE_FROM_SYSTEM,
                       NULL,
                       dwError,
                       0,
                       (LPWSTR)&lpErrorMsg,
                       0,
                       NULL);

        if (lpErrorMsg) {
            int bufferSize = WideCharToMultiByte(936, 0, lpErrorMsg, -1, NULL, 0, NULL, NULL);
            LPSTR lpErrorMsgGb2312 = (LPSTR)malloc(bufferSize * sizeof(char));
            WideCharToMultiByte(936, 0, lpErrorMsg, -1, lpErrorMsgGb2312, bufferSize, NULL, NULL);//Gb2312格式的信息

            printf("Failed to open serial port %s. Error code: %d\n", "COM1", dwError);
            printf("Error message: %s\n", lpErrorMsgGb2312);

            free(lpErrorMsgGb2312);
            LocalFree(lpErrorMsg);
        } else {
            printf("Failed to open serial port %s. Error code: %d\n", "COM1", dwError);
        }

        return 1;
    }

```

**移植C程序代码到Qt中时，打开串口的CreateFile函数使用的字符集会有差异**

在 Windows API 中，有两种常见的版本用于字符集的区分：ANSI（多字节字符集）和 Unicode。`CreateDirectoryA` 用于多字节字符集，而 `CreateDirectoryW` 用于 Unicode 字符集

C程序中默认引用

```c++
#define CreateDirectory  CreateDirectoryA
```

Qt中默认引用

```c++
#define CreateDirectory  CreateDirectoryW
```

想要在Qt中改为使用`CreateDirectoryW` 的话，需要把CreateFile()改成CreateFileA()函数

![image-20231108150029370](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202311081500398.png)









### QML_Item

```c++
//填充
anchors.fill: parent
```

![image-20230811093259317](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308110933952.png)

```c++
//左边距
anchors.left: rect1.rigtht
//左边距空白距离
anchors.leftMargin: 20
```

![image-20230811095604834](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308110956916.png)

```c++
//居中于父控件
anchors.centerIn:parent
//垂直居中 
anchors.verticalCenter:parent.verticalCenter    
//水平居中
anchors.horizontalCenter: parent.horizontalCenter  
//旋转
rotation    
//比例 
sacle 
```



### 实现自定义边框Rectangle

 调整内部矩形的边缘距离

![image-20230812163302660](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308121633056.png)

### Button

按压变换底色

![image-20230814084433995](https://cdn.jsdelivr.net/gh/Siberianwolf926/typora_images@master/img/202308140844152.png)

### 环形百分比进度条

```c++
import QtQuick 2.15
import QtQuick.Controls 2.12
import QtQuick.Controls.Material 2.12
import QtGraphicalEffects 1.15
 
ConicalGradient {
    id:circle
    height: 200
    width: 200
 
    property color barClr: "deepskyblue"
    property int barWidth: 20
    property color bgClr: "gray"
    property color textClr: "deepskyblue"
    property real max: 100
    property real min: 0
    property real value: 50
    property real percent: value/(max-min)
    property int fontSize: 18
    property string operation: "进度"
 
    antialiasing: true
    smooth: true
    source: Rectangle{
        height: circle.height
        width: circle.width
        color: "transparent"
        border.color: circle.bgClr
        border.width: circle.barWidth
        radius: width/2
    }
    gradient: Gradient{
        GradientStop { position: 0.0; color: circle.barClr }
        GradientStop { position: circle.percent; color: circle.barClr }
        GradientStop { position: circle.percent + 0.00001; color: circle.bgClr }
        GradientStop { position: 1.00001; color: circle.bgClr }
    }
    Text{
        anchors.centerIn: parent
        font.family: "微软雅黑"
        font.pointSize: fontSize
        color: circle.textClr
        text: circle.operation+"\n"+(circle.percent*100).toFixed()+" %"
        horizontalAlignment: Text.AlignHCenter
        verticalAlignment: Text.AlignVCenter
    }
}
```

