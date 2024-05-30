Codigo que ayuda a encontrar a Wally


```python
import numpy as np
import cv2 as cv

image = cv.imread(r'C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\datasetwa\\pruebas\\p6.jpg')

gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)

wally = cv.CascadeClassifier(r'C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\datasetwa\\cascade.xml')

scaleFactor=1.10 #Sirve para hacer la escala de precision con los positivos
minNeighbors=25 #Sirve para conocer las refs con los positivos
minSize=70 #Sirve para descartar cuadros con las caracterisiticas anteriores

waldo_detections = wally.detectMultiScale(gray, scaleFactor=scaleFactor, minNeighbors=minNeighbors, minSize=(minSize,minSize))

for (x, y, w, h) in waldo_detections:
    cv.rectangle(image, (x, y), (x+w, y+h), (0, 255, 0), 2)
    cv.putText(image, 'Wally', (x, y-10), cv.FONT_HERSHEY_SIMPLEX, 0.9, (0, 255, 0), 2)


cv.imshow('Wally', image)
cv.waitKey(0)
cv.destroyAllWindows()
```


```python

```


```python

```
