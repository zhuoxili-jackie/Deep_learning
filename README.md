# FSNet 运动去模糊（Motion Deblurring）

基于 **FSNet**（频域选择网络）的单图运动去模糊项目，在 **GOPRO** 数据集上完成训练、推理与可视化。
全部流程整合在一个 Jupyter Notebook 中，无需命令行参数，开箱即用，CPU / GPU 均可运行。

> 课程大作业 · 深度学习

---

## 目录结构

```
.
├── FSNet_Deblur.ipynb     # 一体化 notebook：配置 / 模型 / 数据 / 训练 / 推理 / 可视化
├── GOPRO/                 # 数据集（见下方说明）
│   ├── train/
│   │   ├── blur/          # 训练 - 模糊图  (2103 张)
│   │   └── sharp/         # 训练 - 清晰图  (2103 张)
│   └── valid/
│       ├── blur/          # 验证 - 模糊图  (1111 张)
│       └── sharp/         # 验证 - 清晰图  (1111 张)
├── makaron.pkl            # 可视化配色（马卡龙调色板）
├── math.yaml             # 可视化字体 / 公式样式配置
└── results/              # 训练 / 推理产物（运行时自动生成，已在 .gitignore 中忽略）
```

---

## 环境依赖

- Python 3.8+
- PyTorch（CPU 或 CUDA 版本均可）
- 其余：`numpy`、`pillow`、`matplotlib`、`pyyaml`、`scikit-image`、`tqdm`

```bash
pip install torch torchvision numpy pillow matplotlib pyyaml scikit-image tqdm
```

---

## 数据集（GOPRO）

本仓库已包含整理好的 GOPRO 数据集（`train` / `valid`，各含 `blur` 与 `sharp` 配对）。
GOPRO 是运动去模糊领域的标准基准数据集（Nah et al., CVPR 2017）。如需重新获取原始数据：

- 官方主页 / 下载：<https://seungjunnah.github.io/Datasets/gopro>

> 目录约定：`GOPRO/<split>/blur/*.png` 与 `GOPRO/<split>/sharp/*.png` 一一对应（同名配对）。

---

## 使用方法

1. 在本仓库根目录（`work` 文件夹）启动 Jupyter：
   ```bash
   jupyter notebook FSNet_Deblur.ipynb
   ```
2. 打开 **第 1 节「配置区」**，按需修改：
   - `DATASET`：数据集外层文件夹名（默认 `"GOPRO"`）；
   - 第 6 节里的 `RUN_TRAINING`：设为 `True` 开始训练，`False` 则跳过训练直接推理。
3. 按顺序运行各单元格即可。Notebook 章节如下：

| 章节 | 内容 |
| --- | --- |
| 1   | 配置区 + 马卡龙可视化全局样式 |
| 2   | 模型定义（基础层 + FSNet 网络结构） |
| 3   | 数据管线（成对增强、`DataLoader`、配对与频域可视化） |
| 4–5 | 工具函数、学习率 warmup 调度器 |
| 6   | 训练代码 + 训练过程可视化（损失 / 学习率 / 验证 PSNR） |
| 7   | 推理（加载权重，计算 PSNR）+ 逐张 PSNR 可视化 |
| 8   | 对比图（模糊 / FSNet 恢复 / GT 三栏并排）、残差热力图与频域复原 |

### 主要超参数（见配置区）

| 参数 | 默认值 |
| --- | --- |
| `batch_size` | 2 |
| `learning_rate` | 1e-4 |
| `num_epoch` | 3000 |
| `num_worker` | 0（Windows 下最稳） |
| `device` | 自动选择 CUDA / CPU |

---

## 结果与可视化

- 恢复图输出至 `results/test/`，三栏对比图输出至 `results/comparison/`。
- 评价指标：**PSNR**（逐张 + 平均）。
- 所有图表统一采用马卡龙配色（`makaron.pkl`）与 `math.yaml` 中的字体 / 公式样式，
  含训练曲线、PSNR 分布、残差热力图与频域（FFT）复原分析。

---

## 参考

- **FSNet** — Cui *et al.*, *Image Restoration via Frequency Selection*（频域选择思想）。
- **GOPRO** — Nah *et al.*, *Deep Multi-scale CNN for Dynamic Scene Deblurring*, CVPR 2017。

---

## 作者

- **zhuoxili-jackie** · <zhuoxili@hdu.edu.cn> · 杭州电子科技大学（HDU）
