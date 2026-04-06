# PTG-FSCIR: Pseudo Triplet Guided Few-shot Composed Image Retrieval

> Official resources for **PTG-FSCIR**, a two-stage pseudo-triplet guided framework for **few-shot composed image retrieval (FS-CIR)**, published at **IJCNN 2025**.

## Authors

**Bohan Hou**<sup>1</sup>, **Haoqiang Lin**<sup>1</sup>, **Haokun Wen**<sup>2,5</sup>, **Meng Liu**<sup>3</sup>, **Mingzhu Xu**<sup>4</sup>\*, **Xuemeng Song**<sup>1</sup>\*

<sup>1</sup> Department of Computer Science and Technology, Shandong University, Qingdao, China  
<sup>2</sup> School of Computer Science and Technology, Harbin Institute of Technology (Shenzhen), Shenzhen, China  
<sup>3</sup> Department of Computer Science, Shandong Jianzhu University, Jinan, China  
<sup>4</sup> School of Software, Shandong University, Jinan, China  
<sup>5</sup> Department of Data Science, City University of Hong Kong, Hong Kong, China  
\* Corresponding authors

## Links

- **Paper (arXiv)**: [`arXiv:2407.06001`](https://arxiv.org/abs/2407.06001)
- **Paper (IEEE / DOI)**: [`IJCNN 2025`](https://doi.org/10.1109/IJCNN64981.2025.11228450)
- **Code Repository**: [`GitHub`](https://github.com/hbhalpha/PTG-FSCIR)

---

## Table of Contents

- [Updates](#updates)
- [Released Resources](#released-resources)
- [Our Backbones](#our-backbones)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Dataset / Benchmark](#dataset--benchmark)
- [Usage](#usage)
  - [Masked Image Data](#masked-image-data)
  - [Score Data](#score-data)
- [Citation](#citation)
- [Acknowledgement](#acknowledgement)
- [License](#license)

---

## Updates

- [07/2024] Release arXiv preprint
- [06/2025] Paper published at IJCNN 2025
- [04/2026] Open-source release of resources and code


---

## Released Resources

We have open-sourced the data and code used in this work, including:

1. **Masked image resources**
   - Code for calculating attention masks
   - The module for adding attention masks
   - Mask patches extracted from ImageNet

2. **Second-stage caption and score resources**
   - Image captions used in the second stage on **FashionIQ**, **CIRR**, and **B2W**
   - Filtering and scoring results derived from these captions

这些资源可用于复现论文中两阶段训练流程中的关键部分，也可作为后续 Few-shot CIR 研究的辅助材料。

---

## Our Backbones

In our method, the two backbones used have already been open-sourced in previous research.

### SPRC

SPRC is available on GitHub:

- [SPRC Repository](https://github.com/chunmeifeng/SPRC)

### CLIP4CIR

CLIP4CIR is also publicly available:

- [CLIP4CIR Repository](https://github.com/ABaldrati/CLIP4Cir)

---

## Project Structure

```text
.
├── README.md
├── mask/                       # attention masks / masked blocks
├── score/
│   ├── SPRC/                   # score files for SPRC backbone
│   └── CLIP4CIR/               # score files for CLIP4CIR backbone
├── image_masked.py             # utilities for loading and applying masks
├── creat_masked_patches.py     # script for creating masked patches on other datasets
└── ...
```


---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/hbhalpha/PTG-FSCIR.git
cd PTG-FSCIR
```

### 2. Create environment

```bash
python -m venv .venv
source .venv/bin/activate   # Linux / Mac
# .venv\Scripts\activate    # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

> 如果你使用的是 conda、poetry、uv 或 docker，请按自己的实际环境替换安装方式。

---

## Dataset / Benchmark

This project involves resources for the following few-shot CIR benchmarks:

- **FashionIQ**
- **CIRR**
- **Birds-to-Words (B2W)**

Additionally, the released masked image resources are based on **ImageNet** patches and can be adapted to other image datasets.

---

## Usage

### Masked Image Data

Our mask blocks are located in the `/mask/` directory. You can use them together with the utility functions provided in `image_masked.py`.

You can also use `creat_masked_patches.py` to create masked datasets for other image collections.

Here is an example showing how to load an image and apply masks in your dataset code:

```python
def get_img(self, img_id, mask_list):
    img_path = os.path.join(self.path, f"test/ILSVRC2012_test_{img_id}.JPEG")
    with open(img_path, 'rb') as f:
        img = PIL.Image.open(f)
        img = img.convert('RGB')
    masked_img = self.resize(img.copy())
    masked_img = add_atten_masked(masked_img, mask_list)
```

You can apply the above process as the **first-stage pseudo triplet construction** step for the backbone.

### Score Data

You can find our computed scores in the `/score/{backbone}/` directory.

- Each key corresponds to an index
- The sequence of these indices aligns with the public dataset captions
- You may split samples according to the scores and perform random selection as needed

These selected samples can then be used as the **second-stage fine-tuning samples** for the corresponding backbone.

---



---

## Citation

If you use this repository or find our resources helpful, please cite:

```bibtex
@inproceedings{hou2025pseudo,
  author    = {Bohan Hou and Haoqiang Lin and Haokun Wen and Meng Liu and Mingzhu Xu and Xuemeng Song},
  title     = {Pseudo Triplet Guided Few-shot Composed Image Retrieval},
  booktitle = {Proceedings of the International Joint Conference on Neural Networks},
  pages     = {1--8},
  publisher = {IEEE},
  year      = {2025},
  doi       = {10.1109/IJCNN64981.2025.11228450}
}
```

You may also cite the arXiv version:

```bibtex
@article{hou2024pseudo,
  author  = {Bohan Hou and Haoqiang Lin and Haokun Wen and Meng Liu and Mingzhu Xu and Xuemeng Song},
  title   = {Pseudo-triplet Guided Few-shot Composed Image Retrieval},
  journal = {arXiv preprint arXiv:2407.06001},
  year    = {2024},
  doi     = {10.48550/arXiv.2407.06001}
}
```

---

## Acknowledgement

- Thanks to the authors of the backbone projects **SPRC** and **CLIP4CIR**
- Thanks to the open-source community for providing useful CIR baselines and tools
- Thanks to the dataset providers of FashionIQ, CIRR, and B2W

---

## License

Please follow the license terms of this repository, as well as the licenses of the original backbone projects and datasets before use.
