---
title: 'Samouczek: Uruchamianie modelu TensorFlow w języku Python — Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Uruchom model TensorFlow w języku Python.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: babc9f8c7b8a05c4a91ead4990267311e926fd47
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236425"
---
# <a name="tutorial-run-tensorflow-model-in-python"></a>Samouczek: Uruchamianie modelu TensorFlow w języku Python

W tym przewodniku Szybki start opisano, jak po [wyeksportowaniu modelu TensorFlow](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) z usługi Custom Vision Service użyć tego modelu lokalnie w celu klasyfikowania obrazów.

> [!NOTE]
> Ten samouczek dotyczy tylko modeli wyeksportowane z projektów klasyfikacji obrazów.

## <a name="prerequisites"></a>Wymagania wstępne

Aby korzystać z tego samouczka, należy wykonać następujące czynności:

- Zainstalować język Python 2.7+ lub Python 3.5+.
- Zainstaluj program pip.

Następnie należy zainstalować następujące pakiety:

```
pip install tensorflow
pip install pillow
pip install numpy
pip install opencv-python
```

## <a name="load-your-model-and-tags"></a>Ładowanie modelu i tagów

Pobrany plik zip zawiera pliki model.pb i labels.txt. Te pliki stanowią uczony model i etykiety klasyfikacji. Pierwszym krokiem jest załadowanie modelu do projektu.

```Python
import tensorflow as tf
import os

graph_def = tf.GraphDef()
labels = []

# These are set to the default names from exported models, update as needed.
filename = "model.pb"
labels_filename = "labels.txt"

# Import the TF graph
with tf.gfile.GFile(filename, 'rb') as f:
    graph_def.ParseFromString(f.read())
    tf.import_graph_def(graph_def, name='')

# Create a list of labels.
with open(labels_filename, 'rt') as lf:
    for l in lf:
        labels.append(l.strip())
```

## <a name="prepare-an-image-for-prediction"></a>Przygotowanie obrazu do przewidywania

Przygotowanie obrazu tak, aby miał odpowiedni kształt do przewidywania, wymaga wykonania kilku czynności. Te kroki są podobne do manipulacji obrazem podczas uczenia:

### <a name="open-the-file-and-create-an-image-in-the-bgr-color-space"></a>Otwieranie pliku i tworzenie obrazu w przestrzeni kolorów BGR

```Python
from PIL import Image
import numpy as np
import cv2

# Load from a file
imageFile = "<path to your image file>"
image = Image.open(imageFile)

# Update orientation based on EXIF tags, if the file has orientation info.
image = update_orientation(image)

# Convert to OpenCV format
image = convert_to_opencv(image)
```

### <a name="deal-with-images-with-a-dimension-1600"></a>Praca z obrazami i wymiarze >1600

```Python
# If the image has either w or h greater than 1600 we resize it down respecting
# aspect ratio such that the largest dimension is 1600
image = resize_down_to_1600_max_dim(image)
```

### <a name="crop-the-largest-center-square"></a>Przycinanie największego środkowego kwadratu

```Python
# We next get the largest center square
h, w = image.shape[:2]
min_dim = min(w,h)
max_square_image = crop_center(image, min_dim, min_dim)
```

### <a name="resize-down-to-256x256"></a>Zmiana rozmiaru w dół do 256x256

```Python
# Resize that square down to 256x256
augmented_image = resize_to_256_square(max_square_image)
```

### <a name="crop-the-center-for-the-specific-input-size-for-the-model"></a>Przycinanie środka do określonego rozmiaru danych wejściowych dla modelu

```Python
# Get the input size of the model
with tf.Session() as sess:
    input_tensor_shape = sess.graph.get_tensor_by_name('Placeholder:0').shape.as_list()
network_input_size = input_tensor_shape[1]

# Crop the center for the specified network_input_Size
augmented_image = crop_center(augmented_image, network_input_size, network_input_size)

```

W powyższych krokach używane są następujące funkcje pomocnicze:

```Python
def convert_to_opencv(image):
    # RGB -> BGR conversion is performed as well.
    image = image.convert('RGB')
    r,g,b = np.array(image).T
    opencv_image = np.array([b,g,r]).transpose()
    return opencv_image

def crop_center(img,cropx,cropy):
    h, w = img.shape[:2]
    startx = w//2-(cropx//2)
    starty = h//2-(cropy//2)
    return img[starty:starty+cropy, startx:startx+cropx]

def resize_down_to_1600_max_dim(image):
    h, w = image.shape[:2]
    if (h < 1600 and w < 1600):
        return image

    new_size = (1600 * w // h, 1600) if (h > w) else (1600, 1600 * h // w)
    return cv2.resize(image, new_size, interpolation = cv2.INTER_LINEAR)

def resize_to_256_square(image):
    h, w = image.shape[:2]
    return cv2.resize(image, (256, 256), interpolation = cv2.INTER_LINEAR)

def update_orientation(image):
    exif_orientation_tag = 0x0112
    if hasattr(image, '_getexif'):
        exif = image._getexif()
        if (exif != None and exif_orientation_tag in exif):
            orientation = exif.get(exif_orientation_tag, 1)
            # orientation is 1 based, shift to zero based and flip/transpose based on 0-based values
            orientation -= 1
            if orientation >= 4:
                image = image.transpose(Image.TRANSPOSE)
            if orientation == 2 or orientation == 3 or orientation == 6 or orientation == 7:
                image = image.transpose(Image.FLIP_TOP_BOTTOM)
            if orientation == 1 or orientation == 2 or orientation == 5 or orientation == 6:
                image = image.transpose(Image.FLIP_LEFT_RIGHT)
    return image
```

## <a name="predict-an-image"></a>Przewidywanie obrazu

Po przygotowaniu obrazu jako tensora można go wysłać za pośrednictwem modelu na potrzeby przewidywania:

```Python

# These names are part of the model and cannot be changed.
output_layer = 'loss:0'
input_node = 'Placeholder:0'

with tf.Session() as sess:
    try:
        prob_tensor = sess.graph.get_tensor_by_name(output_layer)
        predictions, = sess.run(prob_tensor, {input_node: [augmented_image] })
    except KeyError:
        print ("Couldn't find classification output layer: " + output_layer + ".")
        print ("Verify this a model exported from an Object Detection project.")
        exit(-1)
```

## <a name="view-the-results"></a>Wyświetlanie wyników

Wyniki uruchomienia tensora obrazu za pośrednictwem modelu trzeb będzie wtedy zamapować z powrotem na etykiety.

```Python
    # Print the highest probability label
    highest_probability_index = np.argmax(predictions)
    print('Classified as: ' + labels[highest_probability_index])
    print()

    # Or you can print out all of the results mapping labels to probabilities.
    label_index = 0
    for p in predictions:
        truncated_probablity = np.float64(np.round(p,8))
        print (labels[label_index], truncated_probablity)
        label_index += 1
```

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak opakowywać modelu w aplikacji mobilnej:
* [Używanie wyeksportowanego modelu TensorFlow w aplikacji systemu Android](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Używanie wyeksportowanego modelu CoreML w aplikacji języka Swift systemu iOS](https://go.microsoft.com/fwlink/?linkid=857726)
* [Używanie wyeksportowanego modelu CoreML w aplikacji systemu iOS na platformie Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)
