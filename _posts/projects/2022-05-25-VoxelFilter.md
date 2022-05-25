---
layout: post
title: 
description: |
  voxel grid filter
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
## 1.1. Ros tutorial
아래와 같이 촘촘히 찍힌 point cloud에 voxel 필터를 적용하면   
![image](https://user-images.githubusercontent.com/69246778/170190751-bfaac365-feb9-44e5-bf0c-977c491fb9a5.png)
   
다음과 같이 **down sampling**된다.
![image](https://user-images.githubusercontent.com/69246778/170190852-76c21d2f-7c69-4e3d-8853-dd9a4258905a.png)다

## 1.2. Code
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


# 2. Passthrough filter
