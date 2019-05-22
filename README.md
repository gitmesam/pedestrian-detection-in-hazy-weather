# Pedestrian detection in hazy weather
Traffic environments are constantly changing with fluctuating weather, driving, road, and pedestrian conditions. This flux is especially pronounced in China due to frequent haze that obstructs visibility on city roadways. As such, an efficient and accurate pedestrian detection algorithm for use during hazy weather is necessary.
![detection_expample_pbmn](pictures/pbmn.jpg)

## Approaches
We used mobilenet_v2 as backbone(Code also supports mobilenet_v1), and then proposed a layer called weighted combination to improve the performance of model. The weighted combination layer would combine feature maps from different convoutional layer, and then vis an attention module to recalibrating the feature map through learning weights in each channel(["Squeeze-and-Excitation Networks"](https://arxiv.org/pdf/1709.01507)) or both channel and spatial(["CBAM: Convolutional Block Attention Module"](https://arxiv.org/pdf/1807.06521.pdf)), following figure show the whole network architecture.
<div align=center><img src="pictures/structure.png"></div>

## Evaluation
We used the average precision(AP) to evaluate our model, the AP for pedestrian would be calculated by using the mAP criterium defined in the [PASCAL VOC 2012 competition](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/)., of which the tools were from [here](https://github.com/Cartucho/mAP). The following fugure show the PR curve of our model.
<div align=center><img width="400" height="288" src="pictures/pbmn.png"></div>

## How to use code
### Requirement
```
tensorflow >= 1.6.0
imgaug ==0.2.8
opencv-python == 3.3.1.11
```

### Training
Below script gives you an example of training a model with our models.
```
python train.py --model_name=prioriboxes_mbn --attention_module=se_block --batch_size=50 --learning_rate=1e-3 --f_log_step=20 --f_summary_step=20 --f_save_step=2000
```
Actually, our code supports multiple network configurations, below script show you some different network configurations.
```
--model_name = prioriboxes_mbn or prioriboxes_vgg
--attention_module = se_block or cbam_block or None
```

when --model_name is prioriboxes_mbn, you colud choose backbone and config the weighted combination layer with following flags:
```
--backbone_name = mobilenet_v2 or mobilener_v1
--multiscale_feats = True or False
```

### Prediction
The job of prediction is to visualize the detection, Below script gives you an example of doing prediction after training.
```
CHECKPOINT_DIR_NAME: something like /usr/my_dir/checkpoint
python predict.py --model_name=prioriboxes_mbn --attention_module=se_block --backone_name=mobilenet_v2 --multiscale_feats=True --whether_aug=True --checkpoint_dir=CHECKPOINT_DIR_NAME
```

### Evaluation
Below script gives you an example of evaluating a model after training.
```
CHECKPOINT_DIR_NAME: something like /usr/my_dir/checkpoint
1. python evaluate.py --model_name=prioriboxes_mbn --attention_module=se_block --backone_name=mobilenet_v2 --multiscale_feats=True --checkpoint_dir=CHECKPOINT_DIR_NAME
2. cd evaluation
3. python eval_tools.py
```
### Pre-trained Model
You can download the [checkpoint](https://drive.google.com/open?id=1UOF1ACYA3Nn5K_RjoItOUFSxnFL6sNOg) and do prediction or evaluation.
Note: In order to use this pre-trained model, you must run with following flags:
```
--model_name=prioriboxes_mbn
--attention_module=se_block
--backbone_name=mobilenet_v2
--multiscale_feats=True
```

## Citations
```
@article{Reza2004,
  title={Realization of the contrast limited adaptive histogram equalization (CLAHE) for real-time image enhancement.},
  author={Reza, Ali M.},
  journal={Journal of VLSI signal processing systems for signal, image and video technology},
  volume={38},
  number={1},
  pages={35--44},
  year={2004}
  publisher={Springer}
}
@article{Jobson1997,
  title={A multiscale retinex for bridging the gap between color images and the human observation of scenes},
  author={Jobson, Daniel J., Zia-ur Rahman, and Glenn A. Woodell.},
  journal={IEEE Transactions on image processing},
  volume={6},
  number={7},
  pages={965--976},
  year={1997},
  publisher={IEEE}
}
@article{Petro2014,
  title={Multiscale retinex},
  author={Petro, Ana Belén, Catalina Sbert, and Jean-Michel Morel},
  journal={Image Processing On Line},
  pages={71--88},
  year={2014}
  publisher={IPOL Journal} 
}
@inproceedings{ying2017new,
  title={A New Image Contrast Enhancement Algorithm Using Exposure Fusion Framework},
  author={Ying, Zhenqiang and Li, Ge and Ren, Yurui and Wang, Ronggang and Wang, Wenmin},
  booktitle={International Conference on Computer Analysis of Images and Patterns},
  pages={36--46},
  year={2017},
  organization={Springer}
 }
@inproceedings{Chen2017,
  title={Fast image processing with fully-convolutional networks},
  author={Chen, Qifeng, Jia Xu, and Vladlen Koltun.},
  booktitle={Proceedings of the IEEE International Conference on Computer Vision. },
  year={2017},
  organization={IEEE}
}
