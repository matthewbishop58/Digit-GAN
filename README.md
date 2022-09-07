# Digit Creation with GANs
Generative Adversarial Networks (GANs) were developed by Ian J. Goodfellow in 2014. They can be used to generate complex colour images such as faces. Images created by GANs can look so real that it is practically impossible to distinguish between real and fake images. However, generating complex images of these level of realism requires a large amount of resources to train the network.

### GAN - General Adversarial Network
In a GAN two neural networks, a generator and a discriminator are trained simultaneously by an adversarial process. The generator (artist) learns to create images that look real. The discriminator (critic) learns to detect fake images. The two competing models try to beat each other. The goal is to train the generator to outperform the discriminator.

## Table of Contents
* [How does GAN work?](#how-does-gan-work)
* [Model architecture](#model-architecture)
* [Defining loss functions](#defining-loss-functions)
* [Model training](#model-training)
* [Summary](#summary)
* [Acknowledgements](#acknowledgements)
* [Connect with me](#connect-with-me)
<a name="how-does-gan-work"></a>
## How does GAN work?
Training a GAN consists of two parts:
1.	While the generator remains idle, the discriminator is trained on real images for a number of epochs to see if it can correctly predict them as real. In the same phase the discriminator is trained on fake images to (generated by the generator) to see if it can predict them as fake.
2.	While the discriminator is idle, train the generator and use the results from the discriminator to improve the images.
These steps are repeated for a large number of epochs. The results (fake images) are examined manually to determine if they look real. If they look real the training is stopped. If not the preceding two steps are repeated until the fake images appears real. This process is depicted in **Figure 1**.

#### Figure 1 - GAN working schematic
![image](https://user-images.githubusercontent.com/94609839/188701299-bfa198fe-0ac1-4de6-8b27-833fc4ee4c0f.png)
Source: Sarang(2021)
<a name="model-architecture"></a>
## Model Architecture

### The Generator
The generator schematic is shown in **Figure 2**.
The generator takes a random noise vector of a given dimension, in this example a dimension of 100 is used. Using this vector, an image of 64x64x3 is generated. The image is upscaled in a series of transitions through convolutional layers. Each convolutional layer is followed by a batch normalisation and leaky ReLU. The leaky ReLU has neither the dying ReLU or vanishing gradient problems. Strides are used in each convolutional layer to avoid unstable training.

#### Figure 2 - Generator architecture
![image](https://user-images.githubusercontent.com/94609839/188701598-507bd723-22a3-4165-abfe-1f8646af73eb.png)
Source: Sarang(2021)

### The Discriminator
The discriminator schematic is shown in **Figure 3**.
The discriminator downsizes the given image for evaluation using convolutional layers.

#### Figure 3 - Discriminator schematic
![image](https://user-images.githubusercontent.com/94609839/188701772-da88e326-a643-4817-ab86-f8c66828da94.png)
Source: Sarang(2021)

## Model Architecture
### Defining the Generator
The purpose of the generator is to create images containing the digit 5 which look similar to the images in the training dataset.
A Keras sequential model is used to create the generator, show in **Figure 4**. A summary of the generator model is shown in **Figure 5**.

#### Figure 4 - Generator architecture
![image](https://user-images.githubusercontent.com/94609839/188702120-1b9724ed-72dd-41b9-9733-2b93d63878c5.png)

#### Figure 5 - Generator model summary
![image](https://user-images.githubusercontent.com/94609839/188702186-fee31ba3-3955-499a-9cb3-7177dc696277.png)

### Testing the Generator
Test output from the generator is shown in **Figure 6**.

#### Figure 6 - Test Generator output
![image](https://user-images.githubusercontent.com/94609839/188702344-10e22da4-a5cb-4d91-b3ca-4326790d9454.png)

### Defining the discriminator model
The discriminator model uses just two convolutional layers. The output of the last convolutional layer is of type (batch size, height, width, filters). The Flatten layer in the network flattens this output to feed it into the last Dense layer in the network.

#### Figure 7 - Discriminator model architecture
![image](https://user-images.githubusercontent.com/94609839/188702471-020251bc-f17a-4b50-a69b-183156c281bb.png)

#### Figure 8 - Discriminator model summary
![image](https://user-images.githubusercontent.com/94609839/188702531-79723fe0-2294-4278-81aa-e285541d7acb.png)

### Testing the Discriminator
The discriminator can be tested by feeding it with the earlier generated image.
The discriminator will give a negative value of the image is fake and a positive value if it is real. 

#### Figure 9 - Discriminator test output

![image](https://user-images.githubusercontent.com/94609839/188702614-007f659c-526c-4ef1-a73e-17d000566793.png)

The decision value is -0.0014415, a negative value indicating that the image is fake. 
<a name="defining-loss-functions"></a>
## Defining Loss Functions
Keras’s binary cross-entropy is used for the loss function as there are two classes (1) for a real image and (0) for a fake one.  The return value of the function is how well the generator tricks the discriminator. If the generator performs well, the discriminator will classify the fake image as real, returning a decision of 1. The function compares the discriminators decision on the generated image with an array of ones.
The discriminator first considers the image is real and computes the loss with respect to an array of ones. The discriminator then considers the image is fake and computes the loss with respect to an array of zeros. The total loss determined by the discriminator is the sum of the two losses.
<a name="model-training"></a>
## Model Training
The generator and discriminator models are trained in several steps. Gradient tape is used for automatic differentiation on both the generator and the discriminator. 

At each step, a batch of images is given to the function as an input. The discriminator is asked to produce outputs for both the training and generated images. The training output is called as real and the generated image as fake. The generator loss is calculated on the fake image, and the discriminator loss on the real and fake. Gradient tape is used to compute the gradients on both of these losses and apply new gradients to the models.

#### Figure 10 - Images for the digit 5 generated by the GAN
![image](https://user-images.githubusercontent.com/94609839/188951693-a5ddf37a-3ff5-45cc-8894-c6f326f49031.png)
<br>
The GAN can create an acceptable output after just 20 / 30 epochs with better quality above 70 epochs.
<a name="summary"></a>
## Summary
Generative Adversarial Networks (GANs) provide a methods for imitating given images. GANs consist of two networks - a generator and discriminator which are trained simultaneously in an adversarial process. In this repo a GAN network was constructed and trained on handwritten digits, alphabets and anime characters. Training a GAN can require huge resources, but the results can be impressive. GANs have been successfully applied to many applications from generating images for large datasets, creating celebrity faces and generating emojis from photos.
<a name="acknowledgements"></a>
## Acknowledgements
Artificial Neural Networks with TensorFlow 2, ANN Architecture Machine Learning Projects (2021) Poornachandra Sarang
<a name="connect-with-me"></a>
## Connect with me
*	Visit my [website](https://www.matthewbishop.info) 💻
*	Follow me on [LinkedIn](https://www.linkedin.com/in/matthewbishop58/) and [Twitter](https://twitter.com/Matthewbishop58)) 🐤
*	Follow me on [Medium](https://medium.com/@matthewbishop58) 📕

