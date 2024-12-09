# AI-Powered Image Colorization for Enhanced Visuals

## Description

This project employs the VGG16 architecture for feature extraction with an emphasis on image colourization. LAB colour space is used to fine-tune the algorithm to forecast colour channels (AB) from greyscale images. In order to create the colourized images, the pipeline consists of preprocessing the dataset, feature extraction using a VGG16 model that has already been trained, and a custom decoder network.

## Developers:

- Isha Paliwal (GWID: G49146952)
- Needhi Kore (GWID: G20475943)

## Dataset

* The dataset contains two classes:
  - **Class 0:** Colored images.
  - **Class 1:** Grayscale images.
* Images are resized to **256x256 pixels** and normalized.
* LAB color space is utilized, with the L (lightness) channel as input and AB (color) channels as output for training.

## Installation Instructions

1. Clone this repository to your local machine.
2. Install the required dependencies.
3. Ensure your dataset is structured as follows:
    - dataset/: The root directory containing the entire dataset, containing folders 'class 0' and 'class 1'.
    - class 0/: This folder contains all the colored images, used as one class in the model.
    - class 1/: This folder contains all the grayscale images, used as the other class in the model.

## Preprocessing

The preprocessing stage involves preparing the dataset for training and testing:

**1. Data Loading:**
  - The 'class 0' directory of the dataset is used to load coloured images.
  - To make greyscale photos compatible with the VGG16 model, they are transformed to 3-channel RGB format after being loaded from the 'class 1' directory.

**2. Resizing and Normalization:**
  - To comply with the VGG16 model's input size requirements, all photos are downsized to 256x256 pixels.
  - To improve training performance, pixel values are normalised to the interval [0, 1].

**3. Color Space Conversion:**
  - RGB to LAB colour space conversion is performed on images.
  - The model uses the retrieved and normalised **L channel** (lightness) as input.
  - The **AB channels** (color information) are normalized to the range [-1, 1] and used as the ground truth for model training.

**4. Data Augmentation:**
  - To improve model generalisation and diversify datasets, data augmentation techniques including rotation, horizontal flip, and vertical flip are used.

## Model Training

**1. Feature Extraction:**
  - The VGG16 model, pre-trained on ImageNet, is used as the encoder.
  - To avoid updates during training, only the first 19 layers of the VGG16 are utilised, and these layers are frozen.
  - Grayscale images (L channel) are converted to 3-channel RGB format and passed through the VGG16 model to extract features.

**2. Decoder Architecture:**
  - Using the attributes that VGG16 extracted, a unique custom decoder network is created to recreate the AB colour channels.
  - The decoder gradually restores the AB channels to their original resolution (256x256 pixels) using up-sampling layers and transposed convolutions.

**3. Training Process:**
  - **Input:** L channel features extracted by the VGG16 encoder.
  - **Output:** AB channels.
  - **Loss Function:** Mean Squared Error (MSE) to measure the difference between the predicted and actual AB channels.
  - **Optimizer:** Adam optimizer with a learning rate of 0.0002.
  - **Batch Size:** 32.
  - **Epochs:** 50.

## Image Colorization

**1. Test Image Processing:**
  - The greyscale test image is normalised to [0, 1] and scaled to 256x256 pixels.
  - The L channel is retrieved after it has been transformed to LAB colour space.

**2. Feature Extraction:**
  - To extract features, the L channel is transformed to 3-channel RGB and run through the VGG16 encoder.

**3. AB Channel Prediction:**
  - To predict the AB channels, the extracted features are fed into the trained decoder network.
  - The predicted AB channels are denormalized to their original scale.

**4. Image Reconstruction:**
  - The LAB image is created by combining the original L channel with the anticipated AB channels.
  - 'lab2rgb' is then used to convert the LAB picture back to RGB format.

**5. Visualization:**
  - For comparison, the colourized image is shown next to the original greyscale input.

## Model Details

**1. Encoder (VGG16):**
  - The VGG16 model is used to extract high-level features from the grayscale L channel.
  - The encoder outputs feature maps with a shape of (8, 8, 512).

**2. Decoder:**
  - The decoder is a fully convolutional network designed to up-sample the feature maps from the encoder and predict the AB channels.
  - Key components:
    - **Conv2DTranspose Layers:** These layers up-sample the feature maps by performing transposed convolution.
    - **UpSampling2D Layers:** These layers double the spatial dimensions of the feature maps.
    - **Activation Functions:** ReLU activation is used for intermediate layers, and Tanh is used for the final output layer to match the range of the AB channels.

**3. Input and Output Shapes:**
  - **Input:** (256, 256, 1) – Grayscale L channel.
  - **Output:** (256, 256, 2) – Predicted AB channels.

**4. Training Strategy:**
  - The encoder layers are frozen to leverage pre-trained weights, ensuring that the model focuses on learning the mapping between the L channel and the AB channels.

## Results

The following images showcase the input grayscale images and the corresponding colorized outputs generated by the model:

![Untitled Project](https://github.com/user-attachments/assets/2f77177a-1d20-4818-83dd-a542698182d8)


## Future Work

**1. Utilizing Larger and More Diverse Datasets**
  - The model's capacity to generalise and correctly colourize a range of input photos can be enhanced by enlarging the training dataset to encompass a wider range of images (such as objects, animals, and landscapes). Prior to fine-tuning on domain-specific images, the model's initial performance may be enhanced by pre-training it on a sizable colourization dataset.
**2. Enhancing Colorization Quality with Generative Models**
  - The existing results show noticeable artefacts and partial colourization. The realism and brilliance of the generated colours could be further enhanced by including sophisticated generative models, such as **Generative Adversarial Networks (GANs)**. Complex colour distributions can be effectively learnt by GANs, particularly Conditional GANs, producing more realistic and aesthetically pleasing outcomes.
**3. Refining the Decoder Architecture**
  - By adding **attention mechanisms** to concentrate on particular areas of the image that need in-depth colourization, the decoder network can be further optimised. Furthermore, experimenting with more complex or specialised designs (like U-Net) might improve the mapping of greyscale data to colours, leading to noise reduction and smoother transitions.
