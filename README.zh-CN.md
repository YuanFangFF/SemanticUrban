<div align="center">

# SemanticUrban

[English](README.md) | **简体中文**

### 基于点云的精确城市场景理解：SemanticUrban 数据集

一个面向城市场景语义分割的大规模、高分辨率地面激光扫描（TLS）点云数据集。

[![Paper](https://img.shields.io/badge/Paper-ESWA%202026-b31b1b.svg)](https://www.sciencedirect.com/science/article/abs/pii/S0957417426018610)
[![DOI](https://img.shields.io/badge/DOI-10.1016%2Fj.eswa.2026.132949-blue.svg)](https://doi.org/10.1016/j.eswa.2026.132949)
[![Project Page](https://img.shields.io/badge/Project-Page-success.svg)](https://zhuqinfeng1999.github.io/SemanticUrban/)
[![Dataset](https://img.shields.io/badge/Download-Google%20Drive-4285F4.svg)](https://drive.google.com/file/d/1fJ5kdstFQScT6Buv46qM-zR1h_lQdy8H/view?usp=sharing)
![Scenes](https://img.shields.io/badge/Scenes-150-orange.svg)
![Points](https://img.shields.io/badge/Points-~4B-orange.svg)
![Classes](https://img.shields.io/badge/Classes-23-orange.svg)
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

</div>

---

<!-- 题图：请在此处上传标注示例图（论文图 1） -->
<div align="center">
<img src="assets/teaser.png" width="90%" alt="SemanticUrban 标注示例"/>
<br/>
<em>SemanticUrban 的标注示例，采用球坐标投影方式进行可视化。</em>
</div>

---

## 概述

**SemanticUrban** 是一个大规模、高分辨率的点云数据集，使用地面激光扫描（TLS）从 **150 个城市场景** 中采集而成。该数据集具有超高分辨率的点云以及高度精确的逐点语义标注，将每个点划分为 **23 个语义类别** 之一。该数据集旨在支持语义分割、建图、导航与城市规划等城市场景理解任务。

与已有的 TLS 城市数据集相比，SemanticUrban 具有以下优势：

- **更大规模的场景数量**（150 个场景，约 40 亿个点）。
- **覆盖 23 个语义类别的高精细、高准确度标注**。
- **包含较少被标注的现象**，例如反射（来自玻璃或水面等）以及运动畸变物体。
- **精确的物体边界**，并同时提供强度信息与 RGB 颜色信息。

> 本仓库提供用于复现论文全部实验结果的 **预处理基准版本（preprocessed benchmark release）**。完整的原始数据可按需申请获取（参见 [下载](#下载)）。

## 目录

- [概述](#概述)
- [亮点](#亮点)
- [数据集统计信息](#数据集统计信息)
- [语义类别](#语义类别)
- [下载](#下载)
- [数据格式](#数据格式)
- [预处理细节](#预处理细节)
- [基准测试结果](#基准测试结果)
- [引用](#引用)
- [联系方式](#联系方式)
- [致谢](#致谢)
- [许可协议](#许可协议)

## 亮点

- 面向城市场景的大规模、高分辨率地面点云数据集。
- 场景数量大（150 个场景），采集自中国四座一线城市。
- 标注高度精细、类别丰富（23 个类别）。
- 提供多种代表性网络（KPConv、MinkowskiNet、OctFormer、PTv1、PTv2）的基准性能。
- 系统梳理了相关挑战与研究机会，以推动后续研究。

## 数据集统计信息

| 属性 | 说明 |
|---|---|
| 场景数量 | 150 |
| 点的数量 | 约 40 亿（全部已标注） |
| 语义类别 | 23 |
| 传感器 | Leica RTC 360（带 HDR 全景成像的 TLS） |
| 视场角 | 水平 360° / 垂直 300° |
| 角度精度 | 18 角秒 |
| 测距精度 | 1.0 mm + 10 ppm |
| 三维点精度 | 在 10 / 20 / 30 m 处分别为 1.9 / 2.9 / 5.3 mm |
| 逐点信息 | 三维坐标、强度、RGB |

**官方划分**

| 划分 | 场景数 | 列表文件 |
|---|---|---|
| 训练集 | 105 | `datasets/Univ/Univ_train.txt` |
| 验证集 | 15 | `datasets/Univ/Univ_val.txt` |
| 测试集 | 30 | `datasets/Univ/Univ_test.txt` |

## 语义类别

基准测试使用 **23 个有效语义类别**。原始标签 `0` 表示未标注的点（因遮挡或距离过远而无法识别的物体），在训练和评估中会被忽略。在基准设置中，原始标签 `1-23` 对应训练标签 `0-22`，而原始标签 `0` 被设为 `ignore_index = -100`。

| 原始标签 | 训练标签 | 类别名称 |
|:---:|:---:|---|
| 0 | -100（忽略） | 未标注 |
| 1 | 0 | 运动畸变（Distortion） |
| 2 | 1 | 道路（Road） |
| 3 | 2 | 其他人造地面（Other man-made terrain） |
| 4 | 3 | 建筑物（Building） |
| 5 | 4 | 墙体（Wall） |
| 6 | 5 | 围栏（Fence） |
| 7 | 6 | 杆状物（Pole） |
| 8 | 7 | 台阶（Stairs） |
| 9 | 8 | 交通标志（Traffic sign） |
| 10 | 9 | 灌木（Shrub） |
| 11 | 10 | 树木（Tree） |
| 12 | 11 | 草地（Grass） |
| 13 | 12 | 裸土（Soil） |
| 14 | 13 | 行人（Person） |
| 15 | 14 | 小汽车（Car） |
| 16 | 15 | 卡车（Truck） |
| 17 | 16 | 其他车辆（Other vehicle） |
| 18 | 17 | 桥梁（Bridge） |
| 19 | 18 | 摩托车（Motorcycle） |
| 20 | 19 | 自行车（Bicycle） |
| 21 | 20 | 杂物与垃圾（Clutter and rubbish） |
| 22 | 21 | 花坛（Flowerbed） |
| 23 | 22 | 反射（Reflections） |

<!-- 可选：请在此处上传点云示例图（论文图 2） -->
<div align="center">
<img src="assets/classes.png" width="90%" alt="包含强度、RGB 与类别标签的点云示例"/>
<br/>
<em>点云示例：强度（左）、RGB（中）与类别标签（右）。</em>
</div>

## 下载

我们发布的是 **预处理基准版本（preprocessed benchmark package）**（体素大小 0.05 m）。论文中报告的全部结果均可仅凭该版本复现。解压后的大小约为 **38.8 GB**。

### 数据集更新说明

以下 `npy_05` 下载链接已于 **2026 年 6 月 22 日** 更新。此前上传的版本中，部分语义标签文件存在预处理错误：旧脚本读取了语义标签列之后的辅助列，当这些辅助列包含 `NaN` 时，会错误地将第 8 列中的有效语义标签重置为 `0`。原始 TXT 标注本身是正确的。

我们已从原始 TXT 文件重新生成完整的 `npy_05` 数据包，预处理时仅读取 `x, y, z, r, g, b, strength, label` 这 8 列。此前上传版本中共有 **36** 个 `<scene>_segment.npy` 文件受到影响；坐标、颜色、强度与重索引文件未受影响。如果你在此次更新前下载过 `npy_05`，请重新下载下方修正后的版本。

<details>
<summary>此前上传的 npy_05 包中受影响的 segment 文件</summary>

`Job_010_002`, `Job_010_005`, `Job_010_008`, `Job_010_032`, `Job_010_042`, `Job_011_016`, `Job_011_023`, `Job_011_031`, `Job_012_007`, `Job_012_010`, `Job_012_019`, `Job_012_022`, `Job_012_025`, `Job_012_042`, `Job_013_001`, `Job_013_007`, `Job_013_010`, `Job_014_014`, `Job_014_018`, `Job_014_024`, `Job_084_025`, `Job_084_027`, `Job_084_032`, `Job_084_034`, `Job_084_058`, `Job_084_064`, `Job_084_075`, `Job_084_081`, `Job_085_039`, `Job_085_061`, `Job_085_080`, `Ning_I_010`, `Ning_I_012`, `Ning_I_031`, `Ning_I_037`, `Ning_I_049`

</details>

| 资源 | 大小 | 链接 |
|---|---|---|
| 预处理基准版本（`npy_05`） | 约 38.8 GB（解压后） | [Google Drive](https://drive.google.com/file/d/1fJ5kdstFQScT6Buv46qM-zR1h_lQdy8H/view?usp=sharing) |
| 预处理基准版本（`npy_05`） | 约 38.8 GB（解压后） | [百度网盘](https://pan.baidu.com/s/1JpMlncciIs-2Gm7UjR2-Dg?pwd=n6wz) &nbsp; 提取码：`<n6wz>` |
### 原始数据（可选）

**原始 TXT 数据**（约 **249.9 GB**）**不** 包含在默认的基准版本中。如需申请原始数据，请发送邮件至通讯作者 **lei.fan@xjtlu.edu.cn**，作者会尽快回复。

> 论文中的基准结果均使用预处理后的 `npy_05` 版本复现。若另行获取原始 TXT 数据，则其仅作为可选的补充资料。

## 数据格式

解压后，数据包的目录结构如下：

```
datasets/
└── Univ/
    ├── Univ_train.txt
    ├── Univ_val.txt
    ├── Univ_test.txt
    └── npy_05/
        ├── <scene_name>_coord.npy
        ├── <scene_name>_color.npy
        ├── <scene_name>_strength.npy
        ├── <scene_name>_segment.npy
        └── <scene_name>_reidx.npy
```

每个场景以五个 NumPy 数组的形式存储：

| 文件 | 数据类型 | 形状 | 说明 |
|---|---|---|---|
| `<scene>_coord.npy` | float32 | `[N, 3]` | 经裁剪与 0.05 m 体素采样后的 XYZ 坐标 |
| `<scene>_color.npy` | float32 | `[N, 3]` | RGB 颜色值，取值范围 0-255 |
| `<scene>_strength.npy` | float32 | `[N]` | 强度 / 反射率 |
| `<scene>_segment.npy` | int | `[N]` | 语义标签（原始标签 `0` 被忽略；`1-23` 为有效类别） |
| `<scene>_reidx.npy` | int64 | `[M]` | 从裁剪后的稠密点到体素化点的逆向索引 |

> `<scene>_reidx.npy` 用于保持数据完整性，并可用于将预测结果反投影回稠密点云。

可直接使用 NumPy 读取单个场景：

```python
import numpy as np

coord    = np.load("datasets/Univ/npy_05/<scene>_coord.npy")     # [N, 3] float32, XYZ 坐标
color    = np.load("datasets/Univ/npy_05/<scene>_color.npy")     # [N, 3] float32, RGB，范围 0-255
strength = np.load("datasets/Univ/npy_05/<scene>_strength.npy")  # [N]    float32, 强度
segment  = np.load("datasets/Univ/npy_05/<scene>_segment.npy")   # [N]    语义标签（0 被忽略，1-23 为有效类别）
```

### 完整性校验（可选）

如需校验下载的文件，可生成并比对 SHA-256 校验值：

```bash
find datasets/Univ/npy_05 -type f -name "*.npy" | sort | xargs sha256sum > SemanticUrban_npy05_sha256.txt
sha256sum datasets/Univ/Univ_*.txt >> SemanticUrban_npy05_sha256.txt
```

## 预处理细节

预处理版本由原始 TXT 文件按如下步骤生成：

1. 仅读取前 8 列：`x, y, z, r, g, b, strength, label`。
2. 忽略语义标签列之后的所有辅助列。
3. 将已读取列中的 `NaN` 数值替换为 `0`，并将 `NaN` 标签置为 `0`。
4. 将点裁剪至 `x, y` 位于 `[-35, 35]` 范围内。
5. 以 `voxel_size = 0.05 m` 进行体素化。
6. 保存 `coord` / `color` / `strength` / `segment` / `reidx` 数组。

## 基准测试结果

我们使用平均交并比（`mIoU`）评估了五种代表性网络，分别在包含和不包含 RGB 信息的情况下进行测试。完整的逐类别 `IoU` 结果详见论文。

| 模型 | 参数量（M） | 推理延迟（s） | 不含 RGB 的 mIoU（%） | 含 RGB 的 mIoU（%） |
|---|:---:|:---:|:---:|:---:|
| KPConv | 24.39 | 12.494 | 21.46 | 33.94 |
| MinkowskiNet | 37.86 | 0.252 | 49.10 | 52.21 |
| OctFormer | 44.06 | 0.129 | 43.41 | 47.10 |
| PTv1 | 9.58 | 1.140 | 45.24 | 48.04 |
| **PTv2** | 11.32 | 0.290 | **50.08** | **52.26** |

<sub>推理延迟在单张 NVIDIA RTX 5090D GPU 上测得，批大小为 1，每个样本 12 万个点。GPU 显存占用与类别不平衡消融实验详见论文。</sub>

<!-- 可选：请在此处上传定性结果 / 失败案例图 -->
<!--
<div align="center">
<img src="assets/results.png" width="90%" alt="定性分割结果"/>
</div>
-->

## 引用

如果 SemanticUrban 对您的研究有所帮助，请引用我们的论文：

```bibtex
@article{FANG2026132949,
  title   = {Towards Accurate Urban Scene Understanding using Point Clouds: The SemanticUrban Dataset},
  author  = {Fang, Yuan and Zhu, Qinfeng and Cai, Yuanzhi and Fan, Lei},
  journal = {Expert Systems with Applications},
  pages   = {132949},
  year    = {2026},
  issn    = {0957-4174},
  doi     = {https://doi.org/10.1016/j.eswa.2026.132949}
}
```

## 联系方式

如对数据集有任何疑问、需申请原始 TXT 数据或有其他事宜，请联系：

- **范磊（Lei Fan，通讯作者）**：lei.fan@xjtlu.edu.cn

作者：方圆（Yuan Fang）、朱钦峰（Qinfeng Zhu）、蔡远志（Yuanzhi Cai）、范磊（Lei Fan）。

## 致谢

本工作部分由西交利物浦大学研究发展基金（项目编号 REF-21-01-003）以及西交利物浦大学研究生科研奖学金（项目编号 PGRS2006010）资助。

## 免责声明

SemanticUrban 数据集采集自公共环境中的真实城市场景，可能包含受第三方权利约束的内容，包括但不限于出现在公共空间中的个人信息或肖像、车辆、建筑物、标识及其他物体。本数据集由作者采集并发布，仅供学术与非商业研究使用。

作者不就数据集或其任何部分可用于商业用途作出任何陈述或保证。由于部分场景可能包含尚未获得商业使用所需权利的数据，对数据集的任何商业使用都可能引发潜在的法律责任，包括但不限于隐私、个人信息保护以及知识产权方面的问题。因此，严禁将本数据集用于任何商业用途。

本数据集按“现状”提供，不附带任何明示或暗示的保证。用户须自行确保其对数据集的使用符合所有适用的法律法规。对于因使用本数据集而引起的任何索赔、损害或其他后果，作者及其所属机构概不承担责任。

## 许可协议

SemanticUrban 数据集采用 [知识共享 署名-非商业性使用-相同方式共享 4.0 国际许可协议（CC BY-NC-SA 4.0）](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh-hans) 进行许可。

**本数据集仅供学术与非商业研究使用，严禁任何形式的商业用途。** 在本协议下，您可以出于非商业目的自由地共享和修改数据，但需满足以下条件：（1）给予适当署名并引用我们的论文；（2）不得将数据用于商业目的；（3）对数据的任何修改或衍生作品须以相同的协议进行分发。

任何商业用途，包括但不限于出售数据、利用数据开发、训练或评估商业产品或服务，以及任何以商业利益或经济报酬为目的或导向的使用，均须事先获得作者的明确书面许可。如需商业授权，请联系 lei.fan@xjtlu.edu.cn。
