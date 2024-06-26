# autoaim_nexte_2024
西安理工大学NEXT E战队2024赛季自动瞄准视觉识别程序
## Author: 崔显允
本程序基于[rm_vision](https://gitlab.com/rm_vision)项目开发，结合队内实际情况重写了串口模块，补充了弹道解算和装甲板选择模块。
## 1.文件结构
    ├── readme.md               # 本文件
    ├── rm_auto_aim             # 自瞄主要算法
    ├── rm_vision               # 自瞄主launch包
    ├── ros2-hik-camera         # 海康相机ROS2驱动
    ├── vision_attacker         # 包括弹道解算和装甲板选择
    ├── vision_interfaces       # ROS2通信接口
    └── vision_serial_driver    # 串口模块
## 2.软硬件环境
- Ubuntu 22.04 LTS
- ROS2 Humble
- MV-CS016 / MV-CA013 工业相机 (综合性能CS016>CA013)
- USB-CDC 虚拟串口
## 3.参数声明
    /vision_attacker:
        trajectory:
            lag_time                    # 预测延迟时间修正
            air_k                       # 空气阻力系数
            yaw_fix                     # 输出Yaw修正
            pitch_fix                   # 输出Pitch修正
        fire_ctrl:
            car_attack_threshold        # 地面机器人装甲板攻击角度阈值
            outpost_attack_threshold    # 前哨站装甲板攻击角度阈值
            threshold_fix               # 角度阈值修正
## 4.使用方法
创建workspace目录

    mkdir -p ws_autoaim/src && cd ws_autoaim/src
clone本项目

    git clone --recurse-submodules https://github.com/prom-se/autoaim_nexte_2024.git
安装依赖

    cd .. && rosdep install -r --from-paths src --ignore-src --rosdistro $ROS_DISTRO -y
编译

    colcon build --symlink-install
使用launch文件启动

    ros2 launch rm_visioin_bringup nexte_autoaim.launch.py
## 5.部署说明
- 需要根据机器人结构修改[launch_params.yaml](rm_vision/rm_vision_bringup/config/launch_params.yaml)中的平移变换(odom2camera).
- 需要根据相机参数修改[launch_params.yaml](rm_vision/rm_vision_bringup/config/camera_info.yaml)中的相机内参.
- 需要根据实际情况选择或修改[串口协议](vision_serial_driver/include/vision_serial_driver/packet.h).
- 调试时使用rqt中plugns-configuration-Dynamic Reconfigure插件动态调参.
- 主要调整的参数为armor_tracker和vision_attacker中的参数.
## 6.致谢(排名不分先后)
  在本赛季程序的开发过程中,因为个人能力和经验不足,在这里特别感谢rm_vision项目,上海交通大学云汉交龙战队,沈阳航空航天大学TUP战队,四川大学火锅战队以及镖群和视觉救命群的各位大佬的帮助。