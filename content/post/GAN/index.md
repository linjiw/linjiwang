---
title: When Cats meet GANs 
subtitle: A Comprehensive Study on DCGANs and CycleGANs with Advanced Augmentation Techniques
summary: In this assignment, we implemented two types of GANs - a Deep Convolutional GAN (DCGAN) and a CycleGAN. The DCGAN was trained to generate grumpy cats from random noise, while the CycleGAN was trained to convert between two types of cats (Grumpy and Russian Blue) and between apples and oranges. Both GANs were implemented with data augmentation and differentiable augmentation techniques.
authors:
- admin
tags:
- Computer Vision
- Image Generation
- Deep Learning
# - 开源

categories:
- Project
# - 教程
projects: []
date: "2019-02-05T00:00:00Z"
lastMod: "2019-09-05T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

design:
  spacing:
    # Customize the section spacing. Order is top, right, bottom, left.
    padding: ["20px", "0", "20px", "0"]
# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'DCGAN Results'
  focal_point: ""
  placement: 2
  preview_only: false
---
<!-- {{ <.TableOfContents> }} -->
{{< toc >}}
## Introduction


In this assignment, we get hands-on experience coding and training GANs. This assignment includes two parts:

Implementing a Deep Convolutional GAN (DCGAN) to generate grumpy cats from samples of random noise.
Implementing a more complex GAN architecture called CycleGAN for the task of image-to-image translation. We train the CycleGAN to convert between different types of two kinds of cats (Grumpy and Russian Blue) and between apples and oranges.

## Part 1: Deep Convolutional GAN

For the first part of this assignment, we implement a slightly modified version of Deep Convolutional GAN (DCGAN).

<!-- ### Implement Data Augmentation -->
<!-- Implemented the deluxe version of data augmentation in 'data_loader.py'. -->

<!-- ```python
elif opts.data_preprocess == 'deluxe':
        load_size = int(1.1 * opts.image_size)
        osize = [load_size, load_size]
        deluxe_transform = transforms.Compose([
        transforms.Resize(opts.image_size, Image.BICUBIC),
        transforms.RandomCrop(opts.image_size),
        transforms.RandomHorizontalFlip(),
        transforms.ToTensor(),
        transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5)),
        ])
        train_transform = deluxe_transform
    pass
```  -->

<!-- # <!-- ### Implement the Discriminator of the DCGAN
# (Answer for padding calculation goes here)

# Implemented the architecture by filling in the '__init__' and 'forward' method of the 'DCDiscriminator' class in 'models.py'. -->

<!-- # ```python
# def __init__(self, conv_dim=64, norm='instance'):
#     super().__init__()
#     self.conv1 = conv(3, 32, 4, 2, 1, norm, False, 'relu')
#     self.conv2 = conv(32, 64, 4, 2, 1, norm, False, 'relu')
#     self.conv3 = conv(64, 128, 4, 2, 1, norm, False, 'relu')
#     self.conv4 = conv(128, 256, 4, 2, 1, norm, False, 'relu')
#     self.conv5 = conv(256, 1, 4, 2, 0, None, False)

# def forward(self, x):
#     """Forward pass, x is (B, C, H, W)."""
#     x = self.conv1(x)
#     x = self.conv2(x)
#     x = self.conv3(x)
#     x = self.conv4(x)
#     x = self.conv5(x)
#     return x.squeeze()
# ``` --> -->

<!-- # <!-- ### Generator
# Implemented the generator of the DCGAN by filling in the '__init__' and 'forward' method of the 'DCGenerator' class in 'models.py'.

# ```python
# def __init__(self, noise_size, conv_dim=64):
#     super().__init__()

#     self.up_conv1 = conv(100, 256, 2, 1, 2, 'instance', False,'relu' )
#     self.up_conv2 = up_conv(256, 128, 3, stride=1, padding=1, scale_factor=2, norm='instance', activ='relu')
#     self.up_conv3 = up_conv(128, 64, 3, stride=1, padding=1, scale_factor=2, norm='instance', activ='relu')
#     self.up_conv4 = up_conv(64, 32, 3, stride=1, padding=1, scale_factor=2, norm='instance', activ='relu')
#     self.up_conv5 = up_conv(32, 3, 3, stride=1, padding=1, scale_factor=2, norm= None, activ='tanh')

# def forward(self, z):
#     """
#     Generate an image given a sample of random noise.

#     Input
#     -----
#         z: BS x noise_size x 1 x 1   --  16x100x1x1

#     Output
#     ------
#         out: BS x channels x image_width x image_height  --  16x3x64x64
#     """

#     z = self.up_conv1(z)
#     z = self.up_conv2(z)
#     z = self.up_conv3(z)
#     z = self.up_conv4(z)
#     z = self.up_conv5(z)
#     return z
# ```

# ### Training Loop
# Implemented the training loop for the DCGAN by filling in the indicated parts of the training_loop function in vanilla_gan.py.

# ```python
#             # TRAIN THE DISCRIMINATOR
#             # 1. Compute the discriminator loss on real images
#             if opts.use_diffaug:
#                 D_real_loss = torch.mean((D(DiffAugment(real_images, policy='color,translation,cutout', channels_first=False )) - 1) ** 2)
#             else:
#                 D_real_loss = torch.mean((D(real_images) - 1) ** 2)


#             # 2. Sample noise
#             noise = sample_noise(opts.batch_size, opts.noise_size)

#             # 3. Generate fake images from the noise
#             fake_images = G(noise)

#             # 4. Compute the discriminator loss on the fake images
#             if opts.use_diffaug:

#                 D_fake_loss = torch.mean((D(DiffAugment(fake_images.detach(), policy='color,translation,cutout', channels_first=False ))) ** 2)
#             else:
#                 D_real_loss = torch.mean((D(fake_images.detach())) ** 2)
#             D_total_loss = (D_real_loss + D_fake_loss) / 2

#             # update the discriminator D
#             d_optimizer.zero_grad()
#             D_total_loss.backward()
#             d_optimizer.step()

#             # TRAIN THE GENERATOR
#             # 1. Sample noise
#             noise = sample_noise(opts.batch_size, opts.noise_size)

#             # 2. Generate fake images from the noise
#             fake_images = G(noise)

#             # 3. Compute the generator loss
#             if opts.use_diffaug:
#                 G_loss = torch.mean((D(DiffAugment(fake_images, policy='color,translation,cutout', channels_first=False ))-1) ** 2)
#             else:
#                 G_loss = torch.mean((D(fake_images)-1) ** 2)

# ``` -->

<!-- #### Differentiable Augmentation
(Discussion of results with and without applying differentiable augmentations, and the difference between two augmentation schemes in terms of implementation and effects) -->

### Experiment with DCGANs
We've been experimenting with different data preprocessing techniques, and we've found that the choice of preprocessing can have a significant impact on the performance of the GAN. To demonstrate this, we've included screenshots of the training loss for both the discriminator and generator with two different preprocessing options: basic, deluxe and diff_aug.

#### grumpifyBprocessed_basic
{{< figure src="./data/grumpifyBprocessed_basic/sample-006400.png" title="sample: data_preprocess=basic, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_basic/D_fake_loss.png" title="D_fake_loss: data_preprocess=basic, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_basic/D_real_loss.png" title="D_real_loss: data_preprocess=basic, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_basic/D_total_loss.png" title="D_total_loss: data_preprocess=basic, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_basic/G_loss.png" title="G_loss: data_preprocess=basic, iter = 6400" >}}
#### grumpifyBprocessed_deluxe

{{< figure src="./data/grumpifyBprocessed_deluxe/sample-006400.png" title="data_preprocess=deluxe, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe/D_fake_loss.png" title="D_fake_loss: data_preprocess=deluxe, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe/D_real_loss.png" title="D_real_loss: data_preprocess=deluxe, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe/D_total_loss.png" title="D_total_loss: data_preprocess=deluxe, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe/G_loss.png" title="G_loss: data_preprocess=deluxe, iter = 6400" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe_diffaug/sample-006400.png" title="data_preprocess=deluxe, iter = 6400, diff_aug = True" >}}

#### grumpifyBprocessed_deluxe_diffaug

{{< figure src="./data/grumpifyBprocessed_deluxe_diffaug/D_fake_loss.png" title="D_fake_loss: data_preprocess=deluxe, iter = 6400, diff_aug = True" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe_diffaug/D_real_loss.png" title="D_real_loss: data_preprocess=deluxe, iter = 6400, diff_aug = True" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe_diffaug/D_total_loss.png" title="D_total_loss: data_preprocess=deluxe, iter = 6400, diff_aug = True" >}}
{{< figure src="./data/grumpifyBprocessed_deluxe_diffaug/G_loss.png" title="G_loss: data_preprocess=deluxe, iter = 6400, diff_aug = True" >}}

#### Results analysis

| Data Preprocessing         | Discriminator Loss                                      | Generator Loss                                           | Convergence Rate | Stability   |
|----------------------------|---------------------------------------------------------|----------------------------------------------------------|------------------|-------------|
| Basic                      | Slow decrease, potential instability                    | Fluctuates, struggles to generate realistic images       | Slow             | Less stable |
| Deluxe                     | Faster decrease, more effective at differentiation      | Converges more quickly, learns from more varied examples | Faster           | More stable |
| Differential Augmentations | Even faster decrease, more effective at differentiation | Faster generation of diverse and realistic images        | Fastest          | Most stable |

The table above highlights the key differences in the loss curves for a DCGAN trained with different data preprocessing techniques. Basic preprocessing techniques result in slower convergence rates and potentially less stable loss curves, while deluxe techniques result in faster convergence and more stable loss curves. The most effective approach is to use differential augmentations, where different augmentation policies are applied to real and fake images, resulting in the fastest convergence and the most stable loss curves. This analysis suggests that the choice of data preprocessing techniques can have a significant impact on the performance of a GAN, and careful consideration should be given to selecting the most effective approach.

## Part 2: CycleGAN
Implemented the CycleGAN architecture.

### Data Augmentation
Set the --data_preprocess flag to deluxe.

### Generator
Implemented the generator architecture by completing the __init__ method of the CycleGenerator class in models.py.

<!-- ```python
def __init__(self, conv_dim=64, init_zero_weights=False, norm='instance'):
    super().__init__()

    # # 1. Define the encoder part of the generator
    self.conv1 = conv(3, 32, 4, 2, 1, norm, False, 'relu')
    self.conv2 = conv(32, 64, 4, 2, 1, norm, False, 'relu')

    # # 2. Define the transformation part of the generator
    self.resnet_block = nn.Sequential(ResnetBlock(conv_dim = 64, norm = norm, activ = 'relu'),
                                      ResnetBlock(conv_dim = 64, norm = norm, activ = 'relu'),
                                      ResnetBlock(conv_dim = 64, norm = norm, activ = 'relu'),)

    # # 3. Define the decoder part of the generator
    self.up_conv1 = up_conv(64, 32, 3, stride=1, padding=1, scale_factor=2, norm='instance', activ='relu')
    self.up_conv2 = up_conv(32, 3, 3, stride=1, padding=1, scale_factor=2, norm= None, activ='tanh')

def forward(self, x):
    """
    Generate an image conditioned on an input image.

    Input
    -----
        x: BS x 3 x 32 x 32

    Output
    ------
        out: BS x 3 x 32 x 32
    """
    x = self.conv1(x)
    x = self.conv2(x)
    x = self.resnet_block(x)
    x = self.up_conv1(x)
    x = self.up_conv2(x)

    return x
``` -->
<!-- ### Training Loop
Implemented the training loop for the CycleGAN by filling in the indicated parts of the training_loop function in cycle_gan.py. -->

<!-- ```python
# TRAIN THE DISCRIMINATORS
# 1. Compute the discriminator losses on real images
if not opts.use_diffaug:
    D_X_loss = torch.mean((D_X(images_X) - 1) ** 2)
    D_Y_loss = torch.mean((D_Y(images_Y) - 1) ** 2)
else:
    D_X_loss = torch.mean((D_X(DiffAugment(images_X, policy='color,translation,cutout', channels_first=False )) - 1) ** 2)
    D_Y_loss = torch.mean((D_Y(DiffAugment(images_Y, policy='color,translation,cutout', channels_first=False )) - 1) ** 2)


d_real_loss = D_X_loss + D_Y_loss

# 2. Generate domain-X-like images based on real images in domain Y
fake_X = G_YtoX(images_Y).detach()

# 3. Compute the loss for D_X
if not opts.use_diffaug:

    D_X_loss = torch.mean((D_X(fake_X) ) ** 2)
else:
    D_X_loss = torch.mean((D_X(DiffAugment(fake_X, policy='color,translation,cutout', channels_first=False )) ) ** 2)


# 4. Generate domain-Y-like images based on real images in domain X
fake_Y = G_XtoY(images_X).detach()

# 5. Compute the loss for D_Y
if not opts.use_diffaug:

    D_Y_loss = torch.mean((D_Y(fake_Y) ) ** 2)
else:
    D_Y_loss = torch.mean((D_Y(DiffAugment(fake_Y, policy='color,translation,cutout', channels_first=False )) ) ** 2)


d_fake_loss = D_X_loss + D_Y_loss

# sum up the losses and update D_X and D_Y
d_optimizer.zero_grad()
d_total_loss = d_real_loss + d_fake_loss
d_total_loss.backward()
d_optimizer.step()

# plot the losses in tensorboard
logger.add_scalar('D/XY/real', D_X_loss, iteration)
logger.add_scalar('D/YX/real', D_Y_loss, iteration)
logger.add_scalar('D/XY/fake', D_X_loss, iteration)
logger.add_scalar('D/YX/fake', D_Y_loss, iteration)

# TRAIN THE GENERATORS
# 1. Generate domain-X-like images based on real images in domain Y
fake_X = G_YtoX(images_Y)

# 2. Compute the generator loss based on domain X
if not opts.use_diffaug:

    g_loss = torch.mean((D_X(fake_X) - 1 ) ** 2)
else:
    g_loss = torch.mean((D_X(DiffAugment(fake_X, policy='color,translation,cutout', channels_first=False )) - 1 ) ** 2)


logger.add_scalar('G/XY/fake', g_loss, iteration)

if opts.use_cycle_consistency_loss:
    # 3. Compute the cycle consistency loss (the reconstruction loss)
    cycle_consistency_loss = torch.mean((images_X - G_YtoX(G_XtoY(images_X)))**2)


    g_loss += opts.lambda_cycle * cycle_consistency_loss
    logger.add_scalar('G/XY/cycle', opts.lambda_cycle * cycle_consistency_loss, iteration)

# X--Y--X CYCLE
# 1. Generate domain-Y-like images based on real images in domain X
fake_Y = G_XtoY(images_X)

# 2. Compute the generator loss based on domain Y
if not opts.use_diffaug:

    g_loss += torch.mean((D_Y(fake_Y) - 1 ) ** 2)
else:
    g_loss += torch.mean((D_Y(DiffAugment(fake_Y, policy='color,translation,cutout', channels_first=False )) - 1 ) ** 2)

``` -->

### Experiment with CycleGAN

<!-- ``` -->
#### cat_10deluxe_instance_dc_cycle_naive

|  Title     |    Image      |
| :------: | :------: |
|sample X to Y |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/sample-001000-X-Y.png" >}}                     |
|sample Y to X |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/sample-001000-Y-X.png"  >}}                 |
|D_fake_loss |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/cat_10deluxe_instance_dc_cycle_naive/G_loss.png" >}}                     |
<!-- ``` -->

#### cat_10deluxe_instance_patch_cycle_naive

|  Title     |    Image      |
| :------: | :------: |
|sample X to Y |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/sample-001000-X-Y.png" >}}                     |
|sample Y to X |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/sample-001000-Y-X.png"  >}}                 |
|D_fake_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive/G_loss.png" >}}                     |
<!-- ``` -->

#### cat_10deluxe_instance_patch_cycle_naive_cycle

|  Title     |    Image      |
| :------: | :------: |
|sample X to Y |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/sample-001000-X-Y.png" >}}                     |
|sample Y to X |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/sample-001000-Y-X.png"  >}}                 |
|D_fake_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle/G_loss.png" >}}                     |
<!-- ``` -->

#### cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug

|  Title     |    Image      |
| :------: | :------: |
|sample X to Y |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-X-Y.png" >}}                     |
|sample Y to X |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-Y-X.png"  >}}                 |
|D_fake_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/G_loss.png" >}}                     |

<!-- #### apple2orange_10deluxe_instance_patch_cycle_naive_cycle

|  Title     |    Image      |
| :------: | :------: |
|sample X to Y |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/sample-001600-X-Y.png" >}}                     |
|sample Y to X |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/sample-001600-Y-X.png"  >}}                 |
|D_fake_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle/G_loss.png" >}}                     | -->


#### apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug

|  Title     |    Image      |
| :------: | :------: |
|sample X to Y |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-X-Y.png" >}}                     |
|sample Y to X |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-Y-X.png"  >}}                 |
|D_fake_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/apple2orange_10deluxe_instance_patch_cycle_naive_cycle_diffaug/G_loss.png" >}}                     |
<!-- ``` -->
#### Observations:

We observed that the results with the cycle-consistency loss were better than the results without it. The translations between the two domains were more accurate and realistic. This is because the cycle-consistency loss enforces the consistency between the two translations, which helps the model to learn better.

We also observed that the DCDiscriminator resulted in better quality translations than the PatchDiscriminator. This is because the DCDiscriminator has a larger receptive field, which enables it to capture more global features of the image.

#### Conclusion:

In conclusion, we have trained CycleGAN from scratch with and without the cycle-consistency loss, and have compared the results using the DCDiscriminator and the PatchDiscriminator. We have observed that the cycle-consistency loss and the DCDiscriminator resulted in better quality translations between the two domains. These observations can help in improving the translation quality between different domains in image processing applications.

## Bells & Whistles

### Implement and train a diffusion model

#### Training Diffusion Models with Hugging Face's Diffusers

#### Introduction

In this project, we train a simple diffusion model using the Hugging Face's Diffusers library. Diffusion models have become state-of-the-art generative models in recent times.

#### Key Parts of the Code

##### Configuration:

We define a 'TrainingConfig' class that holds all the training hyperparameters.
Hyperparameters include 'image_size', 'train_batch_size', 'eval_batch_size', 'num_epochs', 'gradient_accumulation_steps', 'learning_rate', and 'lr_warmup_steps', among others.
##### Data Preprocessing:

We use the datasets library to load our dataset and apply data transformations.
The dataset is preprocessed using the transforms.Compose function from torchvision.
The dataset is then transformed on-the-fly during training.
##### Model Definition:

We define our model using the 'UNet2DModel' class from the diffusers library.
The model has various hyperparameters such as 'sample_size', 'in_channels', 'out_channels', 'layers_per_block', 'block_out_channels', 'down_block_types', and 'up_block_types'.
<!-- ```python
from diffusers import UNet2DModel


model = UNet2DModel(
    sample_size=config.image_size,  # the target image resolution
    in_channels=3,  # the number of input channels, 3 for RGB images
    out_channels=3,  # the number of output channels
    layers_per_block=2,  # how many ResNet layers to use per UNet block
    block_out_channels=(128, 128, 256, 256, 512, 512),  # the number of output channes for each UNet block
    down_block_types=( 
        "DownBlock2D",  # a regular ResNet downsampling block
        "DownBlock2D", 
        "DownBlock2D", 
        "DownBlock2D", 
        "AttnDownBlock2D",  # a ResNet downsampling block with spatial self-attention
        "DownBlock2D",
    ), 
    up_block_types=(
        "UpBlock2D",  # a regular ResNet upsampling block
        "AttnUpBlock2D",  # a ResNet upsampling block with spatial self-attention
        "UpBlock2D", 
        "UpBlock2D", 
        "UpBlock2D", 
        "UpBlock2D"  
      ),
) -->
<!-- ``` -->
<!-- ##### Noise Scheduler:

We use the 'DDPMScheduler' class from the diffusers library to define the noise scheduler for our model.
The scheduler takes a batch of images, a batch of random noise, and the timesteps for each image.
```python
from diffusers import DDPMScheduler

noise_scheduler = DDPMScheduler(num_train_timesteps=1000)
``` -->
#### Training Setup:

We use an AdamW optimizer and a cosine learning rate schedule for training.
We use the DDPMPipeline class from the diffusers library for end-to-end inference during evaluation.
The training function train_loop is defined, which includes gradient accumulation, mixed precision training, and multi-GPU or TPU training using the Accelerator class from the accelerate library.
<!-- 
```python
for step, batch in enumerate(train_dataloader):
    clean_images = batch['images']
    # Sample noise to add to the images
    noise = torch.randn(clean_images.shape).to(clean_images.device)
    bs = clean_images.shape[0]

    # Sample a random timestep for each image
    timesteps = torch.randint(0, noise_scheduler.num_train_timesteps, (bs,), device=clean_images.device).long()

    # Add noise to the clean images according to the noise magnitude at each timestep
    # (this is the forward diffusion process)
    noisy_images = noise_scheduler.add_noise(clean_images, noise, timesteps)
    
    with accelerator.accumulate(model):
        # Predict the noise residual
        noise_pred = model(noisy_images, timesteps, return_dict=False)[0]
        loss = F.mse_loss(noise_pred, noise)
        accelerator.backward(loss)

        accelerator.clip_grad_norm_(model.parameters(), 1.0)
        optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
            
```
#### Training Execution: -->

We use the 'notebook_launcher' function from the accelerate library to launch the training from the notebook.
#### Key Functions

*transform(examples)*: Applies the image transformations on the fly during training.
*evaluate(config, epoch, pipeline)*: Generates a batch of sample images during evaluation and saves them as a grid to the disk.
*train_loop(config, model, noise_scheduler, optimizer, train_dataloader, lr_scheduler)*: The main training loop, which includes the forward diffusion process, loss calculation, and backpropagation.
#### Diffusion Results

|  Title     |    Image      |
| :------: | :------: |
|Apple |{{< figure src="./data/difussion/apple.png" >}}                     |
|Cat |{{< figure src="./data/difussion/cat.png"  >}}                 |

The quality of the generated images and how well the DCGAN has captured the main differences between the two domains depend on factors such as the quality of the training data, hyperparameters used during training, and complexity of image domains. If the diffusion results look unrealistic compared to the DCGAN results, it could be due to factors such as dataset quality, model complexity, hyperparameter tuning, or training time. Further analysis and experimentation would be necessary to pinpoint the specific reason for the difference in image quality.
<!-- |D_fake_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_fake_loss.png" >}}                     |
|D_real_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_real_loss.png" >}}                     |
|D_X_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_X_loss.png" >}}                     |
|D_Y_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_Y_loss.png" >}}                     |
|G_loss |{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/G_loss.png" >}}                     | -->

<!-- Sample images generated during evaluation can be inserted here.
Training metrics such as loss and learning rate can be inserted here. -->
<!-- 
#### cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug

{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-X-Y.png" title="sample X to Y: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-Y-X.png" title="sample Y to X: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_fake_loss.png" title="D_fake_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_real_loss.png" title="D_real_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_X_loss.png" title="D_X_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_Y_loss.png" title="D_Y_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/G_loss.png" title="G_loss: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}} -->

<!-- #### cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug

{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-X-Y.png" title="sample X to Y: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-Y-X.png" title="sample Y to X: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_fake_loss.png" title="D_fake_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_real_loss.png" title="D_real_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_X_loss.png" title="D_X_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_Y_loss.png" title="D_Y_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/G_loss.png" title="G_loss: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}} -->

<!-- #### cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug

{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-X-Y.png" title="sample X to Y: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/sample-010000-Y-X.png" title="sample Y to X: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_fake_loss.png" title="D_fake_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_real_loss.png" title="D_real_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_X_loss.png" title="D_X_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/D_Y_loss.png" title="D_Y_loss: data_preprocess=deluxe, iter = 10000" >}}
{{< figure src="./data/cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug/G_loss.png" title="G_loss: cat_10deluxe_instance_patch_cycle_naive_cycle_diffaug, iter = 10000" >}} --> -->

<!-- 
(Brief comment on the quality of the generated images, and whether the CycleGAN has captured the main differences between the two domains)

INSERT IMAGE: Two example images of generated apples from oranges, and two example images of generated oranges from apples.

(Brief comment on the quality of the generated images, and whether the CycleGAN has captured the main differences between the two domains) -->

## Conclusion
This report presents our implementation of DCGAN and CycleGAN for various image generation tasks. Through these experiments, we have observed the impact of data augmentation and differentiable augmentation on the training process and final results. We have also seen the capabilities of CycleGAN in generating realistic images for domain-to-domain translation tasks, such as converting Grumpy cats to Russian Blue cats and vice versa, and converting apples to oranges and vice versa.
