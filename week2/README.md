# Week 2：WSL、Ubuntu 与 ROS2 环境配置

## 实验内容

本周完成了以下任务：

1. 安装 WSL Ubuntu 22.04
2. 配置 ROS2 Humble 环境
3. 运行 turtlesim 小乌龟节点

## 实验截图

### Ubuntu 安装成功

<img src="img/ubuntu-install.png" alt="Ubuntu 安装" width="600">

### 小乌龟仿真运行

<img src="img/turtlesim.png" alt="小乌龟" width="600">

## 运行命令

\`\`\`bash
# 启动小乌龟节点
ros2 run turtlesim turtlesim_node

# 启动键盘控制
ros2 run turtlesim turtle_teleop_key
\`\`\`

## 遇到的问题

1. **问题**：运行 `ros2` 命令提示 command not found
   **解决**：运行 `source /opt/ros/humble/setup.bash`

## 学习心得

通过本周学习，我掌握了 WSL 的基本使用和 ROS2 的安装配置...

## 返回

[← 返回首页](../)
