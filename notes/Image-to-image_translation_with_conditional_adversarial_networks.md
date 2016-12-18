# Image-to-Image Translation with Conditional Adversarial Networks

--------------------
### Introduction
- Conditional adversarial network as a general-purpose solution to image-to-image translation problems
- Demonstrate the approach is effective at synthesizing photos from label maps, reconstructing objects from edge maps, and colorizing images

### Method
- Expolore severl variants
    - Unconditional variant of GAN
    - Standard conditional GAN 
        - Learning a mapping from observed image x and random noise z to y
    - Standard conditional GAN + L1 loss (less blurring)
        - During the training, found that the generator ignore the noise z
        - Provide noise only in the form of dropout (applied on several layers of generator both trainng and test time). However, still obseved very minor stochasticitiy
- Network architecture
    - Use modules of the form convolution-BatchNorm-ReLu
    - Generator with skips
        - Add skip connections between each layer i and layer n-i
- Markovian discriminator (PatchGAN)
    - L2 (or L1) loss tends to produce blurry artifacts
    - Restricting the GAN discriminator to only model high-frequency structure (relying on an L1 term to force low-frequency correctnes)
        - Restrict our attention ot the structure in local image patches (PatchGAN)
        - Classify it each NxN patch in an image is real or fake
            - Run this discriminator convolutionally across the image, averaging all responses to provide the ultimate output
        - Discriminator can model the image as a Markov random field, assuming independence between pixels separate by more than a patch diameter
- Optimization and inference
    - Alternate between one gradient step on D, then one step on G
        - SGD + Adam solver
    - Use batch normalization using the statistics of the test batch rather than training one
        - "Instance noramlization" - demonstrated to be effective at image generation atasks
        - Batch size 1 or 4 (no big differences)

### Experiments
- Test various tasks
    - Semantic labels - photos (trained on Cityscapes)
    - Architectural labels - phots (CMP Facades dataset)
    - Map - aerial photo (Google Maps)
    - BW - color photos
    - Edge - photo
    - Sketch - photo
    - Day - night

- Evaluation metrics
    - Using AMT, real vs fake perceptual studies
    - Measure whether realistic enoguth that off-the-shelf recognition system can recognize the objects in them (cityscapes)
