# TensorRt

https://www.cnblogs.com/vh-pg/p/11677137.html

https://www.cnblogs.com/vh-pg/p/11680658.html

## 1 什么是tensorRT

可以把TensorRT看做一个“深度学习框架”，不同于常用的TensorFlow、PyTorch和Caffe等深度学习框架，TensorRT的目的不是如何训练我们的深度学习模型，而是考虑如何将那些使用其他框架训练好的模型进行高效快速的Inference。

**要注意，**TensorRT是NVIDIA配套其相关GPU提供的，并不支持在CPU和其他GPU上使用。

## 类&函数

### ILogger

全局变量，在大多数TensorRT的API中都会作为参数使用；

它用于记录一些日志信息

### IRuntime

通过 createInferRuntime(gLogger) 创建。