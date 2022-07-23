**Focal transformer:**
The ability of capturing local and global visual dependencies through self-attention is the key to its success. 
-> Focal Attention: a new attention mechanism that incorporates both fine-gradined local and coarse-grained global interactions.
Each token attends its closest surrounding tokens at fine granuarity and the tokens far away at coarse granularity, and thus can capture both short- and long-range visual dependencies efficiently and effectively. 


<br/>

**Visformer:**
over-fitting especially when the training data is limited.
: transformer model -> convolution model.
Consist this model is better than BoTNet. This paper analyzes the difference between CNN and Transformer. Important paper!
<br/>

**MobileFormer:** Different from vision transformer that uses image patches to form tokens, the transformer in Mobile-Former takes very few learnable tokens as input that are randomly initialized.
Mobile(MobileNet) and Former communicate through a bidirectional bridge.
The paper starts with this question: How to design efficient networks to effectively encode both local processing and global interaction? -> combine convolution and vision transformer. Design paradigm from series to parallel.
<br/>

**BoTNet:** Like the ResNet Bottleneck, invented Bottleneck Transformer
Instead of 3*3 conv, by putting MHSA into the bottleneck, made bottleneck transformer.
This model is faster than EfficientNet. So, in the future, I can change the CNN to MHSA and check the performance. If it’s good, for the general model, we can modify the model.
<br/>

**DAT(Deformable attention transformer)**
Like ViT, using dense attention costs lots of computation and features can be influenced by irrelevant parts. Sparse attention is data agnostic and may limit the ability to model long range relations.
	DAT, a general backbone model with deformable attention for both image classification and dense prediction tasks. deformable self-attention module: positions of key and value pairs in self-attention are selected in a data-dependent way. 
As a backbone model, accuracy is better than Swin, but need more understand this model.
<br/>

**CAT-DET** uses Pointformer branch, Imageformer branch, and Cross-Modal Transformer module. Imageformer branch compensates the occluded part which can happen in Pointformer branch. + One-way multi-modal data augmentation. 
So, I can refer this model when I make the multi-modal fusion later. 
<br/>

**DEPTHFORMER** presented a multiscale encoder-decoder structure, in which it suggested a better way to fuse encoder and decoder features. But as far as I study, for the 3D object detection using outdoor dataset, multiscale encoder-decoder cannot be effective. 
<br/>

**VoxSeT**
<br/>

Hard to compute the self-attention on large-scale point cloud data because point cloud is a long sequence and unevenly distributed in 3D space. -> Existing methods compute self-attention locally by grouping the points into clusters of the same size or perform convolutional self-attention on discretized representation. VoxSeT use set-to-set translation. 
<br/>

<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180611350-a76f51e1-54f4-4c7c-9e88-57154d9ff457.png"></p>
<br/>

The main limitation of transformer models: self-attention computation is quadratic.
Convolutional attention is a point-wise operation, the attention field of the convolutional kernel is typically small, thus hindering to model long-range dependencies.
1.	Voxel-based set attention (VSA) module
Divide the whole scene into non-overlapping 3D voxels and compute the voxel indices of the input point with instant efficiency. With these voxels, determine the attentive region like the window attention in SwinTransformer. Unlike image, LiDAR has irregular structures, the resulting attention groups have different lengths, which hinders the parallelization of the model.
<br/>

<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180611390-1744e17e-fa7b-4757-9445-7a23b7833154.png"></p>
<br/>

The input set is encoded to a hidden space, then the hidden features are refined through a ConvFFN and finally decoded to produce the output set.
<br/>

<p align = "center"><img src="https://user-images.githubusercontent.com/65759092/180611390-1744e17e-fa7b-4757-9445-7a23b7833154.png"></p>
<br/>

2.	VoxSeT overall architecture
<br/>

<p align = "center"><img src="https://user-images.githubusercontent.com/65759092/180611408-00cfd057-8933-40f6-8ae9-2bc48b1bfeb1.png"></p>
<br/>

They empirically found that applying large voxels can learn richer context information, and demonstrate better understanding of the objects with sparse points.
<br/>

<p align = "center"><img src="https://user-images.githubusercontent.com/65759092/180611414-261fb491-9773-46e9-8a0c-a7f50fd6546c.png"></p>
<br/>

**DESTR**
The reasons Transformers slightly lag in performance behind CNN-based detectors:
1) Cross-attention is used for both classification and bounding-box regression tasks.
2) Transformer's decoder poorly initializes content queries.
3) Self-attention poorly accounts for certain prior knowledge which could help improve inductive bias.
Solution 
(1) DESTR separates estimation of cross-attention into two independent branches 
- one tailored for classification and the other for box regression
(2) Mini-detector to initialize the content queries in the decoder with classification and regression embeddings of the respective heads in the mini-detector.
<br/>

<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180611311-cd0db086-bb70-40b3-9f8d-ca7883f3fb89.png"></p>
<br/>

**Early Convolutions help Transformers See Better**
ViT models are sensitive to the choice of optimizer (adamW vs SGD), hyperparameters. In this paper, this reason is called patchify system of ViT models. 
<br/>

<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180611243-ff6bb90a-be85-429f-ab3f-37aef91bec09.png"></p>
<br/>

Therefore, it is said that using a small 3*3 convolution instead of a patch does not sensitize the optimization, but also performs slightly better on the final model accretion.
Based on this theory, I am planning to apply the convolution in VoxSeT.
<br/>

**Single-stride Sparse Transformer (SST)**
1.	Scale challenges in 3D object detection
<br/>

<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180610854-851a486c-629f-45c0-ab62-679a33322c54.png"></p>
<br/>

This figure shows the scales of objects in 2D image exhibit a long-tail distribution, while in 3D space they are quite concentrated due to the non-projective transformation used in voxelization. 
The object size in 3D object detectors is usually tiny while no large objects exist
->	Do we really need downsampling in 3D object detectors?
->	Single-stride architecture with no downsampling operators. 
There are two issues lead to discard of downsampling operators:
1)	The increase of computation cost
2)	The decrease of receptive field
2.	Single-stride Sparse Transformer(SST)
->	Sparse Regional Attention(SRA) which is local attention mechanism is proposed, and by stacking SRA layers, Single-stride Sparse Transformer(SST) is proposed. 
<br/>

<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180611041-15973bb0-c966-4970-818f-e51a99b56c87.png"></p>
<br/>

**RDIoU:** if VoxSeT uses 3D IoU, check whether i can change it to RDIoU.
RDIoU: exsiting 3D IoU is sensitive to rotation, thus can cause training instability and detection performance deterioration.
-> RIoU mitigate the rotation-sensitivity issue, and produce more fficient optimization objectives compared ith 3D IoU during the training stage.
<br/>

**Voxsl-MAE:** MAE-style pre-training on voxelized point clouds. good for large-scale point cloud dataset.
pretty data-efficient. and reduces the need for annotated data. I can refer it for futuer research.
<br/>

**Masked autoencoder(MAE):** use only 25% of patches(masked random patches) as input image and reconstruct the missing pixels.
And then, reconstruct the original image from the latent representation and mask tokens.
VERY GOOD PERFORMANCE. I CAN RERER IT.
