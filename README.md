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

