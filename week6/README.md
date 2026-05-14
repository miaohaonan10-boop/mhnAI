# Week 6：ROS2 小乌龟仿真实验
## 实验内容
本周完成了以下任务：
1.  启动 ROS2 小乌龟仿真节点
2.  验证仿真环境的正常运行
3.  确认小乌龟初始状态与终端日志输出

---

## 实验截图
### 小乌龟仿真启动界面
![小乌龟仿真运行](img/turtlesim-week6.png)

---

## 运行命令
```bash
# 启动小乌龟仿真节点
ros2 run turtlesim turtlesim_node
```

---

## 关键日志说明
终端输出关键信息解读：
- `[INFO] [turtlesim]: Starting turtlesim with node name /turtlesim`：表示仿真节点成功启动
- `[INFO] [turtlesim]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]`：表示小乌龟在画布中心（默认坐标）成功生成，初始朝向为0度

---

## 遇到的问题
1.  **问题**：运行 `ros2 run turtlesim turtlesim_node` 提示命令未找到
    **解决**：执行 `source /opt/ros/humble/setup.bash` 加载 ROS2 环境变量，确保终端能识别 ROS2 命令。

2.  **问题**：仿真窗口无法弹出
    **解决**：确认 WSL/本地系统的图形界面配置正常（如 WSL2 需安装 VcXsrv 并设置 DISPLAY 环境变量），重启终端后再次运行命令。

---

## 学习心得
通过本周实验，我成功启动了 ROS2 基础仿真环境，熟悉了 ROS2 节点的启动方式与终端日志解读，理解了 turtlesim 作为 ROS2 入门工具的基本使用方法，为后续的话题通信、控制指令等进阶学习打下了基础。

---

## 返回
[← 返回首页](../)
