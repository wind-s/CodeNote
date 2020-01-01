# go语言环境搭建

1.到业务下载mac版安装包（https://golang.org/dl/）

2.直接运行.pkg安装，默认安装目录是在 /usr/local/go

3.配置环境变量，sudo vi ~/.bash_profile

配置参数：

```
#GO
export GOROOT=/usr/local/go
export GOPATH=~/gowork
export PATH=$GOROOT/bin:$GOPATH:$PATH
```

执行 source ~/.bash_profile，环境配置生效。

4.go version 查看环境是否安装成功

```
go version go1.12.9 darwin/amd64
```

5.idea配置golang

下载golang压缩包

```
https://plugins.jetbrains.com/plugin/5047-go-language-golang-org-support-plugin/versions
```







使用goland进行开发：

安装此版本前，需要连接互联网，不要断网，而且IDE程序没有被防火墙软件屏蔽，如果之前修改过hosts文件，需要删除相应的配置信息，具体破解步骤如下：

\1. 将对应的IDE程序 ( IntelliJ IDEA，AppCode等其它软件) 拖拽到应用程序文件夹完成安装。

\2. 启动对应的IDE程序，安装时选择 “Evaluate for free”，以试用版方式安装。

\3. 软件启动后，打开IDE菜单栏，点击 "Help" -> "Edit Custom VM Options”，如果提示需要创建，点击Yes即可。

\4. 接下来需要将 “jetbrains-agent.jar” 文件的绝对路径地址添加到上一步中窗口末尾处，我以 AppCode 为例，

将“jetbrains-agent.jar” 拷贝到 /Applications/AppCode.app/Contents/bin/内 (你也可以放到其它地方)，然后将

-javaagent:/Applications/CLion.app/Contents/bin/jetbrains-agent.jar

这段代码拷贝到步骤3中的"Edit Custom VM Options”末尾处。

注意此处“jetbrains-agent.jar”文件的绝对路径地址一定要正确，且不能含有中文，否则会导致软件打不开。

\5. 重启IDE程序，点击 "Help" -> "Register”，选择第三项：license server，填入许可证服务器地址：http://jetbrains-license-server，（此处应该会自动填上的）或者点击下方的 "Discover Server" 来自动填充地址。

\6. 不出意外，软件就会破解成功。





