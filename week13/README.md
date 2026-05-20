# Week 13：PyBullet 四足机器人站立仿真实验

## 一、实验目的
1.  掌握Ubuntu Linux环境下PyBullet物理仿真引擎的安装与配置方法
2.  学会使用Python调用PyBullet加载URDF机器人模型并实现关节控制
3.  理解四足机器人站立姿态的实现原理，掌握关节角度与力矩的调试方法
4.  能够独立排查并解决机器人仿真中的常见问题（关节ID不匹配、姿态倒置、力矩不足等）
5.  为后续四足机器人步态控制、运动规划等进阶实验奠定基础

## 二、实验环境
- 操作系统：Ubuntu 22.04 LTS
- Python版本：Python 3.10
- 核心库：PyBullet 3.2.6
- 机器人模型：Unitree Laikago 四足机器人URDF模型（16关节带脚趾版本）
- 图形渲染：OpenGL 3.3

## 三、实验内容与步骤

### 3.1 安装PyBullet仿真环境
执行以下命令安装PyBullet库：
```bash
pip3 install pybullet
```

### 3.2 编写机器人站立仿真脚本
创建`quad_trot.py`文件，实现机器人加载、空中姿态校准与稳定站立功能：
```python
import pybullet as p
import pybullet_data
import time

def main():
    # 初始化GUI仿真环境
    p.connect(p.GUI)
    p.setAdditionalSearchPath(pybullet_data.getDataPath())
    p.setGravity(0, 0, -9.8)
    p.loadURDF("plane.urdf")

    # 加载机器人到空中，避免初始砸地
    start_pos = [0, 0, 0.5]
    robot = p.loadURDF("laikago/laikago_toes.urdf", start_pos)

    # 创建固定约束，将机器人身体钉在空中，不受重力影响
    fixed_constraint = p.createConstraint(
        robot, -1, -1, -1, p.JOINT_FIXED,
        [0,0,0], [0,0,0], start_pos
    )

    dt = 1/240

    # 匹配Laikago 16关节模型的站立角度
    stand_angles = {
        # 右前腿 FR (ID 0-3)
        0: -0.15,  # 髋外展
        1:  0.7,   # 髋前屈
        2: -1.3,   # 膝屈曲
        3:  0.0,   # 脚趾固定
        # 左前腿 FL (ID 4-7)
        4:  0.15,  # 髋外展
        5:  0.7,   # 髋前屈
        6: -1.3,   # 膝屈曲
        7:  0.0,   # 脚趾固定
        # 右后腿 RR (ID 8-11)
        8: -0.15,  # 髋外展
        9: -0.7,   # 髋后伸
        10: 1.3,   # 膝屈曲
        11: 0.0,   # 脚趾固定
        # 左后腿 RL (ID 12-15)
        12: 0.15,  # 髋外展
        13:-0.7,   # 髋后伸
        14: 1.3,   # 膝屈曲
        15: 0.0    # 脚趾固定
    }

    print("正在空中校准站立姿态...")
    # 给3秒时间让所有关节完全到位
    for _ in range(720):
        for jid, ang in stand_angles.items():
            p.setJointMotorControl2(
                robot, jid, p.POSITION_CONTROL,
                targetPosition=ang, force=500
            )
        p.stepSimulation()
        time.sleep(dt)

    print("姿态校准完成，机器人落地...")
    # 移除固定约束，机器人以站立姿态平稳落地
    p.removeConstraint(fixed_constraint)

    print("✅ 机器人成功四脚着地稳定站立")
    # 永久保持站立状态
    while True:
        for jid, ang in stand_angles.items():
            p.setJointMotorControl2(
                robot, jid, p.POSITION_CONTROL,
                targetPosition=ang, force=300
            )
        p.stepSimulation()
        time.sleep(dt)

if __name__ == "__main__":
    main()
```

在终端中运行仿真脚本：
```bash
python3 quad_trot.py
```

## 四、实验结果
成功运行脚本后，弹出Bullet Physics仿真窗口，机器人执行以下流程：
1.  初始悬浮在空中，四条腿同时弯曲校准到标准站立姿态
2.  姿态校准完成后平稳落地，**四个脚掌完全接触地面**
3.  机器人保持稳定站立状态，无晃动、无倾倒，仿真窗口正常运行

## 五、关键问题与解决方案

### 1. 机器人始终倒地无法站立
**问题现象**：无论如何调整关节角度，机器人始终以肚皮朝上或侧躺的姿态倒地。
**根本原因**：PyBullet不同版本的Laikago URDF模型关节ID顺序和数量存在差异，之前使用的12关节模型与本地16关节带脚趾版本不匹配，所有角度都作用在了错误的关节上。
**解决方案**：
- 编写关节信息打印脚本，获取本地模型的真实关节ID和名称
- 根据打印结果重新映射关节ID，编写完全匹配的站立角度表

### 2. 关节力矩不足无法撑起机身
**问题现象**：机器人关节有动作，但无法完全伸直，机身始终贴地。
**根本原因**：默认的关节驱动力矩过小，不足以支撑机器人的重量。
**解决方案**：
- 初始姿态校准阶段将力矩提升至500N，确保关节能完全到位
- 站立保持阶段使用300N的力矩，平衡稳定性与能耗

### 3. 初始加载姿态倒置
**问题现象**：机器人加载时默认肚皮朝上，即使关节角度正确也无法站立。
**根本原因**：URDF模型的基准坐标系定义为肚皮朝上。
**解决方案**：
- 采用"空中校准法"：先将机器人用固定约束钉在空中
- 在无重力干扰的环境下摆好站立姿态
- 姿态完全正确后再移除约束，让机器人平稳落地

### 4. 仿真窗口自动退出
**问题现象**：运行脚本后只输出PyBullet编译时间，仿真窗口一闪而过。
**根本原因**：代码中缺少主循环，程序执行完初始化后直接退出。
**解决方案**：在代码末尾添加无限循环，持续发送关节控制指令，保持仿真窗口运行。

## 六、学习心得
通过本次四足机器人站立仿真实验，我深入理解了物理仿真环境下机器人控制的核心逻辑。与纯软件编程不同，机器人仿真需要同时考虑物理规律、机械结构和控制算法三个维度，任何一个环节的偏差都会导致实验失败。

本次实验中遇到的一系列问题让我深刻认识到，在机器人仿真开发中，**"先验证模型，再编写控制"**是至关重要的原则。在不了解机器人模型真实关节结构的情况下盲目调整参数，只会事倍功半。通过打印关节信息、空中姿态校准等方法，我们可以系统性地排查问题，逐步实现预期的控制效果。

这次实验也为我后续学习四足机器人Trot步态、跳跃运动等复杂动作打下了坚实的基础，让我对机器人运动控制领域有了更直观和深入的认识。
