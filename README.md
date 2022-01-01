# Swin Transformer for Semantic Segmentaion

This repo contains the supported code and configuration files to reproduce semantic segmentaion results of [Swin Transformer](https://arxiv.org/pdf/2103.14030.pdf). It is based on [mmsegmentaion](https://github.com/open-mmlab/mmsegmentation/tree/v0.11.0).

## Updates

***05/11/2021*** Models for [MoBY](https://github.com/SwinTransformer/Transformer-SSL) are released

***04/12/2021*** Initial commits

## Results and Models

### ADE20K

| Backbone | Method | Crop Size | Lr Schd | mIoU | mIoU (ms+flip) | #params | FLOPs | config | log | model |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Swin-T | UPerNet | 512x512 | 160K | 44.51 | 45.81 | 60M | 945G | [config](configs/swin/upernet_swin_tiny_patch4_window7_512x512_160k_ade20k.py) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.1/upernet_swin_tiny_patch4_window7_512x512.log.json)/[baidu](https://pan.baidu.com/s/1dq0DdS17dFcmAzHlM_1rgw) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.1/upernet_swin_tiny_patch4_window7_512x512.pth)/[baidu](https://pan.baidu.com/s/17VmmppX-PUKuek9T5H3Iqw) |
| Swin-S | UperNet | 512x512 | 160K | 47.64 | 49.47 | 81M | 1038G | [config](configs/swin/upernet_swin_small_patch4_window7_512x512_160k_ade20k.py) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.1/upernet_swin_small_patch4_window7_512x512.log.json)/[baidu](https://pan.baidu.com/s/1ko3SVKPzH9x5B7SWCFxlig) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.1/upernet_swin_small_patch4_window7_512x512.pth)/[baidu](https://pan.baidu.com/s/184em63etTMsf0cR_NX9zNg) |
| Swin-B | UperNet | 512x512 | 160K | 48.13 | 49.72 | 121M | 1188G | [config](configs/swin/upernet_swin_base_patch4_window7_512x512_160k_ade20k.py) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.1/upernet_swin_base_patch4_window7_512x512.log.json)/[baidu](https://pan.baidu.com/s/1YlXXiB3GwUKhHobUajlIaQ) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.1/upernet_swin_base_patch4_window7_512x512.pth)/[baidu](https://pan.baidu.com/s/12B2dY_niMirwtu64_9AMbg) |

**Notes**: 

- **Pre-trained models can be downloaded from [Swin Transformer for ImageNet Classification](https://github.com/microsoft/Swin-Transformer)**.
- Access code for `baidu` is `swin`.

## Results of MoBY with Swin Transformer

### ADE20K

| Backbone | Method | Crop Size | Lr Schd | mIoU | mIoU (ms+flip) | #params | FLOPs | config | log | model |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Swin-T | UPerNet | 512x512 | 160K | 44.06 | 45.58 | 60M | 945G | [config](configs/swin/upernet_swin_tiny_patch4_window7_512x512_160k_ade20k.py) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.3/moby_upernet_swin_tiny_patch4_window7_512x512.log.json)/[baidu](https://pan.baidu.com/s/1i0EMiapoQ-otkDmx-_cJHg) | [github](https://github.com/SwinTransformer/storage/releases/download/v1.0.3/moby_upernet_swin_tiny_patch4_window7_512x512.pth)/[baidu](https://pan.baidu.com/s/1BYgtgkHQV89bGC7LQLS7Jw) |

**Notes**:

- The learning rate needs to be tuned for best practice.
- MoBY pre-trained models can be downloaded from [MoBY with Swin Transformer](https://github.com/SwinTransformer/Transformer-SSL).

## Usage

### Installation

Please refer to [get_started.md](https://github.com/open-mmlab/mmsegmentation/blob/master/docs/en/get_started.md#installation) for installation and dataset preparation.

### Inference
```
# single-gpu testing
python tools/test.py <CONFIG_FILE> <SEG_CHECKPOINT_FILE> --eval mIoU

# multi-gpu testing
tools/dist_test.sh <CONFIG_FILE> <SEG_CHECKPOINT_FILE> <GPU_NUM> --eval mIoU

# multi-gpu, multi-scale testing
tools/dist_test.sh <CONFIG_FILE> <SEG_CHECKPOINT_FILE> <GPU_NUM> --aug-test --eval mIoU
```

### Training

To train with pre-trained models, run:
```
# single-gpu training
python tools/train.py <CONFIG_FILE> --options model.pretrained=<PRETRAIN_MODEL> [model.backbone.use_checkpoint=True] [other optional arguments]

# multi-gpu training
tools/dist_train.sh <CONFIG_FILE> <GPU_NUM> --options model.pretrained=<PRETRAIN_MODEL> [model.backbone.use_checkpoint=True] [other optional arguments] 
```
For example, to train an UPerNet model with a `Swin-T` backbone and 8 gpus, run:
```
tools/dist_train.sh configs/swin/upernet_swin_tiny_patch4_window7_512x512_160k_ade20k.py 8 --options model.pretrained=<PRETRAIN_MODEL> 
```

**Notes:** 
- `use_checkpoint` is used to save GPU memory. Please refer to [this page](https://pytorch.org/docs/stable/checkpoint.html) for more details.
- The default learning rate and training schedule is for 8 GPUs and 2 imgs/gpu.


## Citing Swin Transformer
```
@article{liu2021Swin,
  title={Swin Transformer: Hierarchical Vision Transformer using Shifted Windows},
  author={Liu, Ze and Lin, Yutong and Cao, Yue and Hu, Han and Wei, Yixuan and Zhang, Zheng and Lin, Stephen and Guo, Baining},
  journal={arXiv preprint arXiv:2103.14030},
  year={2021}
}
```

## Other Links

> **Image Classification**: See [Swin Transformer for Image Classification](https://github.com/microsoft/Swin-Transformer).

> **Object Detection**: See [Swin Transformer for Object Detection](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection).

> **Self-Supervised Learning**: See [MoBY with Swin Transformer](https://github.com/SwinTransformer/Transformer-SSL).

> **Video Recognition**, See [Video Swin Transformer](https://github.com/SwinTransformer/Video-Swin-Transformer).
