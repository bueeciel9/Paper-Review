Focal transformer:


VoxSeT: 


Single-stride Sparse Transformer (SST)
1.	Scale challenges in 3D object detection
 
 ![image](https://user-images.githubusercontent.com/65759092/180610854-851a486c-629f-45c0-ab62-679a33322c54.png)
<p align="center"><img src="https://user-images.githubusercontent.com/65759092/180610854-851a486c-629f-45c0-ab62-679a33322c54.png"></p>

This figure shows the scales of objects in 2D image exhibit a long-tail distribution, while in 3D space they are quite concentrated due to the non-projective transformation used in voxelization. 
The object size in 3D object detectors is usually tiny while no large objects exist
->	Do we really need downsampling in 3D object detectors?
->	Single-stride architecture with no downsampling operators. 
There are two issues lead to discard of downsampling operators:
1)	The increase of computation cost
2)	The decrease of receptive field
2.	Single-stride Sparse Transformer(SST)
->	Sparse Regional Attention(SRA) which is local attention mechanism is proposed, and by stacking SRA layers, Single-stride Sparse Transformer(SST) is proposed. 

 
![image](https://user-images.githubusercontent.com/65759092/180610848-21a811cb-db6d-4dc5-8792-16454df72b1c.png)



RDIoU: if VoxSeT uses 3D IoU, check whether i can change it to RDIoU.
RDIoU: exsiting 3D IoU is sensitive to rotation, thus can cause training instability and detection performance deterioration.
-> RIoU mitigate the rotation-sensitivity issue, and produce more fficient optimization objectives compared ith 3D IoU during the training stage.

Voxsl-MAE: MAE-style pre-training on voxelized point clouds. good for large-scale point cloud dataset.
pretty data-efficient. and reduces the need for annotated data. I can refer it for futuer research.

Masked autoencoder(MAE): use only 25% of patches(masked random patches) as input image and reconstruct the missing pixels.
And then, reconstruct the original image from the latent representation and mask tokens.
VERY GOOD PERFORMANCE. I CAN RERER IT.
