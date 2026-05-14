# Week X：ROS2 KITTI 数据集可视化实验
## 实验内容
本周完成了以下任务：
1.  配置 ROS2 环境下的 KITTI 数据集发布节点
2.  启动 RViz 并添加激光雷达点云、相机图像可视化插件
3.  验证 KITTI 数据集中点云与图像数据的同步发布效果

---

### KITTI 数据集 RViz 可视化主界面
![KITTI RViz 可视化](img/kitti-rviz.png)

### 多模态数据同步显示（点云+相机图像）
![多模态数据面板](img/kitti-multimodal.png)

---

## 运行命令
```bash
# 启动 ROS2 环境
source /opt/ros/humble/setup.bash
source ~/ros2_ws/install/setup.bash

# 启动 KITTI 数据集发布节点
ros2 run kitti_publishers kitti_publisher_node

# 启动 RViz 可视化并加载配置文件
rviz2 -d ~/ros2_ws/src/ros2_kitti_publishers/default.rviz
```

---

## 遇到的问题
1.  **问题**：运行 RViz 时无法显示相机图像话题
    **解决**：检查图像话题的发布名称与 RViz 配置中的 Topic 名称是否一致，确认节点已成功发布 `image_color_left`/`image_gray_left` 等话题，并重新添加 Image 显示插件。

2.  **问题**：激光点云显示为空白或颜色异常
    **解决**：确认点云话题 `PointCloud2` 的 Frame ID 与 RViz 中的 Fixed Frame 一致（如设置为 `base_link`），并调整点云的 Color Transformer 为 `Intensity` 或 `Range`，启用点云着色显示。

---

## 学习心得
通过本周学习，我掌握了 ROS2 下多模态数据集（激光雷达+相机）的发布与可视化流程，理解了 RViz 中不同传感器数据的配置方法，熟悉了 KITTI 数据集的数据格式与话题同步逻辑，为后续自动驾驶感知算法的开发打下了基础。

---

## 返回
[← 返回首页](../)
