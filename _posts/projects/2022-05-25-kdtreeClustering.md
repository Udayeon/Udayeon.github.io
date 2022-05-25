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
output.header.frame_id = "/map";
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
