# CIFAR 10 Classification with Transfer Learning
This Python Notebook demonstrates using the Keras API to classify images from the [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html) dataset using transfer learning.

## Method
The model used the classify our images is created from the pretrained image classifier [VGG19](https://arxiv.org/abs/1409.1556)  
This network was originally trained to classify images from the imagenet dataset. This dataset consists of thousands of images divided into 1000 distinct categories. 

To use this network for the CIFAR-10 dataset we apply the following steps:

1. Remove the final fully-connected Softmax layer from the VGG19 network
- This layer is used as the output probabilities for each of the 1000 classes in the original network.
- The CIFAR-10 dataset only has 10 classes so we only want 10 output probabilities
2. Freeze the weights of the remaining layers
- We want to keep the weights learned from the imagenet dataset, so we freeze all the remaining layers from the VGG19 network to prevent our future training from updating the weights
3. Add 2 new layers, a Dropout layer (50%) to prevent overfitting, and a fully-connected softmax layer with 10 units as our new output.
4. Upscale our CIFAR-10 dataset from 32x32x3 -> 224x224x3.
- The VGG19 network was originally trained for images of this size and thus that is the expected size for the input layer, we upscale our images in batches as upscaling the whole dataset at once is far too memory intensive.
5. Fit our new network to the dataset using the Keras API just like we would with a simple network we created ourselves
- Since we froze the weights of the VGG19 network, only the newly added fully-connected layer representing our outputs will have its weights altered by the training process.

## Results

I was able to achieve approximately 83% validation accuracy after 20 epochs of 12500 samples. While this result is already impressive considering the training only took about 15 minutes on [Google Colab](https://colab.research.google.com) there are steps I could take to improve this.

I believe despite the low number of trainable neurons, the network may still be suffering from overfitting, I may experiment layer with increasing the dropout before the final layer to reduce this during training. Secondly, I want to try using the ImageDataGenerator class from the Keras API, this allows us to apply visual transformations to images in a dataset to create more samples from the original dataset. Lastly, I want to experiment with using a different upscaling method for images, currently I am using the build in inter-cubic interpolation from cv2, however I am wondering if a different method may improve the quality of the images, making them easier for the network to classify.