scanRegistration
input: 原始的imu数据和原始的lidar数据
output: 
  /velodyne_cloud_2(消除非匀速运动畸变后的所有点云的点)
  /laser_cloud_sharp(曲率最大的点云点，每个scan,即每条线被６等分，挑选曲率最大的前2个点云点作为曲率最大的点云点，也就是提取到的线特征？)
  /laser_cloud_less_sharp(曲率较大的点云点，每个scan,即每条线被６等分，挑选曲率较的前20个点云点作为曲率较大的点云点)
  /laser_cloud_flat（曲率最大的点云点，每个scan,即每条线被６等分，挑选曲率最小的前４个点云点作为曲率最小的点云点，也就是提取到的面特征？）
  /laser_cloud_less_flat（曲率较小的点云点，每个scan,即每条线被６等分，除了以上所有剩余的点云点）
  /imu_trans（起始点欧拉角，最后一个点的欧拉角，最后一个点相对于第一个点的畸变位移和速度）

process:
  imu:计算每一个imu msg对应的点的欧拉角,位移和速度,
  为了初始化点云的速度，位移，欧拉角，以及计算在加减速非匀速运动过程中点云产生的位移速度畸变，并对点云中的每个点的位置信息进行补偿矫正，运行补偿？
  lidar:
    特征点提取，线特征，面特征

laserOdometry
input:
/laser_cloud_sharp
output:


线性插值：
已知（x0, y0), (x1, y1), 计算[x0, x1]区间内某一点ｘ对应的ｙ，
y = (x1-x)/(x1-x0)*y0+(x-x0)/(x1-x0)*y1 

点云分线存储的方法：loam vs monitor