```python
import cv2 as cv
import os
import numpy as np
```


```python
def rota(img, i):
    for j in range(10, 361, 20):
        h, w = img.shape[:2]
        mw = cv.getRotationMatrix2D((h // 2, w // 2), j, -1)
        img2 = cv.warpAffine(img, mw, (h, w))
        cv.imwrite('C:\Users\MsMon\Desktop\Proyectos\IA\media\Willy\Willy' + str(i) + '_' + str(j) + '.png', img2)
    
```


```python
def escalagris(img,1)
gray = cv.cvtColor(img, cv.Color_BGR2GRAY)
cv.imwrite('C:\Users\MsMon\Desktop\Proyectos\IA\media\Willy\Willy'+str(i)+'.jpg',gray)
```


```python
def escalaimagen(img, 1)
    img = 
```


```python
i=0
imgPaths = "C:\Users\MsMon\Desktop\Proyectos\IA\media\Willy\Willy"
nomFiles = os.listdir(imgPaths)
for nomFiles in nomFiles:
    i=i+1
    imgPath=imgPaths+"/"+nomFiles
    img=cv.imread(imgPath)
    rota(img,i)
```


```python

```
