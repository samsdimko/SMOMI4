Нейросеть на основе vgg16, предобученной на imagenet, с обученным классификатором, темп обучения 1e-11. В файле train.py хранится код для нейросети с использованием аугментации случайным горизонтальным отражением,  поворотом на случайный угол, случайной яркостью и выделением случайного участка изображения. В остальных файлах -- реализация каждого соответствующего метода в отдельности.

Аугментация горизонтальным отражением (train_flip.py):

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/gor.png)

Аугментация поворотом на случайный угол (train_degree.py):

1) 30 градусов

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/dgr30.png)

2) 45 градусов

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/dgr45.png)

3) Совмещенные графики

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/dgr.png)

Поскольку на 45 градусах имеет место возрастание loss после ~60 эпохи, при условии, что точность ниже, чем при 30, я оставляю 30

Аугментация случайной яркостью (train_brit.py):

1) Коэффициент 0.3

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br03.png)

2) Коэффициент 0.4

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br04.png)

3) Коэффициент 0.5

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br05.png)

4) Совмещенные графики

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/br.png)

Поскольку яркость при коэфициенте 0.5 имеет наилучшую точность и при этом меньшие потери, я оставляю 0.5

Аугментация выделением случайного участка изображения (train_crop.py):

1) 130x130

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop130.png)

2) 150x150

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop150.png)

3) 180x180

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop180.png)

4) Совмещенные графики

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/crop.png)

При выделении участка 130x130 сразу наступает переобучение, при 150x150 сеть плохо обучается. При 180x180 графики обучения ведут себя нормально, и точность на валидационных данных растет лучше всего, поэтому оставляю этот вариант

Графики для использования всех методов вместе:

![Image alt](https://github.com/samsdimko/SMOMI4/blob/master/Last.png)