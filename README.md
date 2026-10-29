知识点
1. Mat：OpenCV图像容器，本质是矩阵，用来存放图片像素数据。
2. imread()：读取本地图像文件，OpenCV默认读取为BGR格式，不是RGB。
3. cvtColor()：色彩空间转换函数。本次实验 `COLOR_BGR2HSV`，把BGR图片转为HSV。
    - H(Hue)：色相，代表颜色种类
    - S(Saturation)：饱和度，颜色鲜艳程度
    - V(Value)：亮度，明暗
4. split()：多通道图像分离，把3通道HSV图像拆成3张单通道灰度图。
5. imshow()：弹出窗口显示图像；`waitKey(0)` 等待按键，任意按键关闭窗口。
 应用场景：RoboMaster颜色识别，在HSV空间做阈值筛选，识别装甲板颜色

踩坑记录
1. 图片路径问题：代码写`test.jpg`，必须把图片放在程序同目录，否则`imread`读取失败，`src.empty()`为真。
2. WSL环境下`imshow`无法弹出图像窗口，会报错，需要改用`imwrite`保存图片。
3. Ubuntu编译时，需要带上`pkg-config`链接OpenCV库，直接g++不加库参数会报头文件/函数未定义。
4. Git推送GitHub网络不稳定，push经常超时，可以直接网页端新建文件提交代码。

C++代码片段
#include<opencv2/opencv.hpp>
using namespace cv;
int main()
{
    Mat src = imread("test.jpg");
    if(src.empty()){
        printf("Cannot load image\n");
        return -1;
    }
    Mat hsv;
    cvtColor(src,hsv,COLOR_BGR2HSV);
    vector<Mat> channels;
    split(hsv,channels);
    imshow("H channel",channels[0]);
    imshow("S channel",channels[1]);
    imshow("V channel",channels[2]);
    waitKey(0);
    return 0;
}

Cmake
1.
图片后缀假：webp图片手动改名为jpg。只是改名字，文件编码没变，OpenCV 无法读取，cvtColor直接崩溃。
解决方法：图片另存为，导出真正的JPEG/jpg 格式。
2.
  Linux文件名／路径中文：Ubuntu终端对中文文件夹、中文文件名兼容性差，经常提示找不到文件。
解决办法：文件、文件夹全部使用英文命名。
3.
相对路径问题：程序在build文件夹运行，.…／代表上一级cmake_demo目录，路径写错图片读不到。
解决方法：
  图片放到cmake_demo，代码写．./xxx.jpeg
  使用完整绝对路径
4.
Linux严格区分大小写：test.Jpg 和
test.jpg 是两个不同文件；jpeg和．jpg 不能混用。
解决方法：复制文件名原样写到代码里。
5.
忘记判断图片是否读取成功图片读取为空，直接调用cvtColor，程序直接抛出异常、核心转储。
解决方法： 增加if(src.empty()）判断，打印失败提醒
  
