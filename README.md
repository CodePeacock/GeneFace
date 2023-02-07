# GeneFace: Generalized and High-Fidelity Audio-Driven 3D Talking Face Synthesis | ICLR'23

#### Zhenhui Ye, Ziyue Jiang, Yi Ren, Jinglin Liu, Jinzheng He, Zhou Zhao | Zhejiang University, ByteDance

[![arXiv](https://img.shields.io/badge/arXiv-Paper-%3CCOLOR%3E.svg)](https://arxiv.org/abs/2301.13430)| [![GitHub Stars](https://img.shields.io/github/stars/yerfor/GeneFace)](https://github.com/yerfor/GeneFace) | ![visitors](https://visitor-badge.glitch.me/badge?page_id=yerfor/GeneFace) | [![downloads](https://img.shields.io/github/downloads/yerfor/GeneFace/total.svg)](https://github.com/yerfor/GeneFace/releases) | [中文文档](README-zh.md)

This repository is the official PyTorch implementation of our [ICLR-2023 paper](https://arxiv.org/abs/2301.13430)\, in which we propose **GeneFace** for generalized and high-fidelity audio-driven talking face generation. The inference pipeline is as follows:

<p align="center">
    <br>
    <img src="assets/GeneFace.png" width="1000"/>
    <br>
</p>

Our GeneFace achieves better lip synchronization and expressiveness to out-of-domain audios. Watch [this video](https://geneface.github.io/GeneFace/example_show_improvement.mp4) for a clear lip-sync comparison against previous NeRF-based methods. You can also visit our [project page](https://geneface.github.io/) for more details.

## Quick Start!

We provide pre-trained models of GeneFace in [this release](https://github.com/yerfor/GeneFace/releases/tag/v1.0.0) to enable a quick start. In the following, we show how to infer the pre-trained models in 4 steps. If you want to train GeneFace on your own target person video, please reach to the following sections (`Prepare Environments`, `Prepare Datasets`, and `Train Models`).

Step1. Create a new python env named `geneface` following the guide in `docs/prepare_env/install_guide_nerf.md`.

Step2. Download the `lrs3.zip` in the release and unzip it into the `checkpoints` directory.

Step3. Download the `May.zip` in the release and unzip it into the `checkpoints` directory.

After the above steps, the structure of your `checkpoints` directory should look like this:

```
> checkpoints
    > lrs3
        > lm3d_vae
        > syncnet
    > May
        > postnet
        > lm3d_nerf
        > lm3d_nerf_torso
```

Step4. Run the scripts below:

```
bash scripts/infer_postnet.sh
bash scripts/infer_lm3d_nerf.sh
```

You can find a output video named `infer_out/May/pred_video/zozo.mp4`.

## Prepare Environments

Please follow the steps in `docs/prepare_env`.

## Prepare Datasets

Please follow the steps in `docs/process_data`.

## Train Models

Please follow the steps in `docs/train_models`.

# Train GeneFace on other target person videos

Apart from the `May.mp4` provided in this repo, we also provide 8 target person videos that were used in our experiments. You can download them at [this link](https://drive.google.com/drive/folders/1FwQoBd1ZrBJMrJE3ZzlNhK8xAe1OYGjX?usp=share_link). To train on a new video named `<video_id>.mp4`, you should place it into the `data/raw/videos/` directory, then create a new folder at `egs/datasets/videos/<video_id>` and edit config files, according to the provided example folder `egs/datasets/videos/May`.

You can also record your own video and train a unique GeneFace model for yourself!

# Todo List

- GeneFace use 3D landmark as the intermediate between the audio2motion and motion2image mapping. However, the 3D landmark sequence generated by the postnet sometimes have bad cases (such as shaking head, or extra-large mouth) and influence the quality of the rendered video. Currently, we partially alleviate this problem by postprocessing the predicted 3D landmark sequence. We call for better postprocessing methods.
- The inference process of NeRF-based renderer is relatively slow (it takes about 2 hours on 1 RTX2080Ti to render 250 frames at 512x512 resolution). Currently, we could partially alleviate this problem by setting `--n_samples_per_ray` and `--n_samples_per_ray_fine` to a lower value. In the future we will add acceleration techniques on the NeRF-based renderer.

## Citation

```
@article{ye2023geneface,
  title={GeneFace: Generalized and High-Fidelity Audio-Driven 3D Talking Face Synthesis},
  author={Ye, Zhenhui and Jiang, Ziyue and Ren, Yi and Liu, Jinglin and He, Jinzheng and Zhao, Zhou},
  journal={arXiv preprint arXiv:2301.13430},
  year={2023}
}
```

## Acknowledgements

**Our codes are based on the following repos:**

* [NATSpeech](https://github.com/NATSpeech/NATSpeech) (For the code template)
* [AD-NeRF](https://github.com/YudongGuo/AD-NeRF) (For NeRF-related implementation)
* [style_avatar](https://github.com/wuhaozhe/style_avatar) (For 3DMM parameters extraction)
