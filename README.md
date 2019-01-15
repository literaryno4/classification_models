# Classification models Zoo
Trained on [ImageNet](http://www.image-net.org/) classification models. [Keras](https://keras.io/).

### Architectures: 
- [VGG](https://arxiv.org/abs/1409.1556) [16, 19]
- [ResNet](https://arxiv.org/abs/1512.03385) [18, 34, 50, 101, 152]
- [ResNeXt](https://arxiv.org/abs/1611.05431) [50, 101]
- [SE-ResNet](https://arxiv.org/abs/1709.01507) [18, 34, 50, 101, 152]
- [SE-ResNeXt](https://arxiv.org/abs/1709.01507) [50, 101]
- [SE-Net](https://arxiv.org/abs/1709.01507) [154]
- [DenseNet](https://arxiv.org/abs/1608.06993) [121, 169, 201]
- [Inception ResNet V2](https://arxiv.org/abs/1602.07261)
- [Inception V3](http://arxiv.org/abs/1512.00567)
- [Xception](https://arxiv.org/abs/1610.02357)
- [NASNet](https://arxiv.org/abs/1707.07012) [large, mobile]
- [MobileNet](https://arxiv.org/pdf/1704.04861.pdf)
- [MobileNet v2](https://arxiv.org/abs/1801.04381)

### Specification 
| Model           |Acc@1|Acc@5|Time*|Source|
|-----------------|:---:|:---:|:---:|------|
|densenet121      |74.67|92.04|27.66|[keras](https://github.com/keras-team/keras-applications)|
|densenet169      |75.85|92.93|33.71|[keras](https://github.com/keras-team/keras-applications)|
|densenet201      |77.13|93.43|42.40|[keras](https://github.com/keras-team/keras-applications)|
|inceptionresnetv2|80.03|94.89|54.77|[keras](https://github.com/keras-team/keras-applications)|
|inceptionv3      |77.55|93.48|38.94|[keras](https://github.com/keras-team/keras-applications)|
|mobilenet        |70.36|89.39|15.50|[keras](https://github.com/keras-team/keras-applications)|
|mobilenetv2      |71.63|90.35|18.31|[keras](https://github.com/keras-team/keras-applications)|
|nasnetlarge      |82.12|95.72|116.53|[keras](https://github.com/keras-team/keras-applications)|
|nasnetmobile     |74.04|91.54|27.73|[keras](https://github.com/keras-team/keras-applications)|
|resnet101        |76.58|93.10|33.03|[mxnet](https://github.com/Microsoft/MMdnn)|
|resnet101v2      |71.93|90.41|28.80|[keras](https://github.com/keras-team/keras-applications)|
|resnet152        |76.66|93.08|42.37|[mxnet](https://github.com/Microsoft/MMdnn)|
|resnet152v2      |72.29|90.61|41.09|[keras](https://github.com/keras-team/keras-applications)|
|resnet18         |68.24|88.49|16.07|[mxnet](https://github.com/Microsoft/MMdnn)|
|resnet34         |72.17|90.74|17.37|[mxnet](https://github.com/Microsoft/MMdnn)|
|resnet50         |74.81|92.38|22.62|[mxnet](https://github.com/Microsoft/MMdnn)|
|resnet50v2       |69.73|89.31|19.56|[keras](https://github.com/keras-team/keras-applications)|
|resnext101       |78.48|94.00|60.07|[keras](https://github.com/keras-team/keras-applications)|
|resnext50        |77.36|93.48|37.57|[keras](https://github.com/keras-team/keras-applications)|
|senet154         |81.06|95.24|137.36|[pytorch](https://github.com/Cadene/pretrained-models.pytorch)|
|seresnet101      |77.92|94.00|32.55|[pytorch](https://github.com/Cadene/pretrained-models.pytorch)|
|seresnet152      |78.34|94.08|47.88|[pytorch](https://github.com/Cadene/pretrained-models.pytorch)|
|seresnet50       |76.44|93.02|23.64|[pytorch](https://github.com/Cadene/pretrained-models.pytorch)|
|seresnext101     |79.88|94.87|62.80|[pytorch](https://github.com/Cadene/pretrained-models.pytorch)|
|seresnext50      |78.74|94.30|38.29|[pytorch](https://github.com/Cadene/pretrained-models.pytorch)|
|vgg16            |70.79|89.74|24.95|[keras](https://github.com/keras-team/keras-applications)|
|vgg19            |70.89|89.69|24.95|[keras](https://github.com/keras-team/keras-applications)|
|xception         |78.87|94.20|42.18|[keras](https://github.com/keras-team/keras-applications)|

### Weights
| Name                    |Classes   | Models    |
|-------------------------|:--------:|:---------:|
|'imagenet'               |1000      |all models |
|'imagenet11k-place365ch' |11586     |resnet50   |
|'imagenet11k'            |11221     |resnet152  |


### Installation

Requirements:
- python 3.x
- keras >= 2.x
- tensorflow >= 1.4

###### Note
    This library does not have TensorFlow in a requirements for installation. 
    Please, choose suitable version (‘cpu’/’gpu’) and install it manually using 
    official Guide (https://www.tensorflow.org/install/).

PyPI package:
```bash
$ pip install image-classifiers
```
Latest version:
```bash
$ pip install git+https://github.com/qubvel/classification_models.git
```

### Examples 

##### Loading model with `imagenet` weights:

1) Direct way (keras-applications like)
```python
from classification_models.resnet import ResNet18, preprocess_input

model = ResNet18((224, 224, 3), weight='imagenet')
```

2) Using `Classifiers` container
```python
from classification_models import Classifiers

classifier, preprocess_input = Classifiers.get('resnet18')
model = classifier((224, 224, 3), weight='imagenet')
```
This way take one additional line of code, however if you would 
like to train several models you do not need to import them directly, 
just access everything through `Classifiers`.

You can get all model names using `Classifiers.names()` method.

##### Inference example:
 
```python
import numpy as np
from skimage.io import imread
from skimage.transfrom import resize
from keras.applications.imagenet_utils import decode_predictions

from classification_models.resnet import ResNet18, preprocess_input

# read and prepare image
x = imread('./imgs/tests/seagull.jpg')
x = resize(x, (224, 224)) * 255    # cast back to 0-255 range
x = preprocess_input(x)
x = np.expand_dims(x, 0)

# load model
model = ResNet18(input_shape=(224,224,3), weights='imagenet', classes=1000)

# processing image
y = model.predict(x)

# result
print(decode_predictions(y))
```

##### Model fine-tuning example:
```python
import keras
from classification_models.resnet import ResNet18, preprocess_input

# prepare your data
X = ...
y = ...

X = preprocess_input(X)

n_classes = 10

# build model
base_model = ResNet18(input_shape=(224,224,3), weights='imagenet', include_top=False)
x = keras.layers.AveragePooling2D((7,7))(base_model.output)
output = keras.layers.Dense(n_classes)(x)
model = keras.models.Model(inputs=[base_model.input], outputs=[output])

# train
model.compile(optimizer='SGD', loss='categorical_crossentropy', metrics=['accuracy'])
model.fit(X, y)
```
