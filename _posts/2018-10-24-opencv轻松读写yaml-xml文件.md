---
layout: post
title: opencv轻松读写yaml/xml文件
key: 20181024
tags: c++
  opencv
  yaml
  xml
---

有时候我们需要把参数写入文档，利用opencv库可以轻松实现读写yaml/xml文件，相对于txt文件，参数可以更加直观。（以下部分代码需要编译器支持C++11）
## 样例1（单个变量）
### 写文件
```cpp
#include <opencv2/opencv.hpp>
using namespace cv;

int main() 
{
	FileStorage fs("test.yaml", FileStorage::WRITE);//换成xml效果一样，yaml文件格式相对更直观
	//FileStorage fs("test.xml", FileStorage::WRITE);
	
	fs << "v" << 1.23;

	fs.release();

	std::cout << "文件读写完毕！" << std::endl;

	return 0;
}
```
### 读文件
```cpp
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main() 
{
	FileStorage fs("test.yaml", FileStorage::READ);//换成xml效果一样，yaml文件格式相对更直观
    //FileStorage fs("test.xml", FileStorage::READ);

    if (!fs.isOpened()) 
    {
        cout << "No file!" << endl;
        return false;
    }

	double v = (double)fs["v"];
    cout << "v: " << v << endl;

	fs.release();

	std::cout << "文件读取完毕！" << std::endl;

	return 0;
}
```

## 样例2（多个变量）
### 写文件
```cpp
#include <opencv2/opencv.hpp>
using namespace cv;

int main() 
{
	FileStorage fs("test.yaml", FileStorage::WRITE);//换成xml效果一样，yaml文件格式相对更直观
	//FileStorage fs("test.xml", FileStorage::WRITE);

	fs << "v" << 1.23;
    
	Mat Matrix1 = (Mat_<double>(3, 3) << 1000, 0, 320, 0, 1000, 240, 0, 0, 1);
	Mat Matrix2 = (Mat_<double>(3, 3) << 100, 0, 32, 0, 100, 24, 0, 0, 0.1);
	std::vector<Mat> Mat_vector;

	Mat_vector.push_back(Matrix1);
	Mat_vector.push_back(Matrix2);

	fs << "Matrix1" << Matrix1;
	fs << "Mat_vector" << "[" << Mat_vector << "]";

	Point3d p1(1.1, 2.2, 3.3);
	Point3d p2(1.11, 2.22, 3.33);
	std::vector<Point3d> point_vector;
	point_vector.push_back(p1);
	point_vector.push_back(p2);

	fs << "p1" << p1;
	fs << "point_vector" << "[" << point_vector << "]";

	fs.release();

	std::cout << "文件读写完毕！" << std::endl;

	return 0;
}
```
### 读文件
```cpp
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main() 
{
	FileStorage fs("test.yaml", FileStorage::READ);//换成xml效果一样，yaml文件格式相对更直观
    //FileStorage fs("test.xml", FileStorage::READ);

    if (!fs.isOpened()) 
    {
        cout << "No file!" << endl;
        return false;
    }

	double v = (double)fs["v"];
    cout << "v: " << v << endl;
	
	Mat Matrix1;
	fs["Matrix1"] >> Matrix1;
	cout <<"Matrix1: " << Matrix1 << endl;

    vector<Mat> Mat_vector;
    fs["Mat_vector"][0] >> Mat_vector;
    cout << "Mat_vector: " << endl;
    for(Mat temp:Mat_vector)
    {
        cout << temp << endl;
    }

    Point3d p1;
    fs["p1"] >> p1;
    cout << "p1: " << p1 <<endl;

    vector<Point3d> point_vector;
    fs["point_vector"][0] >> point_vector;
    cout << "point_vector: " << endl;
    for(Point3d temp:point_vector)
    {
        cout << temp << endl;
    }

	fs.release();

	std::cout << "文件读取完毕！" << std::endl;

	return 0;
}
```