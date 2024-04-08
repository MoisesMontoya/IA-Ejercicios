```python
import cv2 as cv
```

pwd es para ver la ruta donde esta corriendo el directorio
Filtro con deteccion de color blanco

```python
cap = cv.VideoCapture(0)
while(True):
    res , img = cap.read()
    img2=cv.cvtColor(img, cv.COLOR_BGR2HSV)
    ubb=(32,32,200)
    uba=(110,255,255)
    mask=cv.inRange(img2, ubb,uba)
    res=cv.bitwise_and(img, img, mask=mask)
    cv.imshow('captura', res)
    if cv.waitKey(1) &0xFF == ord('s'):
        break
       
cap.release()
cv.destroyAllWindows()
```

Filtro con color azul 


```python
cap = cv.VideoCapture(0)
while(True):
    res , img = cap.read()
    img2=cv.cvtColor(img, cv.COLOR_BGR2HSV)
    ubb=(50,85,90)
    uba=(110,255,255)
    mask=cv.inRange(img2, ubb,uba)
    res=cv.bitwise_and(img, img, mask=mask)
    cv.imshow('captura', res)
    if cv.waitKey(1) &0xFF == ord('s'):
        break
       
cap.release()
cv.destroyAllWindows()
```


```python

```


```python

```
