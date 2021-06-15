scanRegistration
input: 原始的imu数据和原始的lidar数据
output: 
process:
  imu:计算每一个imu msg对应的点的欧拉角,位移和速度,
  为了初始化点云的速度，位移，欧拉角，以及计算在加减速非匀速运动过程中点云产生的位移速度畸变，并对点云中的每个点的位置信息进行补偿矫正，运行补偿？
  lidar:


线性插值：
已知（x0, y0), (x1, y1), 计算[x0, x1]区间内某一点ｘ对应的ｙ，
y = (x1-x)/(x1-x0)*y0+(x-x0)/(x1-x0)*y1 

点云分线存储的方法：loam vs monitor