
# Text-to-Image Synthesis using Imagen 

## Introduction

The primary objective of this project was to develop a text-to-image generator, leveraging the power of Google's [Imagen](https://imagen.research.google/) and [MinImagen](https://www.assemblyai.com/blog/minimagen-build-your-own-imagen-text-to-image-model/). Imagen is a cutting-edge diffusion model that translates text into images with remarkable photorealism and a deep understanding of language. The underlying principle of Imagen hinges on large transformer language models, which are effective at encoding text for image synthesis. A key discovery in the development of Imagen was that increasing the size of the language model boosts both sample fidelity and image-text alignment, surpassing the effects of increasing the size of the image diffusion model itself.

At the heart of Imagen lies a large, frozen T5-XXL encoder that encodes the input text into embeddings. A conditional diffusion model then maps these text embeddings into a 64x64 image. To achieve higher resolution, Imagen further employs text-conditional super-resolution diffusion models to upsample the image from 64x64 to 256x256 and then from 256x256 to 1024x1024.


## Model Training

One of the notable characteristics of this implementation was the quick training process, despite the relatively slow image output, and the ability to quickly converge. The results generated by the model were found to be meaningful and visually engaging.

## Implementation Details

In this project, the model was trained using the Hugging Face Pokemon dataset, requiring modifications to the data loader and dataset. The model was too large to fit into a 12GB GPU memory for training, even with a batch size of 1. To resolve this issue, the super-resolution model was removed, which made it possible to train the model effectively. The training process was recorded using Weights and Biases (wandb).

The model quickly converged in 300 epochs with a batch size of 2 and a time step of 1000, and was capable of generating intriguing images. The training of the super-resolution layer was also tested as part of this project.

## Diffusion Models

<!-- ## Diffusion Models -->

Diffusion models are a class of generative models that are used to generate new data, often images. These models function by introducing Gaussian noise to training images over a sequence of timesteps, and then learning to reverse this noising process. A model is trained to predict the noise component of an image at a particular timestep. Once trained, this denoising model can then be iteratively applied to randomly sampled Gaussian noise, "denoising" it in order to generate a novel image.

In the context of the Imagen model, a pretrained T5 text encoder is used to generate an encoding of the input text. This encoding is then utilized to condition an image generator. The image generator begins with Gaussian noise (analogous to "TV static") and progressively denoises it to create a small image that corresponds to the scene described by the text prompt. Subsequently, two super-resolution models sequentially upscale the image to higher resolutions, again conditioning on the encoding information. Both the base image generation model and the super-resolution models used in Imagen are diffusion models.


## Conclusion

This project showcases the power of text-to-image synthesis using diffusion models. Despite initial challenges with GPU memory limitations, the modified model was capable of generating high-quality images, demonstrating the potential of this technology in numerous applications.

_This report is a work in progress and will continue to be updated as more information and results become available._