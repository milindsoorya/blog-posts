## How to build a simple CNN based Image classifier using Keras

## 1 Introduction

### 1.1 What are Convolutional neural networks (CNN)?

Convolutional neural network (CNN), are a class of artificial neural networks that has become dominant in various computer vision tasks, it is attracting interest across a variety of domains.

A convolutional neural network is composed of multiple building blocks, such as convolution layers, pooling layers, and fully connected layers, and is designed to automatically and adaptively learn spatial hierarchies of features through a back propagation algorithm.

CNN work well on computer vision tasks like image classification, object detection, image recognition, etc. Other Neural networks used for similar tasks includes recurrent neural networks (RNN), long short term memory (LSTM), artificial neural networks (ANN), etc.,

### 1.2 The classification task

In this article, I will be trying to solve the [HP Unlocked challenge](https://www.hp.com/us-en/workstations/industries/data-science/unlocked-challenge.html). It is challenge number four.

You can get the data from the above website, or you could also fork the files from my [GitHub repository - Unlocked_Challenge_4](https://github.com/milindsoorya/Unlocked_Challenge_4.git) and start working on it directly.

### 1.3 Problem statement

This is a binary classification task. The challenge is to build a machine learning model to classify images of "La Eterna", a kind of flower.

### 1.4 Approach

I will use a CNN model to get a baseline score. On initial analysis, the dataset is quite small for a deep learning task. I will perform some [image augmentations](https://www.tensorflow.org/tutorials/images/data_augmentation) to increase the dataset.

I will also explore hyperparameter tuning and transfer learning using VGG19 in the upcoming articles.

## 2 The Code

I used a local Jupyter lab instance for running the code. The final code can be found [here](https://github.com/milindsoorya/Unlocked_Challenge_4).

### 2.1 Import data manipulation packages

```py
import pandas as pd
import numpy as np
import os
import cv2
import matplotlib.pyplot as plt
import warnings
```

```py
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras.preprocessing.image import ImageDataGenerator
import keras_tuner as kt
```

```py
print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
```

```py
# Set the seed value for experiment reproducibility.
seed = 1842
tf.random.set_seed(seed)
np.random.seed(seed)
# Turn off warnings for cleaner looking notebook
warnings.simplefilter('ignore')
```

### 2.2 Load in the Data

```py
#define image dataset
# Data Augmentation
image_generator = ImageDataGenerator(
        rescale=1/255,
        rotation_range=10, # rotation
        width_shift_range=0.2, # horizontal shift
        height_shift_range=0.2, # vertical shift
        zoom_range=0.2, # zoom
        horizontal_flip=True, # horizontal flip
        brightness_range=[0.2,1.2],# brightness
        validation_split=0.2,)

#Train & Validation Split
train_dataset = image_generator.flow_from_directory(batch_size=32,
                                                 directory='data_cleaned/Train',
                                                 shuffle=True,
                                                 target_size=(224, 224),
                                                 subset="training",
                                                 class_mode='categorical')

validation_dataset = image_generator.flow_from_directory(batch_size=32,
                                                 directory='data_cleaned/Train',
                                                 shuffle=True,
                                                 target_size=(224, 224),
                                                 subset="validation",
                                                 class_mode='categorical')

#Organize data for our predictions
image_generator_submission = ImageDataGenerator(rescale=1/255)
submission = image_generator_submission.flow_from_directory(
                                                 directory='data_cleaned/scraped_images',
                                                 shuffle=False,
                                                 target_size=(224, 224),
                                                 class_mode=None)
```

### 2.3 Build the CNN

Don't worry about how the network is created. We can use hyperparameter tuning to better tune the layers and get a stable network. I will discuss it in the next article. If you would like to learn more about it, check out [Keras documentation](https://keras.io/guides/keras_tuner/getting_started/).

Make sure you don't mess up the input and output shapes. Here the input shape is (224, 224, 3). Meaning the height, width and the channels of the image are respectively 224, 224 and 3. (3 is the red, green and blue channels of a colour image).

```py
model = keras.models.Sequential([
    keras.layers.Conv2D(32, (3, 3), activation='relu', input_shape = [224, 224,3]),
    keras.layers.MaxPooling2D(),
    keras.layers.Conv2D(64, (2, 2), activation='relu'),
    keras.layers.MaxPooling2D(),
    keras.layers.Conv2D(64, (2, 2), activation='relu'),
    keras.layers.Flatten(),
    keras.layers.Dense(100, activation='relu'),
    keras.layers.Dense(2, activation ='softmax')
])
```

Now we can compile the prepared model. Note that we are also using a call back to stop the training early. In this case, the callback will be triggered if the validation loss is the same or increasing for more than 3 epochs.

```py
model.compile(optimizer='adam',
             loss = 'binary_crossentropy',
             metrics=['accuracy'])

callback = keras.callbacks.EarlyStopping(monitor='val_loss',
                                            patience=3,
                                            restore_best_weights=True)
```

### 2.4 Training the CNN

```py
model.fit(train_dataset, epochs=20, validation_data=validation_dataset, callbacks=callback)
```

### 2.4 Evaluating the performance of CNN

```py
loss, accuracy = model.evaluate(validation_dataset)
print("Loss: ", loss)
print("Accuracy: ", accuracy)
```

### 2.5 Saving the model

```py
model.save('cnn-model')
```

### 2.6 Load the model

```py
model = keras.models.load_model('cnn-model')
```

### 2.7 Create Sample Submission

```py
onlyfiles = [f.split('.')[0] for f in os.listdir(os.path.join('data_cleaned/scraped_images/image_files')) if os.path.isfile(os.path.join(os.path.join('data_cleaned/scraped_images/image_files'), f))]
submission_df = pd.DataFrame(onlyfiles, columns =['images'])
submission_df[['la_eterna', 'other_flower']] = model.predict(submission)
submission_df.head()
```

```py
submission_df.to_csv('submission_file.csv', index = False)
```

## Related articles

- [Installing TensorFlow on M1 MacBook Air with GPU (Metal)](https://www.milindsoorya.com/blog/installing-tensorflow-on-m1-macbook-air-with-gpu)
- [Run Ubuntu on M1 Macbook Air using UTM](https://www.milindsoorya.com/blog/run-ubuntu-on-m1-macbook-air-using-utm)
- [Add anaconda to right-click menu in windows](https://www.milindsoorya.com/blog/add-anaconda-to-right-click-menu-in-windows)
- [How to open sublime text from the windows command line](https://www.milindsoorya.com/blog/how-to-open-sublime-text-from-the-windows-command-line)

## References

- https://www.tensorflow.org/tutorials/images/data_augmentation
- https://www.analyticsvidhya.com/blog/2020/02/learn-image-classification-cnn-convolutional-neural-networks-3-datasets/
- https://www.youtube.com/watch?v=l-NAT4H4384
