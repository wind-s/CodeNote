Android环境搭建

1.先把java环境搭建好

2.sudo vi ~/.bash_profile 环境配置变量的信息在此。

3.下载android studio https://developer.android.com/studio 

4.sudo vi ~/.bash_profile 添加参数

```
#android
SDK_HOME=/Users/wushaofeng/Library/Android/sdk
export SDK_HOME
export PATH=$PATH:$SDK_HOME
```



5.安装 gradle环境

```
1. brew install gradle
2.检查版本 gradle -version
3.配置环境变量
#GRADLE
GRADLE_HOME=/usr/local/bin/gradle
export GRADLE_HOME
export PATH=$PATH:$GRADLE_HOME/bin
```

6.设置文件夹为source roots

```
一、Source roots (or source folders)

通过这个类指定一个文件夹，你告诉IntelliJ IDEA，这个文件夹及其子文件夹中包含的源代码，可以编译为构建过程的一部分。

2. Test source roots (or test source folders; shown as rootTest)

这些根类似于源根，但用于用于测试的代码（例如用于单元测试）。测试源文件夹允许您将与测试相关的代码与生产代码分开。

通常，源和测试源的编译结果被放置在不同的文件夹中。

3. Resource roots

用于应用程序中的资源文件（图像、各种配置XML和属性文件等）。

在构建过程中，资源文件夹的所有内容都复制到输出文件夹中，如下所示。

类似于源，您可以指定生成资源。您还可以指定输出文件夹中的文件夹，您的资源应该复制到。

4. Test resource roots

（或测试资源文件夹；如roottestresourceij；只有在java模块）是资源文件与您的测试源有关。在所有其他方面，这些文件夹类似于资源文件夹
```




