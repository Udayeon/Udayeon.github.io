---
layout: post
title: 
description: |
  ros튜토리얼 - kdtree , clustering
hide_image: true
tags:
  - projects
published: true
---

# (Ros) Point Cloud Filtering - kdtree , clustering
* * *

# 1. kdtree
![image](https://user-images.githubusercontent.com/69246778/170198765-297842ed-64d0-4fa3-b99d-f1ae48f633eb.png)
![image](https://user-images.githubusercontent.com/69246778/170199064-9f947efa-7f20-4454-9d64-693b592b7e2d.png)
K차원으로 공간상의 점들을 효율적 접근이 가능하게 정리하는 자료구조.   
   
Point cloud에서는 두 가지 방법이 있다.
* 초기에 점의 갯수를 설정하고 그 갯수가 만족될 때까지 영역을 탐색하는 방법.
* 탐색 시작점을 기준으로 특정 반경 내에 있는 모든 포인트를 탐색하는 방법.
원하는 영역만 segmentation할 수 있다. 

 
## 1.1. Code
```c++
#include <ros/ros.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/point_types.h>
#include <pcl/kdtree/kdtree_flann.h>
#include <vector>

ros::Publisher pub;
void callbackFcn(const sensor_msgs::PointCloud2::ConstPtr& msg)
{
pcl::PointCloud<pcl::PointXYZ> inputCloud;
pcl::PointCloud<pcl::PointXYZ> filteredCloud;
pcl::fromROSMsg(*msg, inputCloud);
pcl::KdTreeFLANN<pcl::PointXYZ> kdtree;   //kdtree 사용
kdtree.setInputCloud (inputCloud.makeShared());
pcl::PointXYZ searchPoint; // 탐색 시작 위치 설정 : (0,0,0)에서 시작
searchPoint.x = 0; 
searchPoint.y = 0;
searchPoint.z = 0;

std::vector<int> pointIdxRadiusSearch;
std::vector<float> pointRadiusSquaredDistance;
float radius = 3;// (0,0,0)에서 반경 3m이내의 점들을 탐색.

if(kdtree.radiusSearch(searchPoint, radius, pointIdxRadiusSearch, pointRadiusSquaredDistance) > 0)
for(int i = 0; i < pointIdxRadiusSearch.size (); ++i)
filteredCloud.points.push_back(inputCloud.points[pointIdxRadiusSearch[i]]);
sensor_msgs::PointCloud2 output;
pcl::toROSMsg(filteredCloud, output);
output.header.frame_id = "/map";  //3m이내에 있는 점들 출력
pub.publish(output);
}

int main(int argc, char** argv)
{
ros::init(argc, argv, "kdtree");
ros::NodeHandle nh;
// << Subscribe Topic >>
// topic name : /vLidarPC
// topic type : sensor_msgs::PointCloud2
ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2>("/vLidarPC", 1, callbackFcn);
// << Publish Topic >>
// topic name : /kdtreePC
// topic type : sensor_msgs::PointCloud2
pub = nh.advertise<sensor_msgs::PointCloud2>("/kdtreePC", 1);
ros::spin();
return 0;
}
```

## 1.2. Ros tutorial

### 1.2.a. 
```c++
pcl::PointXYZ searchPoint; // set Search Point : (0, 0, 0)
searchPoint.x = 0;
searchPoint.y = 0;
searchPoint.z = 0;
float radius = 3;
```
![image](https://user-images.githubusercontent.com/69246778/170202788-53e1977f-860c-458e-a80b-6d40eb3d4bcb.png)


### 1.2.b.
```c++
pcl::PointXYZ searchPoint; // set Search Point : (0, 0, 0)
searchPoint.x = 0;
searchPoint.y = 0;
searchPoint.z = 0;
float radius = 2;
```
![image](https://user-images.githubusercontent.com/69246778/170205073-7365ccd2-81bd-43d7-b3f4-29223b805e0f.png)
반경이 짜끄매짐

# 2. Clustering
유클리드 거리 기반 클러스터링을 다룬다. 

## 2.1. Code
```c++
#include <ros/ros.h>
#include <pcl/point_types.h>
#include <pcl_conversions/pcl_conversions.h>
#include <pcl/kdtree/kdtree.h>
#include <pcl/segmentation/extract_clusters.h>
#include <vector>
ros::Publisher pub1;
ros::Publisher pub2;
void callbackFcn(const sensor_msgs::PointCloud2::ConstPtr& msg)
{
pcl::PointCloud<pcl::PointXYZ> inputCloud;
pcl::fromROSMsg(*msg, inputCloud);
pcl::search::KdTree<pcl::PointXYZ>::Ptr kdtree (new pcl::search::KdTree<pcl::PointXYZ>);
kdtree->setInputCloud (inputCloud.makeShared());


std::vector<pcl::PointIndices> clusterIndices;
pcl::EuclideanClusterExtraction<pcl::PointXYZ> ec;
ec.setClusterTolerance (1.5); // set distance threshold = 1.5m //이 간격 내에 있는 애들끼리 하나의 cluster
ec.setMinClusterSize (10); // set Minimum Cluster Size //한 cluster당 최소 점의 갯수
ec.setMaxClusterSize (25000); // set Maximum Cluster Size //한 clustere당 최대 점의 갯수
ec.setSearchMethod (kdtree);
ec.setInputCloud (inputCloud.makeShared());
ec.extract (clusterIndices);


int clusterN = 1;
std::vector<pcl::PointIndices>::const_iterator it;
for (it = clusterIndices.begin (); it != clusterIndices.end (); ++it)
{
pcl::PointCloud<pcl::PointXYZ> filteredCloud;
for (std::vector<int>::const_iterator pit = it->indices.begin (); pit != it->indices.end (); ++pit)
filteredCloud.push_back (inputCloud[*pit]);
sensor_msgs::PointCloud2 output;
pcl::toROSMsg(filteredCloud, output);
output.header.frame_id = "/map";
clusterN == 1 ? pub1.publish(output) : pub2.publish(output); //clusterN==1이 참일 때 pub1.publish(output)반환, 아니면 pub2.publish(output)반환
clusterN++;
}
}



int main(int argc, char** argv)
{
ros::init(argc, argv, "clustering");
ros::NodeHandle nh;b
// << Subscribe Topic >>
// topic name : /passPC
// topic type : sensor_msgs::PointCloud2
ros::Subscriber sub = nh.subscribe<sensor_msgs::PointCloud2>("/passPC", 1, callbackFcn);
// << Publish Topic >>
// topic name : /cluster1PC
// topic type : sensor_msgs::PointCloud2
pub1 = nh.advertise<sensor_msgs::PointCloud2>("/cluster1PC", 1);
// << Publish Topic >>
// topic name : /cluster2PC
// topic type : sensor_msgs::PointCloud2
pub2 = nh.advertise<sensor_msgs::PointCloud2>("/cluster2PC", 1);
ros::spin();
return 0;
}
```

## 1.2. Ros tutorial
passthrough를 적용한 data
![image](https://user-images.githubusercontent.com/69246778/170207876-99e6e092-ec2c-4cd1-8186-5cf802b8d2b1.png)

### 1.2.a. 
```c++
ec.setClusterTolerance (1.5); // set distance threshold = 1.5m 
ec.setMinClusterSize (10); // set Minimum Cluster Size 
ec.setMaxClusterSize (25000);
```
**1 Cluster**   
![image](https://user-images.githubusercontent.com/69246778/170208342-f909272d-b258-4282-a60a-fb3a982a8905.png)
      
**2 Cluster**   
![image](https://user-images.githubusercontent.com/69246778/170208404-c62eef7b-e01a-408f-90e4-e54182f10d34.png)

### 1.2.b.
아래와 같은 raw data이용.
![image](https://user-images.githubusercontent.com/69246778/170214759-89fb1549-5150-4fa9-a17f-523ade313c88.png)
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
xfilter.setFilterLimits (-3.0, 4.0); // -3.0~4.0범위
xfilter.setFilterLimitsNegative (true); // 범위를 제외한 나머지를 살린다
xfilter.filter (xfilteredCloud);  //x범위가 -3.0~4.0인 data만 남음.
pcl::PassThrough<pcl::PointXYZ> yfilter;  //필터이름 yfilter
yfilter.setInputCloud (xfilteredCloud.makeShared());  //xfilter 적용한 결과물인 xfilteredCloud에 적용
yfilter.setFilterFieldName ("y"); // y방향에 대해  
yfilter.setFilterLimits (-2.0, 3.0); // -2.0~3.0범위
yfilter.setFilterLimitsNegative (true); // 범위를 제외한 나머지를 살린다.
yfilter.filter (xyfilteredCloud); 

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
