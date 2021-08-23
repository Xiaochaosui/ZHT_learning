# OpenCV

**opencv 颜色通道构造是：BGR！！！**

## 1 模块

### 1.1 core模块

opencv核心功能

### 1.2 imgproc 模块

图像处理模块

### 1.3 highgui模块

包含读写图像和视频的函数

## 2 类

### 2.1Mat类

使用Mat的好处:

- 不需要手动申请内存
- 在不需要时不需要手动释放内存
- 可以通过类的封装，方便获取到数据的相关信息

**cv::Mat**初识和它的六种创建方法:

- 使用构造函数

- ```C++
  cv::Mat M1(3, 3, CV_8UC4, cv::Scalar(0, 0, 0, 255));
  std::cout << "M1 = " << std::endl << M1 << std::endl;
  ```

- 通过数组初始化矩阵维数

- 通过create函数来初始化

- 通过opencv提供的类matlab的函数创建

- 数据自定义矩阵Mat创建通过clone函数创建不同的Mat

#### 2.1.1 属性

**data：**  
       uchar类型的指针，指向Mat数据矩阵的首地址。可以理解为标示一个房屋的门牌号；

​		data（uchar*）成员就是指向图像数据的第一个字节的

**dims：** 
        Mat矩阵的维度，若Mat是一个二维矩阵，则dims=2，三维则dims=3，大多数情况下处理的都是二维矩阵，是一         个平面上的矩阵。
        可以理解为房屋是一个一层的平房，三维或更多维的则是多层楼房；
**rows：**
        Mat矩阵的行数。可理解为房屋内房间行数；
**cols：** 
        Mat矩阵的列数。可理解为房屋内房间列数；
**size()：**
        首先size是一个结构体，定义了Mat矩阵内数据的分布形式，数值上有关系式：
         image.size().width==image.cols;        image.size().height==image.rows                                                      

​     	可以理解为房屋内房间的整体布局，这其中包括了房间分别在行列上分布的数量信息；

**channels()：**
        Mat矩阵元素拥有的通道数。例如常见的RGB彩色图像，channels==3；而灰度图像只有一个灰度分量信息，             			channels==1。
        可以理解为每个房间内放有多少床位，3通道的放了3张床，单通道的放了1张床；
**depth：** 
        用来度量每一个像素中每一个通道的精度，但它本身与图像的通道数无关！depth数值越大，精度越高。在                 		Opencv中，Mat.depth()得到的是一个0~6的数字，分别代表不同的位数，对应关系如下：
        enum{CV_8U=0,CV_8S=1,CV_16U=2,CV_16S=3,CV_32S=4,CV_32F=5,CV_64F=6}          

 	   其中U是unsigned的意思，S表示signed，也就是有符号和无符号数。
 	
 	 可以理解为房间内每张床可以睡多少人，这个跟房间内有多少床并无关系；

**elemSize：**
        elem是element(元素)的缩写，表示矩阵中每一个元素的数据大小，如果Mat中的数据类型是CV_8UC1，那么             		elemSize==1；如果是CV_8UC3或CV_8SC3，那么elemSize==3；如果是CV_16UC3或者CV_16SC3，那么             		elemSize==6；即elemSize是以8位（一个字节）为一个单位，乘以通道数和8位的整数倍；
        可以理解为整个房间可以睡多少人，这个时候就得累计上房间内所有床位数（通道）和每张床的容纳量了；
		elemSize1：
        elemSize加上一个“1”构成了elemSize1这个属性，1可以认为是元素内1个通道的意思，这样从命名上拆分后		就很容易解释这个属性了：表示Mat矩阵中每一个元素单个通道的数据大小，以字节为一个单位，所以有： 
        eleSize1==elemSize/channels；
**step：**
        可以理解为Mat矩阵中每一行的“步长”，以字节为基本单位，每一行中所有元素的字节总量，是累计了一行中		所有元素、所有通道、所有通道的elemSize1之后的值；

​		step 为图象像素行的实际宽度(width x channels)

​		不一定与width相符
​		比如 图像为 1024 x 768
​		设置了感兴趣区域ROI为 400 x 200
​		那么这个感兴趣区域的图象宽度 为 200
​		要访问这个感兴趣区域的下一行，
​		图像数据指针的步长应该为 1024 而不是 200
​		这里 width 为 200  而 step为 1024

**step1()：** 
       以字节为基本单位，Mat矩阵中每一个像素的大小，累计了所有通道、所有通道的elemSize1之后的值,所以有：
        step1==step/elemSize1；
**type：**
        Mat矩阵的类型，包含有矩阵中元素的类型以及通道数信息，type的命名格式为CV_(位数)+(数据类型)+(通道               		数)，所有取值如下：
![img](https://img-blog.csdn.net/20160823213115424)

这里U（unsigned integer）表示的是无符号整数，S（signed integer）是有符号整数，F（float）是浮点数。
例如：CV_16UC2，表示的是元素类型是一个16位的无符号整数，通道为2.
C1，C2，C3，C4则表示通道是1,2,3,4
type一般是在创建Mat对象时设定，如果要取得Mat的元素类型，则无需使用type，使用上面的depth

## 3 函数

### 3.1 cv::imshow()

显示图像，有两个参数

```C++
cv::imshow("img1",img1);
// 第一个参数 string，表示显示窗口的名字
// 第二个参数 是Mat类,被显示的图像
cv::waitkey();//括号里无参数则 按任意键结束
```

### 3.2 cv::resize()

**函数功能: 缩小或者放大函数至某一个大小**

```C++
void cv::resize(
	Mat src,
    Mat dst,
    Size dsize,
    double fx=0,
    double fy=0,
    int interpolation = INTER_LINEAR
)
    /*	src:输入图像(Mat类)
    	dst：输出图像(Mat类)
    	dsize：输出图像的尺寸，如果传入参数0，则按照以下公式计算
        dsize = Size(round(fx*src.col, fy*src.rows))
        fx：水平方向的缩放因子，如果取默认参数0，则按照以下公式计算
        	(double)dsize.width  / src.cols
        fy：垂直方向的缩放因子，如果取默认参数0，则按照以下公式计算
       		 (double)dsize.height / src.rows 
        interpolation：插值方法，对于缩小图片的情况，cv::resize()函数会默认选择INTER_AREA作为插值方						法，对于放大图片的操作，通常最最好选用cv::INTER_CUBIC（较慢），或者使用快速效果						稍弱的cv::INTER_LINER插值方法
    */
    
```

### 3.3 ptr

用于获得每一行的第一个指针

uchar* data = img.ptr<uchar>(i);

### 3.4 blur

均值模糊，用于降噪

### 3.5 threshold

threshold(需要处理的图像，输出图像，阈值，分配的值，阈值处理模式选择)

去掉噪声，过滤很小或者很大的像素点

常用来做二值化

阈值处理模式选择：

- THRESH_BINARY
- THRESH_BINARY_INV
- THRESH_TRUNC
- THRESH_TOZERO
- THRESH_TOZERO_INV

### 3.6 createTrackbar

创建滚动条

createTrackbar这个函数我们以后会经常用到，它创建一个可以调整数值的轨迹条，并将轨迹条附加到指定的窗口上，使用起来很方便。首先大家要记住，它往往会和一个回调函数配合起来使用

**on_trackbar()**

​	这个函数是在创建滚动条的时候要填的参数，用来做反馈的，也就是说在改变滑动滚条的时候做出相应的响应的函数，比如改变二值化的阈值，图片响应的改变都在这个函数里

## 4 应用场景

### 4.1 查找轮廓

https://blog.csdn.net/qq_38392229/article/details/92988628

