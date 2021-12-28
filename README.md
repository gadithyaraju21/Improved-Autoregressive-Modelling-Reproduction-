# Improved Autoregressive Modeling with Distribution Smoothing

This repo contains the implementation for the paper <a href="https://openreview.net/forum?id=rJA5Pz7lHKb&amp;referrer=%5BAuthor%20Console%5D(%2Fgroup%3Fid%3DICLR.cc%2F2021%2FConference%2FAuthors%23your-submissions)">Improved Autoregressive Modeling with Distribution Smoothing</a>

by [Chenlin Meng](https://cs.stanford.edu/~chenlin/), [Jiaming Song](http://tsong.me), [Yang Song](http://yang-song.github.io/), [Shengjia Zhao](http://szhao.me/) and [Stefano Ermon](https://cs.stanford.edu/~ermon/), Stanford AI Lab.

<p align="center">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/mnist_samples.png" width="200">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/cifar10_samples.png" width="200">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/celeba_samples.png" width="200">
</p>

<p align="center">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/mnist_noisy_samples.png" width="200">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/cifar10_noisy_samples.png" width="200">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/celeba_noisy_samples.png" width="200">
</p>


## Running Experiments

### Dependencies

Run the following to install all necessary python packages for our code.

```bash
pip install -r requirements.txt
```

### Stage1: Learning the smoothed distribution
<p align="center">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/smoothing.gif" width="600">
</p>

To train the PixelCNN++ model on the smoothed distribution for CIFAR-10, run:
```
python main.py --runner SmoothedPixelCNNPPTrainRunner --config pixelcnnpp_smoothed_train_cifar10.yml --doc cifar10_smoothed_0.3 --ni
``` 

### Stage2: Reverse smoothing
<p align="center">
<img src="https://github.com/chenlin9/Autoregressive-Modeling-with-Distribution-Smoothing/blob/main/images/denoise.png" width="500">
</p>
To reverse the smoothing process, we train a second PixelCNN++ model conditioned on the smoothed distribution.
To train the model on CIFAR-10, run:

```
python main.py --runner SmoothedPixelCNNPPTrainRunner --config pixelcnnpp_conditioned_train_cifar10.yml --doc reverse_cifar10_0.3 --ni
``` 


### Sampling
Sampling from stage 1:


**pixelcnnpp_smoothed_sample.yml** needs to be modified.

**ckpt_path**: path to the model trained on the smoothed data in stage 1. 

The **dataset** parameter might also need to be modified accordingly. Selections are MNIST, CIFAR10, or celeba.
```
python main.py --runner PixelCNNPPSamplerRunner --config pixelcnnpp_smoothed_sample.yml --doc cifar10_0.3_images
``` 

Sampling from stage 2:

**pixelcnnpp_reverse_sample.yml** needs to be modified.

**noisy_samples_path**: path to the noisy samples generated by the model trained on the smoothed data in stage 1,

**ckpt_path**: path to the reverse smoothing model in stage 2.

The **dataset** parameter might need to be changed accordingly. Selections are MNIST, CIFAR10, or celeba.
```
python main.py --runner PixelCNNPPSamplerRunner --config pixelcnnpp_reverse_sample.yml --doc cifar10_denoise_images
``` 

## References and Acknowledgements
```
@article{meng2021improved,
  title={Improved Autoregressive Modeling with Distribution Smoothing},
  author={Meng, Chenlin and Song, Jiaming and Song, Yang and Zhao, Shengjia and Ermon, Stefano},
  journal={arXiv preprint arXiv:2103.15089},
  year={2021}
}
```


This implementation is based on / inspired by:

- [https://github.com/pclucas14/pixel-cnn-pp](https://github.com/pclucas14/pixel-cnn-pp) (PixelCNN++ PyTorch implementation), 
and
- [https://github.com/ermongroup/ncsnv2](https://github.com/ermongroup/ncsnv2) (code structure).