# Fast Video Object Segmentation by Reference Guided Mask Propagation
Implementation of [Fast Video Object Segmentation by Reference-Guided Mask Propagation](https://openaccess.thecvf.com/content_cvpr_2018/papers/Oh_Fast_Video_Object_CVPR_2018_paper.pdf) paper which participated in the [DAVIS](https://davischallenge.org/) challenge.

## Trainning
[DAVIS-2017](https://davischallenge.org/davis2017/code.html) is the largest public benchmark dataset for the video object segmentation, and provides a training set consisting of 60 videos. This is not enough to train the deep network from scratch even though it used pre-trained weights for the encoder.

### Two Stage Trainning
Pre-Traing on simulated samples and then Fine-tuning on video data

#### Pre-training on simulated samples
In the first stage,
we used image datasets with instance object masks ([Pascal VOC](https://pjreddie.com/projects/pascal-voc-dataset-mirror/)), To automatically generate the training samples from an image with an object mask, by applying two image transformation (rotation and mirror)

![2007_000032](https://user-images.githubusercontent.com/62859032/130882244-db427fd4-2bdb-441f-b4fd-702a681c5368.jpg)



## Model Structure
![Screenshot 2021-08-22 220831](https://user-images.githubusercontent.com/62859032/130368685-53b1d7c4-087c-4bff-9eca-e732701f6a5c.png)

### Encoders
it is two parallel encoders, each consist of one convolutional layer attached to a resnet50 model.

### Global Convolutional Block
The output of the two encoders are concatenated then feeded to the GCB

### Decoder
Which consists of 3 refinement modules taking the output of the previous block, upsample it and Add it to a skip connection from the resnet50 encoder  
