# PX4-ROS2 Setup

This repository is guide for setup PX4 firmware & ROS2 system. 
* reference : https://docs.px4.io/main/en/ros/ros2_comm.html
## 1. For SITL

### Install Environment
>- Ubuntu 20.04.6 LTS

### Install PX4 development environment (Firmware Release version 1.14.0-rc1 / 2023.08.09)
>```bash
>cd
>git clone https://github.com/PX4/PX4-Autopilot.git --recursive
>bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
>cd PX4-Autopilot/
>make px4_sitl
>```

### Install ROS2 Foxy
>- Follow the official installation guide: https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html
>- Some Python dependencies must also be installed (using pip or apt):
>```sh
>pip install --user -U empy pyros-genmsg setuptools
>```

### Setup Micro XRCE-DDS Agent & Client
>- Setup the Agent
>```sh
>git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
>cd Micro-XRCE-DDS-Agent
>mkdir build
>cd build
>cmake ..
>make
>sudo make install
>sudo ldconfig /usr/local/lib/
>```

>- Start the agent
>```sh
>MicroXRCEAgent udp4 -p 8888
>```

### PX4 <-> DDS Link Test
>- Start PX4 Gazebo Simulation
>```sh
>make px4_sitl gazebo-classic
>```

>- If it`s successful, you can see this lines.
>``` sh
>[1691572028.387775] info     | UDPv4AgentLinux.cpp | init                     | running...             | port: 8888
>[1691572028.388083] info     | Root.cpp           | set_verbose_level        | logger setup           | verbose_level: 4
>[1691572028.511461] info     | Root.cpp           | create_client            | create                 | client_key: 0x00000001, session_id: 0x81
>[1691572028.511562] info     | SessionManager.hpp | establish_session        | session established    | client_key: 0x00000001, address: 127.0.0.1:39044
>[1691572028.527568] info     | ProxyClient.cpp    | create_participant       | participant created    | client_key: 0x00000001, participant_id: 0x001(1)
>[1691572028.527920] info     | ProxyClient.cpp    | create_topic             | topic created          | client_key: 0x00000001, topic_id: 0x3E8(2), participant_id: 0x001(1)
>[1691572028.528037] info     | ProxyClient.cpp    | create_subscriber        | subscriber created     | client_key: 0x00000001, subscriber_id: 0x3E8(4), participant_id: 0x001(1)
>[1691572028.530429] info     | ProxyClient.cpp    | create_datareader        | datareader created     | client_key: 0x00000001, datareader_id: 0x3E8(6), subscriber_id: 0x3E8(4)
>...
>```

### ROS2 Sample Workspace
>- Create new workspace
>```sh
>mkdir -p ~/ws_sensor_combined/src/
>cd ~/ws_sensor_combined/src/
>```

>- Clone example repository
>```sh
>git clone https://github.com/PX4/px4_msgs.git
>git clone https://github.com/PX4/px4_ros_com.git
>```

>- Build workspace
>```sh
>cd ..
>source /opt/ros/foxy/setup.bash
>colcon build
>```

>- Source workspace
>```sh
>source install/local_setup.bash
>```

>- Launch sample node
>```sh
>ros2 launch px4_ros_com sensor_combined_listener.launch.py
>```

>- Did you see like this?
>```sh
>RECEIVED SENSOR COMBINED DATA
>=============================
>ts: 1691573166807626
>gyro_rad[0]: -0.00213053
>gyro_rad[1]: -0.00506001
>gyro_rad[2]: -0.00213053
>gyro_integral_dt: 4000
>accelerometer_timestamp_relative: 0
>accelerometer_m_s2[0]: -0.199916
>accelerometer_m_s2[1]: 0.137667
>accelerometer_m_s2[2]: -9.89883
>accelerometer_integral_dt: 4000
>```
>- Success!!

## 2. For FC Hardware (Up to date!)
### Install Environment
>- Flight Controller: Pixhawk 5X (PX4 Firmware Release Version 1.14.0-rc1)
>- Companion PC : Nvidia Jetson Orin Nano
>- Jetpack 5.0.1
>- ROS2 Foxy
>- Link through Ethernet between FC and Companion PC
