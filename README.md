# Fast Video Object Segmentation by Reference Guided Mask Propagation
Implementation of [Fast Video Object Segmentation by Reference-Guided Mask Propagation](https://openaccess.thecvf.com/content_cvpr_2018/papers/Oh_Fast_Video_Object_CVPR_2018_paper.pdf) paper which participated in the [DAVIS](https://davischallenge.org/) challenge.

## Trainning
[DAVIS-2017](https://davischallenge.org/davis2017/code.html) is the largest public benchmark dataset for the video object segmentation, and provides a training set consisting of 60 videos. This is not enough to train the deep network from scratch even though it used pre-trained weights for the encoder.

### Two Stage Trainning
Pre-Traing on simulated samples and then Fine-tuning on video data

#### Pre-training on simulated samples
In the first stage,
I used image datasets with instance object masks ([Pascal VOC](https://pjreddie.com/projects/pascal-voc-dataset-mirror/)), I simulated the trainning data by having the real image and mask as the refrence frame and and the mirrored image as the new target frame and the mirrored/deformed mask as the previous frame mask 

Image transformation:

 -Mirror on both the Image and the Mask
 
 -Shearing on the mask to deform it

##### Before

![before](https://user-images.githubusercontent.com/62859032/131570732-024d52e4-fef9-44b2-b21d-27b8051ee137.png)

##### After

![after](https://user-images.githubusercontent.com/62859032/131570750-0f1cd74c-6c49-455d-b96c-6b05997660b3.png)

#### Dataset and Dataloader
For the pre-trained data I extended the class Dataset from pytorch and modified the __getitem__() function, it will return a sample which type is dict contains 

-Image

-Mask

-Target Image

-Target Mask

-The output mask (to calculate the loss on it)



## Model Structure
![Screenshot 2021-08-22 220831](https://user-images.githubusercontent.com/62859032/130368685-53b1d7c4-087c-4bff-9eca-e732701f6a5c.png)

### Encoders
The encoder takes a pair of RGB images, each with a mask map, as an input. The encoder includes a reference and a target stream each consist of one convolutional layer attached to a resnet50 model.

### Global Convolutional Block
The outputs of the two encoder streams are concatenated and fed into a global convolution block. This block is designed to perform global feature matching between the reference and the target streams to localize the target object

### Decoder
The decoder takes the output of the global convolution block and also features in the target encoder stream
through skip-connections to produce a mask output.
To efficiently merge features in different scales, it employ the refinement module as the building block of our decoder. 
