<div align="center">

# SemanticUrban

**English** | [简体中文](README.zh-CN.md)

### Towards Accurate Urban Scene Understanding using Point Clouds: The SemanticUrban Dataset

A large-scale, high-resolution terrestrial laser scanning (TLS) point cloud dataset for urban scene semantic segmentation.

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

<!-- Teaser figure: upload your annotation example image (paper Fig. 1) here -->
<div align="center">
<img src="assets/teaser.png" width="90%" alt="SemanticUrban annotation examples"/>
<br/>
<em>Annotation examples from SemanticUrban, visualized via spherical coordinate projection.</em>
</div>

---

## Overview

**SemanticUrban** is a large-scale, high-resolution point cloud dataset acquired from **150 urban scenes** using terrestrial laser scanning. It features super-high-resolution point clouds and highly accurate per-point semantic annotations, classifying each point into one of **23 semantic classes**. The dataset is designed to support urban scene understanding tasks such as semantic segmentation, mapping, navigation, and urban planning.

Compared with existing TLS urban datasets, SemanticUrban offers:

- **A much larger number of scenes** (150 scenes, ~4 billion points).
- **Highly detailed and accurate annotations** across **23 semantic categories**.
- **Rarely labeled phenomena**, including reflections (e.g., from glass or water) and distorted moving objects.
- **Precise object boundaries** together with intensity and RGB color information.

> This repository hosts the **preprocessed benchmark release** used to reproduce all results reported in the paper. The full raw data is available upon request (see [Download](#download)).

## Table of Contents

- [Overview](#overview)
- [Highlights](#highlights)
- [Dataset Statistics](#dataset-statistics)
- [Semantic Classes](#semantic-classes)
- [Download](#download)
- [Data Format](#data-format)
- [Preprocessing Details](#preprocessing-details)
- [Benchmark Results](#benchmark-results)
- [Citation](#citation)
- [Contact](#contact)
- [Acknowledgements](#acknowledgements)
- [License](#license)

## Highlights

- A large-scale high-resolution terrestrial point cloud dataset of urban scenes.
- Large number of scenes (150 scenes) collected from four first-tier cities in China.
- Highly detailed annotations and rich categories (23 classes).
- Benchmark performances of representative networks (KPConv, MinkowskiNet, OctFormer, PTv1, PTv2).
- Documented challenges and opportunities to motivate future research.

## Dataset Statistics

| Attribute | Description |
|---|---|
| Scenes | 150 |
| Number of points | ~4 billion (all labelled) |
| Semantic classes | 23 |
| Sensor | Leica RTC 360 (TLS with HDR panoramic imaging) |
| Field of view | 360 deg (horizontal) / 300 deg (vertical) |
| Angular accuracy | 18 arcsec |
| Range accuracy | 1.0 mm + 10 ppm |
| 3D point accuracy | 1.9 / 2.9 / 5.3 mm @ 10 / 20 / 30 m |
| Per-point information | 3D coordinates, intensity, RGB |

**Official split**

| Split | Scenes | List file |
|---|---|---|
| Train | 105 | `datasets/Univ/Univ_train.txt` |
| Validation | 15 | `datasets/Univ/Univ_val.txt` |
| Test | 30 | `datasets/Univ/Univ_test.txt` |

## Semantic Classes

The benchmark uses **23 valid semantic classes**. Raw label `0` denotes unannotated points (occluded or distant objects) and is ignored during training and evaluation. In the benchmark setup, raw labels `1-23` correspond to training labels `0-22`, while raw label `0` is treated as `ignore_index = -100`.

| Raw label | Training label | Class name |
|:---:|:---:|---|
| 0 | -100 (ignored) | Unannotated |
| 1 | 0 | Distortion |
| 2 | 1 | Road |
| 3 | 2 | Other man-made terrain |
| 4 | 3 | Building |
| 5 | 4 | Wall |
| 6 | 5 | Fence |
| 7 | 6 | Pole |
| 8 | 7 | Stairs |
| 9 | 8 | Traffic sign |
| 10 | 9 | Shrub |
| 11 | 10 | Tree |
| 12 | 11 | Grass |
| 13 | 12 | Soil |
| 14 | 13 | Person |
| 15 | 14 | Car |
| 16 | 15 | Truck |
| 17 | 16 | Other vehicle |
| 18 | 17 | Bridge |
| 19 | 18 | Motorcycle |
| 20 | 19 | Bicycle |
| 21 | 20 | Clutter and rubbish |
| 22 | 21 | Flowerbed |
| 23 | 22 | Reflections |

<!-- Optional: upload the example point cloud figure (paper Fig. 2) here -->
<div align="center">
<img src="assets/classes.png" width="90%" alt="Example point clouds with intensity, RGB, and class labels"/>
<br/>
<em>Example point clouds: intensity (left), RGB (middle), and class labels (right).</em>
</div>


## Download

We release the **preprocessed benchmark package** (voxel size 0.05 m). All results reported in the paper can be reproduced from this package alone. The decompressed size is approximately **38.8 GB**.

### Dataset update notice

The `npy_05` download links below were updated on **22 June 2026**. A previous uploaded package contained a preprocessing error in some semantic label files: the old script read auxiliary columns after the semantic label column, and when those auxiliary columns contained `NaN` values, it incorrectly reset valid labels in column 8 to `0`. The original raw TXT annotations are correct.

We regenerated the complete `npy_05` package from the original TXT files by reading only `x, y, z, r, g, b, strength, label`. In the previous uploaded package, **36** `<scene>_segment.npy` files were affected; the coordinate, color, strength, and re-index files were unchanged. If you downloaded `npy_05` before this update, please download the corrected package below.

<details>
<summary>Affected segment files in the previous uploaded npy_05 package</summary>

`Job_010_002`, `Job_010_005`, `Job_010_008`, `Job_010_032`, `Job_010_042`, `Job_011_016`, `Job_011_023`, `Job_011_031`, `Job_012_007`, `Job_012_010`, `Job_012_019`, `Job_012_022`, `Job_012_025`, `Job_012_042`, `Job_013_001`, `Job_013_007`, `Job_013_010`, `Job_014_014`, `Job_014_018`, `Job_014_024`, `Job_084_025`, `Job_084_027`, `Job_084_032`, `Job_084_034`, `Job_084_058`, `Job_084_064`, `Job_084_075`, `Job_084_081`, `Job_085_039`, `Job_085_061`, `Job_085_080`, `Ning_I_010`, `Ning_I_012`, `Ning_I_031`, `Ning_I_037`, `Ning_I_049`

</details>

| Resource | Size | Link |
|---|---|---|
| Preprocessed benchmark (`npy_05`) | ~38.8 GB (decompressed) | [Google Drive](https://drive.google.com/file/d/1fJ5kdstFQScT6Buv46qM-zR1h_lQdy8H/view?usp=sharing) |
| Preprocessed benchmark (`npy_05`) | ~38.8 GB (decompressed) | [Baidu Cloud](https://pan.baidu.com/s/1JpMlncciIs-2Gm7UjR2-Dg?pwd=n6wz) &nbsp; Extraction code: `<n6wz>` |

### Raw data (optional)

The **raw TXT data** (approximately **249.9 GB**) is **not** included in the default benchmark package. To request access to the raw data, please email the corresponding author at **lei.fan@xjtlu.edu.cn**. The author will respond as soon as possible.

> The benchmark results in the paper are reproduced using the preprocessed `npy_05` package. The raw TXT release, if obtained, is an optional supplement.

## Data Format

After decompression, the package follows this layout:

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

Each scene is stored as five NumPy arrays:

| File | Dtype | Shape | Description |
|---|---|---|---|
| `<scene>_coord.npy` | float32 | `[N, 3]` | XYZ coordinates after cropping and 0.05 m voxel sampling |
| `<scene>_color.npy` | float32 | `[N, 3]` | RGB values in range 0-255 |
| `<scene>_strength.npy` | float32 | `[N]` | Intensity / reflectance |
| `<scene>_segment.npy` | int | `[N]` | Semantic labels (raw `0` ignored; `1-23` valid classes) |
| `<scene>_reidx.npy` | int64 | `[M]` | Inverse index from cropped dense points to voxelized points |

> `<scene>_reidx.npy` is kept for completeness and for possible prediction back-projection to the dense point cloud.

A single scene can be read directly with NumPy:

```python
import numpy as np

coord    = np.load("datasets/Univ/npy_05/<scene>_coord.npy")     # [N, 3] float32, XYZ
color    = np.load("datasets/Univ/npy_05/<scene>_color.npy")     # [N, 3] float32, RGB in 0-255
strength = np.load("datasets/Univ/npy_05/<scene>_strength.npy")  # [N]    float32, intensity
segment  = np.load("datasets/Univ/npy_05/<scene>_segment.npy")   # [N]    semantic labels (0 ignored, 1-23 valid)
```

### Integrity check (optional)

To verify the downloaded files, you can generate and compare SHA-256 checksums:

```bash
find datasets/Univ/npy_05 -type f -name "*.npy" | sort | xargs sha256sum > SemanticUrban_npy05_sha256.txt
sha256sum datasets/Univ/Univ_*.txt >> SemanticUrban_npy05_sha256.txt
```

## Preprocessing Details

The preprocessed package was generated from the raw TXT files by:

1. Reading only the first eight columns: `x, y, z, r, g, b, strength, label`.
2. Ignoring any auxiliary columns after the semantic label column.
3. Replacing `NaN` values within the loaded columns with `0`, and assigning `NaN` labels to `0`.
4. Cropping points to `x, y` in `[-35, 35]`.
5. Voxelizing with `voxel_size = 0.05 m`.
6. Saving the `coord` / `color` / `strength` / `segment` / `reidx` arrays.

## Benchmark Results

We evaluate five representative networks using mean Intersection over Union (`mIoU`), both with and without RGB information. Full per-class `IoU` tables are available in the paper.

| Model | Params (M) | Latency (s) | mIoU w/o RGB (%) | mIoU w/ RGB (%) |
|---|:---:|:---:|:---:|:---:|
| KPConv | 24.39 | 12.494 | 21.46 | 33.94 |
| MinkowskiNet | 37.86 | 0.252 | 49.10 | 52.21 |
| OctFormer | 44.06 | 0.129 | 43.41 | 47.10 |
| PTv1 | 9.58 | 1.140 | 45.24 | 48.04 |
| **PTv2** | 11.32 | 0.290 | **50.08** | **52.26** |

<sub>Latency measured on a single NVIDIA RTX 5090D GPU, batch size 1, 120k points per sample. See the paper for GPU memory footprint and the class-imbalance ablation.</sub>

<!-- Optional: upload a qualitative results / failure-case figure here -->
<!--
<div align="center">
<img src="assets/results.png" width="90%" alt="Qualitative segmentation results"/>
</div>
-->

## Citation

If you find SemanticUrban useful in your research, please cite our paper:

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

## Contact

For questions about the dataset, requests for the raw TXT data, or other inquiries, please contact:

- **Lei Fan** (corresponding author): lei.fan@xjtlu.edu.cn

Authors: Yuan Fang, Qinfeng Zhu, Yuanzhi Cai, Lei Fan.

## Acknowledgements

This work is partially supported by the Xi'an Jiaotong-Liverpool University Research Enhancement Fund (grant number REF-21-01-003) and the Xi'an Jiaotong-Liverpool University Postgraduate Research Scholarship (grant number PGRS2006010).

## Disclaimer

The SemanticUrban dataset was acquired from real-world urban scenes in public environments and may contain elements that are subject to third-party rights, including but not limited to the personal information or likeness of individuals, vehicles, buildings, signage, and other objects appearing in public spaces. The dataset has been collected and released by the authors solely for academic and non-commercial research purposes.

The authors make no representations or warranties that the dataset, or any portion of it, is cleared for commercial use. Because certain scenes may contain data for which the rights necessary for commercial exploitation have not been obtained, any commercial use of the dataset may give rise to potential legal liabilities, including but not limited to privacy, personal data protection, and intellectual property concerns. For this reason, commercial use of the dataset is strictly prohibited.

The dataset is provided "as is" without warranty of any kind, express or implied. Users are solely responsible for ensuring that their use of the dataset complies with all applicable laws and regulations. The authors and their affiliated institutions accept no liability for any claims, damages, or other consequences arising from the use of the dataset.

## License

The SemanticUrban dataset is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/).

**This dataset is provided strictly for academic and non-commercial research purposes only. Commercial use of any kind is prohibited.** Under this license you are free to share and adapt the data for non-commercial purposes, provided that you (1) give appropriate credit and cite our paper, (2) do not use the data for commercial purposes, and (3) distribute any derivative works under the same license.

Any commercial use, including but not limited to selling the data, using it to develop, train, or evaluate commercial products or services, or any use directed toward commercial advantage or monetary compensation, requires explicit prior written permission from the authors. For commercial licensing inquiries, please contact lei.fan@xjtlu.edu.cn.
