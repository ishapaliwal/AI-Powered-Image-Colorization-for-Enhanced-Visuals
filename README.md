# Image Colorization: From Grayscale to Vibrant Visuals
_Bringing Life to Grayscale: Teaching Machines the Art of Color._

## Professor/s:

Robert Pless

## Developers:

Isha Paliwal (GWID: G49146952)
Needhi Kore (GWID: G20475943)

## Introduction

Image colorization is a fascinating computer vision task where we transform grayscale images into vibrant, colored ones. Leveraging modern deep learning techniques, this blog explores three approaches: VGG16, EfficientNet with U-Net, and Pix2Pix GANs. We also discuss the importance of dataset selection and how refining the dataset significantly improved the results.

## Data Collection

**_Why Data Collection Matters_**

The COCO dataset is an excellent starting point due to its diversity and large scale. However, for a model to excel at specific tasks like identifying the colors of particular items, a targeted dataset is crucial. This section outlines the steps to prepare and augment the data used for training the models.

**_Steps for Data Preparation_**

1. **Downloading COCO Dataset**
  - **Annotations File:** Download the COCO dataset annotations using the code in the first block of Data_Collection.ipynb. This file provides metadata for the images, such as object bounding boxes and categories.
  - **Images:** Use the second block of code to download the corresponding images based on the annotations. The images are stored in the colored_images directory.
2. **Targeted Dataset**
  - Download the **targeted dataset** from [this link](https://drive.google.com/file/d/1iOt3PCIrLGXMtbNN-p-PdV6OFz-KyqLp/view?usp=drive_link). This is a zipped folder containing selected images of specific objects like:
    - Animals: **Indian Peacocks, Common Kingfishers, Elephants, Zebras**.
    - Objects: **Basketball, Rubber Duck, Stop Sign**.
    - Others: **Egg sunny-side up, Orange, Red Ferrari, Rose**.
Extract the zipped folder into a directory called selected_images.

3. **Augmentation**
  - Run the third block of code in Data_Collection.ipynb to apply data augmentation on images in the selected_images folder. The augmentations include:
    - Rotations.
    - Brightness and contrast adjustments.
    - Flipping (horizontal and vertical).
  - The augmented images are saved in a new folder named filtered_images.

4. **Combining Datasets**
  - Copy all images from the filtered_images directory into the colored_images directory to create a unified dataset for training.
5. **Generating Grayscale Images**
  - Run the fourth block of code to convert all colored images in the colored_images directory into grayscale images. The grayscale images are saved in a directory called grayscale_images.

_**Importance of Targeted Data**_
While the COCO dataset provides a diverse set of training examples, it may not sufficiently train the model to recognize and colorize specific items accurately. By creating a targeted dataset, we taught the models to associate specific objects with their correct colors, improving performance and reducing generalization errors, such as overly green tints.

**Methodologies**

We began our journey with VGG16, a simple and widely used model for feature extraction, and progressively advanced to Pix2Pix GAN, leveraging its generative capabilities to achieve high-quality and realistic colorization results.

## 1. VGG16
  - **Architecture:** A pre-trained convolutional neural network originally designed for classification.
  - **Dataset Preparation:**
    - To make VGG16 work for the colorization task, the dataset was reorganized into a **parent directory "dataset"** with two child directories:
      - **Class 0:** Contained all the colored images (from colored_images).
      - **Class 1:** Contained all the grayscale images (from grayscale_images).
  - **Results:**
    - Struggled to adapt to the image colorization task.
    - Produced overly muted and inaccurate colors.
  - **Key Observation:** VGG16, despite its power in feature extraction, isn't inherently suited for generative tasks like colorization.
  - **Code Implementation:** VGG16 for Grayscale to RGB.ipynb

## 2. EfficientNet with U-Net
  - **Architecture:** Combined **EfficientNet** for efficient feature extraction and **U-Net** for capturing spatial details through skip connections.
  - **Training:**
    - Initial dataset: COCO (a wide variety of images). Results were suboptimal.
    - Refined dataset: Specific objects such as **basketball, beach, elephants, peacocks, and zebras**. Results improved dramatically.
  - **Results:**
    - Accurately identified broad regions like sky and grass.
    - However, tended to colorize most objects with a greenish tint.
  - **Key Observation:** Dataset specificity and U-Net’s spatial detail preservation led to better but still limited results.
  - **Code Implementation:** EfficientNet_with_UNet.ipynb

## 3. Pix2Pix GAN
  - **Architecture:** A Generative Adversarial Network (GAN) framework with a U-Net-like generator and PatchGAN discriminator.
  - **Training:** Fine-tuned on the refined dataset.
  - **Results:**
    - Significantly better at generating realistic and diverse colors.
    - High fidelity in distinguishing objects like **clownfish, kingfishers, and Ferrari cars**.
  - **Key Observation:** The adversarial training mechanism of GANs resulted in superior realism compared to traditional CNN architectures.
  - **Code Implementation:** pix2pix_implementation.ipynb

## Dataset Refinement: The Turning Point

The COCO dataset, while diverse, posed challenges for colorization due to its broad range of objects and scenes. A more selective dataset with specific classes improved model performance. Objects included:

- Animals: **Indian Peacocks, Common Kingfishers, Elephants, Zebras.**
- Objects: **Basketball, Rubber Duck, Stop Sign.**
- Scenes: **Beaches.**
- Others: **Egg sunny-side up, Orange, Rose, Red Ferrari.**
The refined dataset enabled the models to focus on specific object features, significantly boosting accuracy and color vibrancy.

## Visual Results

![Real_Image_vs_VggModel_Colorization](https://github.com/user-attachments/assets/7bdadee0-5cba-47c9-ac04-ad348dc057bb)

1. **VGG16 Model Performance:** The VGG16 model served as the **baseline** for image colorization due to its simplicity and pre-trained feature extraction capabilities. Below are the **observations** for its behavior:
  - **Results Directory:** VGG_Resultant_images directory
  - **Color Representation:**
    - The model struggled to reproduce **accurate colors** for objects, resulting in dull or grayscale-like outputs.
    - In many cases, colors appear desaturated or inconsistent, with noticeable lack of vibrancy.
    - For example:
      - The **red Ferrari** is faintly tinted but largely dull.
      - **Orange fruits** remain almost grayscale with very little color detected.
  - **Inability to Capture Context:**
    - VGG16 could not capture broader image contexts, such as distinguishing between **foreground and background**.
    - Complex items like the **blue tang fish** and **stop sign** failed to retain their true colors, with outputs appearing washed out or incorrect.
  - **Performance on Textured Objects:**
    - Simple structures like the **rubber duck** or **zebras** retain some form of object identification, but the textures lacked proper colorization.
    - For example, while the zebra pattern is visible, there is no realistic contrast between black and white stripes.
  - **Overall Observations:**
    - The model shows its limitations as a **feature extractor** rather than a generative model.
    - Its outputs lack sharpness, detail, and realistic color distributions, as VGG16 alone is not well-suited for **generative tasks** like image colorization.

![Real_Image_vs_EfficientNet_with_UNet_Colorization](https://github.com/user-attachments/assets/d4d9c834-4720-4146-a01f-e2765d67ede1)

2. **EfficientNet with U-Net Performance:** The EfficientNet with U-Net architecture improves upon the limitations of VGG16 by leveraging EfficientNet for feature extraction and U-Net for preserving spatial details. Below are the observations for its performance:
  - **Results Directory:** EfficientNet_with_UNet_Resultant_images directory
  - **Improved Colorization:**
    - The model demonstrates better **color coverage** compared to VGG16, with noticeable improvements in identifying major regions:
    - The **stop sign** retains its red hue.
    - **Clownfish** and **blue tang** fish have partial recognition of their respective vibrant colors.
    - However, the colors still appear **muted and washed out** rather than vivid and realistic.
  - **Better Object Recognition:**
    - The model shows an improved ability to recognize objects:
    - The **rubber duck** retains its yellow tone, unlike the grayscale VGG16 result.
    - **Zebras** and **peacocks** have some identifiable patterns and colors, albeit lacking sharpness.
    - Backgrounds like **grass** and **water bodies** are partially colorized but appear dull and less saturated.
  - **Context Understanding:**
    - The model starts capturing **broader contexts** of the images, such as distinguishing between **foreground objects** and **background scenes**.
    - For instance:
      - The **beach scene** shows faint recognition of the blue sky and water.
      - The **red Ferrari** retains some reddish tint, although less pronounced.
  - Challenges and Shortcomings:
    - While EfficientNet with U-Net significantly improves object recognition, it still struggles with:
      - Producing vibrant and saturated colors.
      - Complex textures like the **orange fruits** and **peacock feathers**, where the colors are inconsistent.
    - Overall, the outputs remain slightly **hazy and blurry**, lacking the sharpness seen in the real images.

![Real_Image_vs_Pix2Pix_Colorization](https://github.com/user-attachments/assets/03992f75-47a2-4c1d-b92a-68e5fa2a094e)

2. **Pix2Pix GAN Performance:** The Pix2Pix GAN, a generative adversarial network, significantly outperforms the previous models (VGG16 and EfficientNet with U-Net). By leveraging adversarial training, it achieves more realistic, vibrant, and context-aware colorization. Below are the observations for its performance:
  - **Results Directory:** Pix2Pix_Resultant_images diretory
  - **Color Accuracy and Vibrancy**
    The Pix2Pix model produces **rich and vibrant** colors that closely match the real images.
      - The **red Ferrari** is accurately colorized with vivid red tones.
      - The **orange fruits** display realistic orange hues, unlike the muted results from previous models.
      - The **stop sign** retains its prominent red color and clarity.
    - Objects like the **blue tang fish and clownfish** are particularly impressive, showcasing their distinct **blue, orange, and white patterns** accurately.
  - **Contextual Understanding**
    - The model demonstrates an improved ability to understand **context**:
      - The **beach scene** is well colorized, with clear blue skies and vibrant water tones.
      - The **peacock** and **elephants** retain natural-looking colors with appropriate blending into their backgrounds.
      - The **zebras** are clearly defined, with sharp black-and-white stripes and realistic green grass surroundings.
    - Backgrounds, which were a challenge for VGG16 and EfficientNet, now exhibit **clear distinctions** between foreground objects and surrounding scenery.
  - **Sharpness and Detail**
    - The Pix2Pix GAN produces outputs with better **sharpness** and **fine detail**, capturing textures and contrasts that were previously lost:
      - The **egg yolk** on the breakfast plate is vibrant and distinct.
      - The **rubber duck** is rendered with bright yellow tones and reflective water details.
    - These improvements are particularly visible in images with **high texture and fine features** like animals, objects, and food items.
  - **Minor Artifacts**
    - Despite its overall superior performance, the model occasionally introduces **artifacts** or **color bleeding**:
      - Small red spots are noticeable on the **blue tang fish** and the **zebra** image.
      - Certain regions, such as the **grass near the elephants**, show slight irregularities in color consistency.

## Discussion

The progression from **VGG16** to **EfficientNet with U-Net** and finally to **Pix2Pix GAN** highlights significant improvements in colorization performance. VGG16, while easy to implement, struggled to produce meaningful outputs, with images remaining mostly grayscale or desaturated due to its limitations as a feature extractor. EfficientNet with U-Net introduced better object recognition and spatial awareness, showing some distinction between foreground and background. However, the outputs lacked vibrancy and appeared washed out, with textures remaining underdeveloped. The Pix2Pix GAN, on the other hand, delivered the most realistic and vibrant results by leveraging adversarial training. It excelled in capturing fine details, accurately reproducing colors, and understanding context, though minor artifacts like color bleeding persisted. These findings demonstrate that GANs are far more capable of handling the complexities of realistic image colorization compared to traditional convolutional approaches.

## Enhancements for Future Work

1. While Pix2Pix GAN performed the best, the following strategies can further enhance the results:
  - **Refined Loss Functions:** Incorporating perceptual loss or a combination of adversarial and reconstruction loss can help improve color consistency and reduce artifacts.
  - **Data Augmentation:** Adding more variations in lighting, angles, and object scales to the dataset can improve generalization.
  - **Higher Resolution Outputs:** Training on higher resolution images can capture even finer details and textures.
  - **Hybrid Models:** Combining GANs with attention mechanisms or transformers may further improve color accuracy and spatial understanding.

## Conclusion

This project explored the evolution of image colorization techniques, starting from **VGG16** as a simple baseline model, progressing to **EfficientNet with U-Net**, and culminating in the **Pix2Pix GAN**. The comparative analysis reveals a clear trend:

- **VGG16** struggled with basic colorization.
- **EfficientNet with U-Net** improved object recognition but failed to deliver vivid and sharp outputs.
- **Pix2Pix GAN** achieved the best performance, producing vibrant, realistic, and contextually accurate colorizations.
The transition to GAN-based approaches demonstrates the power of adversarial training in addressing generative tasks like image colorization. Future improvements such as enhanced loss functions, hybrid architectures, and higher-resolution training can push the boundaries further, delivering even more stunning results.

This journey highlights the importance of selecting the right architecture for the task, with GANs proving to be the most effective for transforming grayscale images into colorful and realistic visuals.
