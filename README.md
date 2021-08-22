# Fast Video Object Segmentation by Reference Guided Mask Propagation
Implementation of [Fast Video Object Segmentation by Reference-Guided Mask Propagation](https://openaccess.thecvf.com/content_cvpr_2018/papers/Oh_Fast_Video_Object_CVPR_2018_paper.pdf) paper which participated in the [DAVIS](https://davischallenge.org/) challenge.

## Model Structure
![Screenshot 2021-08-22 220831](https://user-images.githubusercontent.com/62859032/130368685-53b1d7c4-087c-4bff-9eca-e732701f6a5c.png)

### Encoders
it is two parallel encoders, each consist of one convolutional layer attached to a resnet50 model.

### Global Convolutional Block
The output of the two encoders are concatenated then feeded to the GCB

### Decoder
Which consists of 3 refinement modules taking the output of the previous block, upsample it and Add it to a skip connection from the resnet50 encoder  
