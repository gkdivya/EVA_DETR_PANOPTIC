# EVA_DETR_PANOPTIC

Objective of the project is to perform **Panoptic segmentation** on construction materials using transformers - [DETR](https://github.com/facebookresearch/detr). DETR framework is used mainly for object detection and its extended to perform panoptic segmentation, by adding a mask head on top of the decoder outputs.

## DETR FRAMEWORK
**DE**tection **TR**ansformer leverages the transformer network(both encoder and the decoder) for Detecting Objects in Images without using and predefined anchors or NMS approach.

### STEPS
![image](https://user-images.githubusercontent.com/17870236/130376998-6f4fb2fe-bd05-43c1-ad59-678e9d2fc133.png)

1. Use ResNet backbone to extract image vector of size **d x H/32 x W/32**
    * Basically tensor of shape (Batch size, 3, h, w) is converted as (batchsize, 2048, h/32, w/32) with ResNet
    * Then this feature vector is projected to (batchsize, 256, h/32, w/32)
3. Use 1x1 convolution to convert the image vector as 1D - batch size, w/32*h/32, 256
4. Use Positional embeddings learned at input(preferrable) compared to sine based positional embeddings(DETR - default)
5. Encoded image along with the positional embeddings is sent to transformer - encoder
6. Object Queries random embeddings as input to decoder which gets trained in decoder to learn object embeddings
7. Using **BiPartite loss**, we get the final object embeddings corresponding to the things identified in image.
8. Using Hungarian matching algorithm, we finalize the object embeddings.

### DATA CREATION
Using CVAT, objects are annotated in COCO format.
![image](https://user-images.githubusercontent.com/17870236/130353342-27fda0a2-3c75-4272-9d72-124180e2bdcb.png)

**Along with the construction material images, coco images dataset is combined for detecting stuffs.**

### ADVANTAGES OF DETR 
* Differentiable
* Set prediction formulation without any geometric priors like NMS, anchor generation


## PANOPTIC SEGMENTATION
Panoptic segmentation segments every pixel in the image identifying each and every class in the image
![image](https://user-images.githubusercontent.com/17870236/130378972-5abd6d6a-11e8-41cb-a3c4-6948b7c85cd5.png)

### MODELING

![image](https://user-images.githubusercontent.com/17870236/130357338-9afb73a2-88e4-4206-a530-87c355cb4dee.png)

### STEPS

1. Train DETR to predict the construction materials(things) and stuffs.
2. Once trained, we freeze the weights and train the **MASK HEAD** for 25 epochs
3. Image is passed through the CNN and the activations of the intermediate ResNet layers are kept aside
4. Encoded Image (dxH/32xW/32) from **CNN - Resnet backbone** and object embeddings (d * N) are passed through Multi-head attention.
5. Attention scores of the encoded image corresponding to each object embedding(both thing and stuff) generates  N x M(Attention heads) x H/32 x W/32 maps.
6. The maps are further upsampled and concatenated with the step 3. activations of the intermediate layers to produce high resolution maps.
7. Overlay the maps on top of the image and by using pixel wise argmax, we get the final panoptic segmented image


## REFERENCES
**Videos & Blogs:**</br>
https://www.youtube.com/watch?v=utxbUlo9CyY&t=1s </br>
https://giou.stanford.edu/ </br>
**Papers:**</br>
https://arxiv.org/pdf/2005.12872.pdf</br>



