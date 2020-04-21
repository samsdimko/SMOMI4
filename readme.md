Нейросеть на основе vgg16, предобученной на imagenet, с обученным классификатором, везде темп обучения 1e-11. В файле train.py хранится код для нейросети с использованием аугментации случайным горизонтальным отражением,  поворотом на случайный угол, случайной яркостью и выделением случайного участка изображения. В остальных файлах -- реализация каждого соответствующего метода в отдельности.

a) Аугментация горизонтальным отражением (train_flip.py):

```
image = tf.image.random_flip_left_right(image)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/gor.png)

b) Аугментация поворотом на случайный угол (train_degree.py):

1) 30 градусов

```
degree = 30
dgr = random.uniform(-degree, degree)
image = tf.image.convert_image_dtype(image, tf.float32)
image = tf.contrib.image.rotate(image, dgr * math.pi / 180, interpolation='BILINEAR')
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/dgr30.png)

2) 45 градусов

```
degree = 45
dgr = random.uniform(-degree, degree)
image = tf.image.convert_image_dtype(image, tf.float32)
image = tf.contrib.image.rotate(image, dgr * math.pi / 180, interpolation='BILINEAR')
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/dgr45.png)

3) Совмещенные графики

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/dgr.png)

Поскольку на 45 градусах имеет место возрастание loss после ~60 эпохи, при том, что точность ниже, чем при 30 градусах, я оставляю 30





c) Аугментация случайной яркостью (train_brit.py):

1) Коэффициент 0.3

`````
image = tf.image.random_brightness(image, 0.3, seed=None)
image = tf.image.random_contrast.image(0.4, 1.4, seed=None)
`````

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br03.png)

2) Коэффициент 0.4

```
image = tf.image.random_brightness(image, 0.4, seed=None)
image = tf.image.random_contrast.image(0.4, 1.4, seed=None)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br04.png)

3) Коэффициент 0.5

```
image = tf.image.random_brightness(image, 0.5, seed=None)
image = tf.image.random_contrast.image(0.4, 1.4, seed=None)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br05.png)

4) Совмещенные графики

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br.png)

Поскольку яркость при коэфициенте 0.5 имеет наилучшую точность и при этом меньшие потери, я оставляю 0.5





d) Аугментация выделением случайного участка изображения (train_crop.py):

1) 130x130

```
image = tf.image.random_crop(image, size=[130, 130, 3], seed=None, name=None)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop130.png)

2) 150x150

```
image = tf.image.random_crop(image, size=[150, 150, 3], seed=None, name=None)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop150.png)

3) 180x180

```
image = tf.image.random_crop(image, size=[180, 180, 3], seed=None, name=None)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop180.png)

4) Совмещенные графики

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop.png)

При выделении участка 130x130 сразу наступает переобучение, при 150x150 сеть плохо обучается. При 180x180 графики обучения ведут себя нормально, и точность на валидационных данных растет лучше всего, поэтому оставляю этот вариант





e) Графики для использования всех методов вместе (отражение, поворот на 30 градусов, выделение участка 180x180, изменение яркости с коэффициентом 0.5)

```
degree = 30
dgr = random.uniform(-degree, degree)
image = tf.image.random_flip_left_right(image)
image = tf.image.random_brightness(image, 0.5, seed=None)
image = tf.image.random_contrast(image, 0.4, 1.4, seed=None)    
image = tf.contrib.image.rotate(image, dgr * math.pi / 180, interpolation='BILINEAR')
image = tf.image.random_crop(image, size=[180, 180, 3], seed=None, name=None)
```

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/Last.png)