# AGX 引发的learning

## ioctl()

Linux 下的函数

ioctl 是设备驱动程序中设备控制接口函数，一个字符设备驱动通常会实现设备打开、关闭、读、写等功能，在一些需要细分的情境下，如果需要扩展新的功能，通常以增设 ioctl() 命令的方式实现。

<img src="https://gitee.com/kevin1993175/image_resource/raw/master/ioctl1.png" alt="image" style="zoom:50%;" />

`int ioctl(int fd, int cmd, ...) ;`

ioctl() 函数执行成功时返回 0，失败则返回 -1 并设置全局变量 errorno 值，

## 外部函数getPluginRegistry（）

**TensorRT**还提供了通过调用REGISTER_TENSORRT_PLUGIN（pluginCreator）来注册插件的功能，该插件将插件创建器静态注册到插件注册表。在运行时，可以使用外部函数getPluginRegistry（）查询插件注册表。插件注册表存储指向所有已注册插件创建者的指针，可用于根据插件名称和版本查找特定的插件创建者。 TensorRT库包含可以加载到您的应用程序中的插件。

**solution：**要把nvinfer.lib导入正确

## addKernel函数(cuda工程)

addKernel函数上来，这个函数会被GPU上的多个线程同时执行一次，线程间彼此没有通信，相互独立。到底会有多少个线程来分别执行核函数，是在“<<<>>>”符号里定义的。“<<<>>>”表示运行时配置符号，在本程序中的定义是<<<1，size>>>，表示分配了一个线程块（Block），每个线程块有分配了size个线程，“<<<>>>”中的 参数并不是传递给设备代码的参数，而是定义主机代码运行时如何启动设备代码。以上定义的这些线程都是一个维度上的，可以通过thredaIdx.x来获取执行当前计算任务的线程的ID号。

## VS导入附加库问题

项目->属性->链接器->输入->附加依赖项.

path要具体到*.lib，如果只写到lib文件夹 会默认你是一个\*.obj文件,会报错你找不到啥啥啥\lib.obj文件**(报错类型为LINK : fatal error LNK1104)**

**solution：**项目->属性->链接器->输入->附加依赖项 修改附加依赖项指向*.lib文件

## LNK2019 无法解析外部函数

1. 查看是否导入.lib文件
2. .lib文件是否编译正确，记得看，编译该.lib的编译类型(.dll  .exe  .lib)  配置哪种类型，源码就包括在哪个类型里，换句话说就是，调用的函数放在哪个类型文件里。

## Qt调试器出现：the selected debugger may be inappropriate for the inferior

Qt在调试的过程中出现上述异常，是因为没有安装Windows debugger或者debugger版本不合适而造成的。

https://blog.csdn.net/lym940928/article/details/90208942

## 查看依赖项dll工具-Depends

depends是一款可以查看一个exe文件或dll文件需要依赖哪些dll文件的工具，比如我们生产了一个exe程序，显然在我们的开发环境下是可以执行这个exe程序的，但是换一个环境还可以执行吗？这就不见得了。所以我们需要知道这个exe程序都依赖哪些动态链接库，以保证程序离开了开发环境还可以正常运行。

## cuda64_x.dll 文件

cuda64_x.dll是由nvinfer.dll文件决定的

## Serialization Error in nvinfer1::rt::CoreReadArchive::verifyHeader: 0

反序列化模型的时候遇到以下提示问题

**原因**：模型是在TensorRT5上面序列化得到的，但是目前项目我切换到了TensorRT7，因此用TensorRT7的代码试图去加载TensorRT5的序列化得到的模型就会出现以上错误

TensorRT engines are not compatible across different TensorRT versions.

**TensorRT引擎在不同的TensorRT版本之间不兼容。**

## nvinfer1::cudnn::Engine::deserialize()

- 一个Engine的建立是根据特定GPU和CUDA版本来的，所以在一个机器上序列化的Engine到另一个机器上不一定能使用，因此在使用Builder生成Engine前，要注意自己的环境配置
