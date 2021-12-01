# [Rich feature hierarchies for accurate object detection and semantic segmentation](https://arxiv.org/abs/1311.2524)
###  Input image is received
### Extracts ~2000 region proposals
- This is done via [[selective search]] which allows for comparing with previously detected objects.
### Computes features for each proposal using a large CNN
- Extract a fixed-length feature vector from each proposal
- [Affine image warping](https://www.researchgate.net/publication/328968274_NumPy_SciPy_Recipes_for_Image_Processing_Affine_Image_Warping) is used to compute a fixed-size CNN input irrespective of the regional shape
- To train the CNN, supervised pretraining is performed on a large auxiliary dataset (ILSVRC), followed by domain-specific fine-tuning on a small dataset (PASCAL)
- 94% of the learned paramters can be removed with insignificant drops in detection accuracy. Simple bounding box regression dramatically reduces mislocalizations, which are the dominant cause of errors.

### Classifies each region using class-specific SVMs 

Note: this allows for the entire system to be quite computationally efficient. The only class-specific computations are small matrix-vector product and greedy non-maximum suppression. This allows for higher scaling
