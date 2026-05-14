# Week X：OpenCV 图像处理实验
## 实验内容
本周完成了以下任务：
1.  安装图像处理依赖库（ImageMagick、OpenCV 相关组件）
2.  运行 Python 脚本 `cat_test.py` 读取并显示图像
3.  解决 OpenCV 运行时字体缺失问题

---

## 实验截图
### 图像读取与显示结果
![OpenCV 读取并显示猫咪图像](img/opencv-cat.png)

---

## 运行命令
```bash
# 安装图像处理依赖
sudo apt install graphicsmagick-imagemagick-compat
sudo apt install imagemagick-6.q16
sudo apt install imagemagick-6.q16hdri

# 运行 Python 图像处理脚本
python3 cat_test.py
```

---

## 关键日志与问题解决
### 1. 命令执行报错
- **问题**：终端输入了代码片段（如括号、字符串），导致 `bash` 报语法错误。
- **解决**：直接在终端中运行代码片段会被视为命令，需将代码写入 `.py` 文件再执行，避免直接在终端中粘贴 Python 代码。

### 2. 图片读取错误
- **问题**：提示“找不到图片 cat.png”
- **解决**：将图片文件与脚本放在同一目录下，或在代码中使用图片的绝对路径，确保文件路径正确。

### 3. OpenCV 字体缺失警告
- **问题**：`FontDatabase: Cannot find font directory` 警告，提示 Qt 字体缺失。
- **解决**：
  1.  安装字体依赖：
      ```bash
      sudo apt install fonts-dejavu
      ```
  2.  临时解决（代码中添加环境变量）：
      ```python
      import os
      os.environ['QT_QPA_FONTDIR'] = '/usr/share/fonts/truetype/dejavu'
      ```

---

## 学习心得
通过本周实验，我掌握了 Ubuntu 环境下 OpenCV 图像处理的基础流程，学会了处理依赖安装、路径错误和运行时警告等常见问题，理解了 OpenCV 读取并显示图像的核心逻辑，为后续的图像预处理、特征提取等进阶任务打下了基础。

---

## 返回
[← 返回首页](../)
