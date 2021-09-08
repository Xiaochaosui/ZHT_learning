# Vscode_Intro

## 1 使用过程中遇到的问题

### 1.1 新建文件夹之后的cpp文件运行时，launch配置不对

因为launch配置的运行文件是，根目录下的运行文件，/root/新建文件夹/filename.cpp

**solution：** 未找到，大概应该是改变launch的配置

## 2 新建cpp文件 

### 2.1 设置自己的cpp模板

1. 选择菜单里的 文件 > 首选项 > 用户代码片段
2. 选择cpp 进入json文件
3. 修改里面的json代码为如下

```json
{
"Print to conaole":
    "prefix": "C++",    //在新建立的页面中输入C++就会有智能提示，Tab就自动生成好了
    "body": [
        "#include <iostream>",
        "#include <cmath>",     //这个头文件可以删除，我为了使用方便就加了
        "", //空行
        "using namespace std;", //标准命名空间
        "",
        "int main()",   //main()函数
        "{",
        "   $0",    //最终光标会在这里等待输入
        "   system(\"pause\");",    //标准C++的等待用户动作
        "   return 0;", //结束
        "}",
        "",
    ],
    "description": "A cpp file template."   //用户输入后智能提示的内容（你可以用中文写“生成C++模板”）
}
}
```

![img](https://pic4.zhimg.com/80/v2-16bcfda27023311237e2574b227edb5b_1440w.jpg)

### 2.2 设置exe文件的生成路径

在task.json 文件下修改参数args

### 2.3 VsCode预定义全局变量使用

**VsCode预定义全局变量使用**
在VsCode的launch.json和tasks.json中我们常用到一些全局变量，同时为了修改配置文件方便，还想自定义一些全局变量，这里做一下介绍。

**预定义全局变量**
${workspaceFolder} :表示当前workspace文件夹路径

${workspaceRoot} :同上表示当前workspace文件夹路径

${cwd} :切换workspace文件夹路径

${workspaceRootFolderName}:表示workspace的文件夹名

${workspaceFolderBasename}:同上表示workspace的文件夹名

${lineNumber}:当前活动文件的光标行号

${selectedText}:当前活动文件的选中文本

${file}:当前活动文件的绝对路径

${fileWorkspaceFolder}:当前活动文件工作空间绝对路径

${relativeFile}:当前活动文件的相对workpace的相对路径

${relativeFileDirname}:当前活动文件的目录相对workpace的相对路径

${fileDirname}:当前活动文件的目录绝对路径

${fileExtname}:当前活动文件的后缀名

${fileBasename}:当前活动文件的文件名

${fileBasenameNoExtension}:当前活动文件的文件名，不带后缀

${fileDirnameBasename}:当前活动文件目录名

${execPath}:vscode的执行文件路径

${execInstallFolder}:当前工作空间的目录

${pathSeparator}:系统文件分割符

${env:PATH}:系统中的环境变量

**自定义全局变量**
官方没有提供具体的文档说明，通过调试vscode的js文件，发现负责计算变量的evaluateSingleVariable函数有个config的分支，可以从setting中提取变量，于是可以通过在setting中注入变量。

### 2.4 设置自动换行

### 2.5 过滤资源管理显示文件

![img](https://img-blog.csdnimg.cn/20200330002032654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjA3NDgzMg==,size_16,color_FFFFFF,t_70)

在这里添加你要过滤的文件格式，用正则的格式写



## 3 注释

ctrl+shift+c：注释掉选中的多行代码

ctrl+shift+x：对已经注释的多行代码取消注释

