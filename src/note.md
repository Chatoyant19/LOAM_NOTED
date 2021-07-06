scanRegistration
input: 原始的imu数据和原始的lidar数据
output: 
  /velodyne_cloud_2(消除非匀速运动畸变后的所有点云的点)
  /laser_cloud_sharp(曲率最大的点云点，每个scan,即每条线被６等分，挑选曲率最大的前2个点云点作为曲率最大的点云点，也就是提取到的线特征？)
  /laser_cloud_less_sharp(曲率较大的点云点，每个scan,即每条线被６等分，挑选曲率较的前20个点云点作为曲率较大的点云点)
  /laser_cloud_flat（曲率最大的点云点，每个scan,即每条线被６等分，挑选曲率最小的前４个点云点作为曲率最小的点云点，也就是提取到的面特征？）
  /laser_cloud_less_flat（曲率较小的点云点，每个scan,即每条线被６等分，除了以上所有剩余的点云点）
  /imu_trans（一帧点云起始点欧拉角，一帧点云最后一个点的欧拉角，最后一个点相对于第一个点的畸变位移和速度）

process:
  imu:计算每一个imu msg对应的点的欧拉角,位移和速度,
  这一个callback函数比较好理解，就是为了初始化点云的速度，位移，朝向（欧拉角），以及计算在加减速非匀速运动过程中点云产生的位移速度畸变，并对点云中的每个点的位置信息进行补偿矫正（运动补偿）
  为什么需要做运动补偿？因为LOAM是基于匀速运动的假设
  lidar:
    1.点云预处理，首先过滤无效点，然后计算当前帧点云起始点和结束点的旋转角
    2.点云去畸变处理并根据仰角将点云点归类到不同的线束中
      点云去畸变处理的过程还没有看明白，看三个函数
      点云运动补偿是做什么的？是把当前点云坐标系下的点投影到点云起始点（这个起始点是每一帧点云即每一个sweep的起始点）坐标系下
    3.提取点特征，分为平面特征和线特征:根据曲率...

laserOdometry
input:
  /laser_cloud_sharp
  /laser_cloud_flat
output:
  /laser_cloud_corner_last
  /velodyne_cloud_3(消除匀速运动畸变后所有点云的点)
  /laser_odom_to_init

process: 估计帧间的相对运动

laserMapping



线性插值：
已知（x0, y0), (x1, y1), 计算[x0, x1]区间内某一点ｘ对应的ｙ，
y = (x1-x)/(x1-x0)*y0+(x-x0)/(x1-x0)*y1 
