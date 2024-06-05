# CS2230.CH181

## Information
- Course: CS2230.CH181
- Members:
  - 230202023 - Lê Đăng Dũng
  - 220202020 - Nguyễn Duy Minh Nhật
  - 230101051 - Lăng Huỳnh Đăng Khoa

## Table of contents
   * [Installation](#installation)
   * [Getting started](#getting-started)
   * [Citation](#citation)
   
## Installation

Instructions below use Conda, if you don't have Conda installed, you can check out [How to Install Conda](https://docs.conda.io/en/latest/miniconda.html#latest-miniconda-installer-links).

```bash
# Create a virtual env. We use Conda
conda env create -f environment_mbvt3.yml
conda activate mbvt3

# install requirements and CVNets package
pip install -r requirements.txt
pip install --editable .
```

## Getting started

### Preparing dataset

1. Create new virtual env for data loader

```bash
# Create a virtual env. We use Conda
conda create -n dataloader python==3.7
conda activate dataloader

# install requirements
pip install -r scripts/requirements-coco.txt
```

1. Change **root_dir** and **save_root_dir** in scripts/coco.py, **global_path** in download_coco.sh to your dataset path

1. Run scripts/download_coco.sh

### Training detection network on the MS-COCO dataset

1. Activate the mbvt3 virtual env

```bash
conda activate mbvt3
```

1. Change the **root_train** and **root_val** in config/detection/ssd_coco/mobilevit_v3.yaml to your coco dataset path

1. Run the command below

```bash
PYTHONWARNINGS="ignore" cvnets-train --common.config-file config/detection/ssd_coco/mobilevit_v3.yaml --common.results-loc ssd_mobilevitv3_results/detection/width_1_0_0 --common.override-kwargs --model.classification.pretrained pretrained/mobilevitv3_1_0_0.pt
```

### Evaluation

An example command to run detection on an image using SSDLite-MobileViTv3 model is given below

```bash
cvnets-eval-det --common.config-file config/detection/ssd_coco/mobilevit_v3.yaml --common.results-loc ssdlite_mobilevitv3_results --model.detection.pretrained ssd_mobilevitv3_results/detection/width_1_0_0/run_1/checkpoint_best.pt --model.detection.n-classes 81 --evaluation.detection.resize-input-images --evaluation.detection.mode single_image --evaluation.detection.path <IMAGE_PATH> --model.detection.ssd.conf-threshold 0.3
```

An example command to run detection on images stored in a folder using SSDLite-MobileViTv3 model is given below

```bash
cvnets-eval-det --common.config-file config/detection/ssd_coco/mobilevit_v3.yaml --common.results-loc ssdlite_mobilevitv3_results --model.detection.pretrained ssd_mobilevitv3_results/detection/width_1_0_0/run_1/checkpoint_best.pt --model.detection.n-classes 81 --evaluation.detection.resize-input-images --evaluation.detection.mode image_folder --evaluation.detection.path <IMAGE_FOLDER_PATH> --model.detection.ssd.conf-threshold 0.3
```

## Citation

``` 
@inproceedings{mehta2022mobilevit,
     title={MobileViT: Light-weight, General-purpose, and Mobile-friendly Vision Transformer},
     author={Sachin Mehta and Mohammad Rastegari},
     booktitle={International Conference on Learning Representations},
     year={2022}
}

@inproceedings{mehta2022cvnets, 
     author = {Mehta, Sachin and Abdolhosseini, Farzad and Rastegari, Mohammad}, 
     title = {CVNets: High Performance Library for Computer Vision}, 
     year = {2022}, 
     booktitle = {Proceedings of the 30th ACM International Conference on Multimedia}, 
     series = {MM '22} 
}

@inproceedings{wadekar2022mobilevitv3,
  title = {MobileViTv3: Mobile-Friendly Vision Transformer with Simple and Effective Fusion of Local, Global and Input Features},
  author = {Wadekar, Shakti N. and Chaurasia, Abhishek},
  doi = {10.48550/ARXIV.2209.15159},
  year = {2022}
}
```

