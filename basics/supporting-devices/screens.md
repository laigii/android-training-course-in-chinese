# 适配不同的屏幕

> 编写:[Lin-H](http://github.com/Lin-H) - 原文:<http://developer.android.com/training/basics/supporting-devices/screens.html>

Android将设备屏幕归类为两种常规属性：尺寸和分辨率。你应该想到你的app会被安装在各种屏幕尺寸和分辨率的设备中。这样，你的app就应该包含一些可选资源，针对不同的屏幕尺寸和分辨率，来优化你的app外观。

- 有4种普遍尺寸：小(small)，普通(normal)，大(large)，超大(xlarge)
- 4种普遍分辨率：低精度(ldpi), 中精度(mdpi), 高精度(hdpi), 超高精度(xhdpi)，自适应精度（nodpi）

声明针对不同屏幕所用的layout和bitmap，你必须把这些可选资源放置在独立的目录中，与你适配不同语言时的做法类似。

同样要注意屏幕的方向(横向或纵向)也是一种需要考虑的屏幕尺寸变化，所以许多app会修改layout，来针对不同的屏幕方向优化用户体验。

## 创建不同的layout

为了针对不同的屏幕去优化用户体验，你需要对每一种将要支持的屏幕尺寸，创建唯一的XML文件。每一种layout需要保存在相应的资源目录中，目录以`-<screen_size>`为后缀命名。例如，对大尺寸屏幕(large screens)，一个唯一的layout文件应该保存在`res/layout-large/`中。

> **Note**:为了匹配合适的屏幕尺寸Android会自动地测量你的layout文件。所以你不需要因不同的屏幕尺寸去担心UI元素的大小，而应该专注于layout结构对用户体验的影响。(比如关键视图相对于同级视图的尺寸或位置)

例如，这个工程包含一个默认layout和一个适配大屏幕的layout：

```
MyProject/
    res/
        layout/
            main.xml
        layout-large/
            main.xml
```

layout文件的名字必须完全一样，为了对相应的屏幕尺寸提供最优的UI，文件的内容不同。

按照惯例在你的app中简单引用：

```java
@Override
 protected void onCreate(Bundle savedInstanceState) {
     super.onCreate(savedInstanceState);
     setContentView(R.layout.main);
}
```

系统会根据你的app所运行的设备屏幕尺寸，在与之对应的layout目录中加载layout。更多关于Android如何选择恰当资源的信息，详见[Providing Resources](https://developer.android.com/guide/topics/resources/providing-resources.html#BestMatch)。

另一个例子，这一个工程中有为适配横向屏幕的layout:

```
MyProject/
    res/
        layout/
            main.xml
        layout-land/
            main.xml
```

默认的，`layout/main.xml`文件用作竖屏的layout。

如果你想给横屏提供一个特殊的layout，也适配于大屏幕，那么你需要使用`large`和`land`修饰符。

```
 MyProject/
    res/
        layout/              # default (portrait)
            main.xml
        layout-land/         # landscape
            main.xml
        layout-large/        # large (portrait)
            main.xml
        layout-large-land/   # large landscape
            main.xml
```

> **Note**:Android 3.2和以上版本支持定义屏幕尺寸的高级方法，它允许你根据屏幕最小长度和宽度，为各种屏幕尺寸指定与密度无关的layout资源。这节课程不会涉及这一新技术，更多信息详见[Designing for Multiple Screens](../../ui/multiscreen/index.html)。

## 创建不同的bitmap

你应该为4种普遍分辨率:低，中，高，超高精度，都提供相适配的bitmap资源。这能帮助你在所有屏幕分辨率中都能有良好的画质和效果。

要生成这些图像，你应该从原始的矢量图像资源着手，然后根据下列尺寸比例，生成各种密度下的图像。

- xhdpi: 2.0
- hdpi:  1.5
- mdpi:  1.0 (基准)
- ldpi:  0.75

这意味着，如果你针对xhdpi的设备生成了一张200x200的图像，同样的你应该为hdpi生成150x150,为mdpi生成100x100, 和为ldpi生成75x75的图片资源。

然后，将这些文件放入相应的drawable资源目录中:

```
MyProject/
    res/
        drawable-xhdpi/
            awesomeimage.png
        drawable-hdpi/
            awesomeimage.png
        drawable-mdpi/
            awesomeimage.png
        drawable-ldpi/
            awesomeimage.png
        drawable-nodpi/
            awesomeimage.png
```
注：有时系统根据屏幕大小拉伸或缩放图片，此时会使图片变形，新建的drawable-nodpi中不会进行操作。推荐使用png，jpg 格式图片。
任何时候，当你引用`@drawable/awesomeimage`时系统会根据屏幕的分辨率选择恰当的bitmap。

> **Note**:低密度(ldpi)资源是非必要的，当你提供了hdpi的图像，系统会把hdpi的图像按比例缩小一半，去适配ldpi的屏幕。
更多关于为app创建图标assets的信息和指导，详见[Iconography design](https://developer.android.com/design/style/iconography.html)。
