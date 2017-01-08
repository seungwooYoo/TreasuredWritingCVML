# RefineNet:Multi-path refinement networks with identity mappings for high resolution semantic segmentation

--------------------

### Introduction
- Repeated subsampling operations like pooling or convolution striding lead to segmentation performance decrease
- RefineNet : Generic multi-path refinement network that uses all the information available along the down-sampling process using long-range residual connections
    - Individual components of RefineNet : residual connections following the identity mapping 
    - Chained residual pooling
- Some arguments from the paper
    - Features from all levels are helpful for semantic segmentation
        - High-level semantic features : category recognition of image regions
        - Low-level visual features : generate sharp, detailed boundaries for high-resolution prediction
- Contributions
    - Multi-path refinement network
    - End-to-end training - with residual connections with identity mappings 
    - Chained residual pooling - pooling features with multiple window sizes and fusing them together with residual connections
    - Achieve new state-of-the-art performance on 7 public datasets (e.g., IoU 83.4 on the PASCAL VOC 2012)

### Proposed method 
- Multi-path refinement
    - Divide the pre-trained ResNet into 4 blocks -> 4-cascaded architecture with 4 RefineNet units
        - RefineNet-# : set of convolutions adapt the ResNet weights to the task at hand
        - RefineNet-4 and ResNet-3 are fed to RefineNet-3 as 2-path inputs
        - RefineNet-3 is to use the high-resolution features from ResNet block-3 to refine the low-resolution feature map output of RefineNet-4 
        - Finaly stage, soft-max with up-sampling (bilinear interpolation)
    - RefineNet
        - Residual convolution unit (RCU)
            - Two residual convolution units 
        - Multi-resolution fusion
            - All path inputs are fused into a high-resolution feature map
            - Generates feature maps of the same feature dimension by convolution + upsample and then summation for fusing 
        - Chained residual pooling
            - Chain of multiple pooling blocks - one max-poolng layer + one convolution layer
        - Output convolutions
            - Sequence of three RCUs
                - Add non-linear operations on the multi-path fused feature maps
