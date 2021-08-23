# EVA_DETR_PANOPTIC

Objective of the project is to perform **Panoptic segmentation** on construction materials using transformers - [DETR](https://github.com/facebookresearch/detr). DETR framework is used mainly for object detection and its extended to perform panoptic segmentation, by adding a mask head on top of the decoder outputs.

## DETR FRAMEWORK

## ADVANTAGES OF DETR 
* Differentiable
* Set prediction formulation without any geometric priors like NMS, anchor generation


## PANOPTIC SEGMENTATION

## STEPS

### DATA CREATION
Using CVAT, objects are annotated in COCO format.
![image](https://user-images.githubusercontent.com/17870236/130353342-27fda0a2-3c75-4272-9d72-124180e2bdcb.png)


### MODELING


### PANOPTIC SEGMENTATION
![image](https://user-images.githubusercontent.com/17870236/130357338-9afb73a2-88e4-4206-a530-87c355cb4dee.png)

1. Train DETR to predict the construction materials(things) and stuffs.
2. Once trained, we freeze the weights and train the **MASK HEAD** for 25 epochs
3. Image is passed through the CNN and the activations of the intermediate ResNet layers are kept aside
4. Encoded Image (dxH/32xW/32) from **CNN - Resnet backbone** and object embeddings (d * N) are passed through Multi-head attention.
5. Attention scores of the encoded image corresponding to each object embedding(both thing and stuff) generates  NxMxH/32xW/32 maps.
6. The maps are further upsampled and concatenated with the step 3. activations of the intermediate layers to produce high resolution maps.
7. 

We take the encoded image and send it to Multi-Head Attention (FROM WHERE DO WE TAKE THIS ENCODED IMAGE?) </br>
We also send dxN Box embeddings to the Multi-Head Attention
We do something here to generate. (WHAT DO WE DO HERE?)
Then we concatenate these maps with Res5 Block (WHERE IS THIS COMING FROM?)
aarch.png
Then we perform the above steps (EXPLAIN THESE STEPS)
And then we are finally left with the panoptic segmentation


## REFERENCES
**Videos & Blogs:**</br>
https://www.youtube.com/watch?v=utxbUlo9CyY&t=1s </br>
**Papers:**</br>
https://arxiv.org/pdf/2005.12872.pdf</br>



