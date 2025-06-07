
##### agibot_x1_infer_CN/src/module/sim_module/src/sim_module.cc
文件实现了sim_module.h所定义的各个函数

1. initialize(aimrt::CoreRef core)
该函数负责加载配置文件，初始化订阅者、发布者和执行器，为仿真模块的运行做准备。
首先获取了aimrt框架的核心句柄`core_=core`，然后加载配置文件，再获取关节命令的订阅joint_cmd_sub_，获取 IMU 数据的发布者imu_data_pub_，获取关节状态的发布者joint_state_pub_，最后获取渲染执行器render_executor_，初始化成功。

2. Start()
该函数负责初始化相机、选项和扰动，启动渲染线程，加载模型和数据，初始化 PID 控制器。
初始化相机参数，选项参数，扰动参数为默认值。再启动渲染线程（创建仿真类型sim_，加载模型文件m_与模型数据对象d_），获取关节名称joint_names_，初始化 PID 控制器相关参数,启动成功。
```
  // 初始化 PID 控制器相关参数
  // 目标位置
  target_q_.resize(joint_names_.size());
  // 目标速度
  target_dq_.resize(joint_names_.size());
  // 目标扭矩
  target_tq_.resize(joint_names_.size());
  // 比例系数
  kp_.resize(joint_names_.size());
  // 微分系数
  kd_.resize(joint_names_.size());
  // 电机扭矩
  motor_torque_.resize(joint_names_.size());
```

3. Shutdown()
该函数负责释放控制噪声缓冲区，删除模型数据和模型对象。
```
  // 释放控制噪声缓冲区
  free(ctrl_noise_);
  // 删除模型数据对象
  mj_deleteData(d_);
  // 删除模型对象
  mj_deleteModel(m_);
  // 输出关闭成功的信息
  AIMRT_INFO("Shutdown succeeded.");
```

4. CmdCallback(const std::shared_ptr<const my_ros2_proto::msg::JointCommand> &msg)
关节命令回调函数。该函数在接收到关节命令消息时被调用，负责 写入电机命令，执行仿真步骤，读取传感器数据并发布。

5. ReadSensorData(sensor_msgs::msg::Imu &imu_data, sensor_msgs::msg::JointState &joint_state)
读取传感器数据。该函数从仿真数据中读取 IMU 数据和关节状态数据，并填充到相应的消息对象中。
填充IMU数据的方向，角速度，线加速度，时间戳等信息，设置关节状态消息的关节名称；初始化关节位置、速度、力矩数组，并复制。

6. WriteMotorCmd(my_ros2_proto::msg::JointCommand cmd)
写入电机命令。该函数根据接收到的关节命令消息，计算电机扭矩，并添加控制噪声。

其中PID和IMU是什么：
PID（比例-积分-微分）控制器是一种最广泛应用的反馈控制机制，它根据期望值与实际值之间的误差来计算控制信号，广泛应用于机器人、无人机、工业自动化等领域。PID控制器通过组合三种不同的控制策略来产生控制输出：
1. **比例项(P)**：当前误差的线性函数，提供与误差成正比的即时响应
2. **积分项(I)**：误差的历史累积，用于消除稳态误差（静态偏差）
3. **微分项(D)**：误差变化率的预测，用于减少超调和提高系统稳定性

在机器人开发框架中，IMU数据消息对象是一种标准化的数据结构，用于封装和传输来自IMU传感器的测量数据。这种消息对象通常由机器人中间件（如ROS2）定义，用于系统内部各模块间的通信。我们常提到的就是IMU数据消息对象


mujoco 基本语法，sim2real仿真软件和现实之间的差异，以及如何弥补差异


状态机，控制器（如何激活），

problem：发布什么命令，创建什么发布者与订阅者，

查看topic信息:
```
./build.sh
cd build/
./run_sim.sh
source build/ros2_setup.sh
ros2 topic list #列出话题
ros2 topic echo /imu/data #查看话题
```
设置yaml并解析,并构建一个单独的module，包含src，cfg，include等等文件夹
1.碰撞检测  2.动态规划末端最优路径（避障）  3.基于端到端 4.有视觉检测
两实现个模块，一个碰撞检测动态规划，一个识谱做动作
要实现星尘机器人视觉识别
mjcf文件要有手部的xml文件(主线)

上位机跑算法，也可以进行控制，下位机进行控制

保持控制器状态与规划控制器状态 过渡状态

在运行build.sh文件时发现有连接不上github拉取aimrt的情况，于是运行了以下命令
cnbot@cnrobot-pad:~$ git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
export http_proxy=http://127.0.0.1:7897
export https_proxy=http://127.0.0.1:7897
cnbot@cnrobot-pad:~$ echo $http_proxy
echo $https_proxy
http://127.0.0.1:7897
http://127.0.0.1:7897
