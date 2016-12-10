# Speed/accuracy trade-offs for modern convolutional object detectors

--------------------
### Introduction
- Study the trade-off between accuracy and speed (Faster R-CNN, R-FCN and SSD)
    - Unified framework (in Tensorflow) of the "meta-architectures"
    - Analyze the performance of many variations
        - Found the set of models which achieve different points on the speed-accuracy trade off (from the suitable model for use on a mobile phone to a new state of the art model on the COCO detection challenge)
- Faster R-CNN can be gained speed up significantly using fewer proposal without big loss in accuracy, which making it competitive with its cousins (SSD, RFCN)
- SSDs performance is less sensitive to the quality of the feature extractor than Faster R-CNN and R-FCNN
 
### Prior work (Meta-architectures)
- Single Shot Detector (SSD)
    - Single feed-forward CNN to directly predict classes and anchor offsets without requiring a second stage per-proposal classification operation
- Faster R-CNN 
    - Two stage detection 
        - Region proposal network (RPN) - features are extracted using a network (VGG-16) and features at some selected intermediate level (e.g., "conv5") are used to predict class insensitive box proposals
        - Box classifiers - Box proposals (about 300) are used to crop features from the the same intermediate feature map and fed to the remainder of the feature extractor 
    - Computation time is depends on the number of region proposed by the RPN
- R-FCN
    - Similar to Faster R-CNN but instead of cropping features from the same layer (where region proposals are predicted), crops are taken from the last layer of features prior to prediction
    - R-FCN model (using Resnet 101) could achieve comparable accuracy to Faster R-CNN

### Summary of analysis of experiments 
- Accuracy vs. time
    - Fastest : SSD w/MobileNet : SSD models with Inception v2 and Mobilenet feature extractors are most accurate of the fastest models 
    - Sweet spot : R-FCN w/Resnet or Faster R-CNN w/Resnet and only 50 proposals
    - Most accurate : Faster R-CNN w/Inception Resnet at stride 8 - the state of the art single model performance
- The effect of the feature extractor
    - High correlation with Faster R-CNN and R-FCN / Less reliant to SSD performances
- The effect of object size 
    - SSD models typically have (very) poor performance on small objects / competitive with Faster R-CNN and R-FCN on large objects
- The effect of # of proposals 
    - Sweet spot of the region proposal based detector : 50 proposals (about 96% of the accuracy of using 300 proposals)
- Good localization at .75 IoU means good localization at all IoU thresholds
- State of the art detection on COCO
    - Ensemble detector    
        - Models are selected greedily on a validation set (choose R-CNN models with Resnet and Inception Resnet)
        - Varying output stride configurations, retrained using variations on the loss function, and different random orderings of the training data
        - Explicitly encourating diversity by pruning away models that are too close 
        - To do so, compute the vector of average precision results across each COCO category for each model and declared two models are similar if their category-wise AP vertors had cosine distance greater than some threshold
