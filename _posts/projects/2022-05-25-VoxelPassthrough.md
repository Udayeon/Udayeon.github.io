---
layout: post
title: 
description: |
  ros튜토리얼 - Voxel filter, Passthrough filter
hide_image: true
tags:
  - projects
published: true
---

# (Ros) Point Cloud Filtering - Voxel , Passthrough
* * *

# 1. Voxel grid filter
![image](https://user-images.githubusercontent.com/69246778/170190024-9935ab51-95a7-4286-96ed-8d5f977fabe2.png)
point 5개가 1개의 Voxel로 표현된다. 그리고 Voxel내 점들의 중심점 하나만 남기고 나머지 점은 제거한다. 즉, 데이터의 크기가 1/5로 줄어든 것. 
voxel의 크기(=leaf size)를 크게 잡으면 더 많은 점을 하나의 데이터로 축소할 수 있다. 대신 물체 표현력도 낮아진다. 
따라서, 물체표현력과 데이터 크기의 트레이드 오프를 잘 고려하여 적절한 voxel size 결정하는 것이 관건이다.

## 1.1. Code
```c++
#include <ros/ros.h>
#include <pcl/point_types.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/filters/voxel_grid.h>

ros::Publisher pub;

void callbackFcn(const sensor_msgs::PointCloud2::ConstPtr& msg)
{
pcl::PointCloud<pcl::PointXYZ> inputCloud;
pcl::PointCloud<pcl::PointXYZ> filteredCloud;

// Convert sensor_msgs::PointCloud2 to pcl::PointCloud
pcl::fromROSMsg(*msg, inputCloud);
pcl::VoxelGrid<pcl::PointXYZ> vox;   //voxel grid필터
vox.setInputCloud (inputCloud.makeShared());
vox.setLeafSize (1.0f, 1.0f, 1.0f); // voxel의 크기를 한 변이 1m인 정육면체로 설정
vox.filter (filteredCloud);
sensor_msgs::PointCloud2 output;
pcl::toROSMsg(filteredCloud, output);
output.header.frame_id = "/map";
pub.publish(output);
}

int main(int argc, char** argv) {
ros::init(argc, argv, "voxel");
ros::NodeHandle nh;

// << Subscribe Topic >> //
//topic name : /vLidarPC
// topic type : sensor_msgs::PointCloud2
ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2>("/vLidarPC", 1, callbackFcn);

// << Publish Topic >>
// topic name : /voxelPC
// topic type : sensor_msgs::PointCloud2

pub = nh.advertise<sensor_msgs::PointCloud2>("/voxelPC", 1);
ros::spin();
return 0; }
```

## 1.2. Ros tutorial
point cloud raw data   
![image](https://user-images.githubusercontent.com/69246778/170190751-bfaac365-feb9-44e5-bf0c-977c491fb9a5.png)

### 1.2.a. 
```c++
vox.setLeafSize (1.0f, 1.0f, 1.0f); 
```
![image](https://user-images.githubusercontent.com/69246778/170192272-917822f5-bba6-4e27-93fb-db5afedb6d62.png)

### 1.2.b. 
```c++
vox.setLeafSize (0.5f, 0.5f, 0.5f); 
```
![image](https://user-images.githubusercontent.com/69246778/170191967-898ba2b6-3fd9-48a0-80a6-a6cfea01095a.png)

### 1.2.c. 
```c++
vox.setLeafSize (1.5f, 1.5f, 1.5f); 
```
![image](https://user-images.githubusercontent.com/69246778/170192142-d231a218-df13-481e-975f-d39a7dd8f0bb.png)



# 2. Passthrough filter
특정한 영역을 기반으로 제약 조건을 통과한 점들만 남긴다. x,y,z의 최소/최대 범위를 지정해 crop하는 방식으로 필터링한다. 직관적이지만 
정교함

## 2.1. Code
```c++
#include <ros/ros.h>
#include <pcl/point_types.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/filters/passthrough.h>
ros::Publisher pub;
void callbackFcn(const sensor_msgs::PointCloud2::ConstPtr& msg)
{
pcl::PointCloud<pcl::PointXYZ> inputCloud;
pcl::PointCloud<pcl::PointXYZ> filteredCloud;
pcl::fromROSMsg(*msg, inputCloud);
pcl::PassThrough<pcl::PointXYZ> pass; //Passthrough 필터
pass.setInputCloud (inputCloud.makeShared());
pass.setFilterFieldName ("x"); // set Axis(x)   //x좌표에 대해 제약 조건을 준다.
pass.setFilterLimits (-2.0, 2.0); // x값이 -2.0m ~ 2.0m 사이에 있는 데이터는 crop한다.
pass.setFilterLimitsNegative (true); // x값이 -inf ~ -2.0 && 2.0 ~ inf 인 데이터(crop안 한 부분)는 살려둔다. 
pass.filter (filteredCloud);
sensor_msgs::PointCloud2 output;
pcl::toROSMsg(filteredCloud, output);
output.header.frame_id = "/map";
pub.publish(output);
}

int main(int argc, char** argv)
{
ros::init(argc, argv, "passthrough");
ros::NodeHandle nh;
// << Subscribe Topic >>
// topic name : /voxelPC
// topic type : sensor_msgs::PointCloud2
ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2>("/voxelPC", 1, callbackFcn);
// << Publish Topic >>
// topic name : /passPC
// topic type : sensor_msgs::PointCloud2
pub = nh.advertise<sensor_msgs::PointCloud2>("/passPC", 1);
ros::spin();
return 0;
}
```

## 2.2. Ros tutorial
point cloud raw data   
![image](https://user-images.githubusercontent.com/69246778/170190751-bfaac365-feb9-44e5-bf0c-977c491fb9a5.png)

### 1.2.a. 
```c++
pass.setFilterFieldName ("x");  
pass.setFilterLimits (-2.0, 2.0);
pass.setFilterLimitsNegative (true);
```
![image](https://user-images.githubusercontent.com/69246778/170193868-6820869e-bd1f-4bc8-b9b6-3f560af5ffe6.png)

### 1.2.b. 
```c++
pass.setFilterFieldName ("x");  
pass.setFilterLimits (-1.0, 1.0);
pass.setFilterLimitsNegative (true);
```
![image](https://user-images.githubusercontent.com/69246778/170193977-79415ed7-3452-4f17-ba8a-cae33af7f2bf.png)


### 1.2.c. 
```c++
pass.setFilterFieldName ("y");  
pass.setFilterLimits (-1.0, 1.0); 
pass.setFilterLimitsNegative (true);  //범위 외 부분만 살리기
```
![image](https://user-images.githubusercontent.com/69246778/170194398-402081b5-35c3-49b2-8e6f-2791ff9e9216.png)

### 1.2.d.
```c++
pass.setFilterFieldName ("y");  
pass.setFilterLimits (-1.0, 1.0); 
pass.setFilterLimitsNegative (false); //범위 외 부분 버리기
```
![image](https://user-images.githubusercontent.com/69246778/170194747-d24d6817-bd81-47ce-8aca-78bb97e01753.png)


### 1.2.e. x,y 둘 다에 대해 필터링하기
x방향필터, y방향필터 **두 개**를 사용한다.
```c++
#include <ros/ros.h>
#include <pcl/point_types.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/filters/passthrough.h>
ros::Publisher pub;
void callbackFcn(const sensor_msgs::PointCloud2::ConstPtr& msg)
{
//사용한 PointCloud 변수들을 미리 모두 선언한다.
pcl::PointCloud<pcl::PointXYZ> inputCloud;
pcl::PointCloud<pcl::PointXYZ> xfilteredCloud; 
pcl::PointCloud<pcl::PointXYZ> xyfilteredCloud;
pcl::fromROSMsg(*msg, inputCloud);

//x방향 filter
pcl::PassThrough<pcl::PointXYZ> xfilter;  //필터이름은 xfilter
xfilter.setInputCloud (inputCloud.makeShared());  //xfilter를 inputCloud에 적용.
xfilter.setFilterFieldName ("x"); // set Axis(x)  //x방향에 대해
xfilter.setFilterLimits (-1.0, 1.0); // -1.0~1.0범위
xfilter.setFilterLimitsNegative (false); // 범위를 제외한 나머지를 버린다.
xfilter.filter (xfilteredCloud);  //x범위가 -1.0~1.0인 data만 남음.

pcl::PassThrough<pcl::PointXYZ> yfilter;  //필터이름 yfilter
yfilter.setInputCloud (xfilteredCloud.makeShared());  //xfilter 적용한 결과물인 xfilteredCloud에 적용
yfilter.setFilterFieldName ("y"); // y방향에 대해  
yfilter.setFilterLimits (-1.0, 1.0); // -1.0~1.0범위
yfilter.setFilterLimitsNegative (false); // 범위를 제외한 나머지를 버린다.
yfilter.filter (xyfilteredCloud); //x,y모두 -1.0~1.0이내의 데이터만 남음.



sensor_msgs::PointCloud2 output;
pcl::toROSMsg(xyfilteredCloud, output);
output.header.frame_id = "/map";
pub.publish(output);
}

int main(int argc, char** argv)
{
ros::init(argc, argv, "passthrough");
ros::NodeHandle nh;
// << Subscribe Topic >>
// topic name : /voxelPC
// topic type : sensor_msgs::PointCloud2
ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2>("/vLidarPC", 1, callbackFcn);
// << Publish Topic >>
// topic name : /passPC
// topic type : sensor_msgs::PointCloud2
pub = nh.advertise<sensor_msgs::PointCloud2>("/passPC", 1);
ros::spin();
return 0;
}
```
![image](https://user-images.githubusercontent.com/69246778/170196437-199c1352-9053-4e14-8d2b-c972e7dba18b.png)


