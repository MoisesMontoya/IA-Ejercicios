```python
import cv2 as cv
```

Se busca en la imagen y se filtra por escala de rojo.


```python
img = cv.imread('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media\\imagenprueba.jpg')
img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
ubb = (0, 60, 60)
uba = (10, 255, 255)

ubb1 = (170, 60, 60)
uba1 = (180, 255, 255)

ubb2=(0,255,165)
uba3=(126,86,36)

mask1 = cv.inRange(img2, ubb, uba)
mask2 = cv.inRange(img2, ubb1, uba1)
mask3 = mask1 + mask2
mask4 = cv.inRange(img2,ubb2,ubb2)
mask4 = mask3 + mask4

img3 = cv.bitwise_and(img, img, mask = mask3)

cv.imshow('img2', img2)
cv.imshow('img', img)
cv.imshow('img3', img3)
cv.imshow('mask3', mask3)

cv.waitKey(0)
cv.destroyAllWindows()
```


```python
cap = cv.VideoCapture(0)
while(True):
    ret, img = cap.read()
    if ret == True:
        cv.imshow('video', img)
        k = cv.waitKey(1) & 0xFF
        if k == 27 :
            break
    else:
        break
cap.release()
cv.destroyAllWindows()
```


```python
cap = cv.VideoCapture(0)
while(True):
    ret, img = cap.read()
    img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
    if ret == True:
        cv.imshow('video', img)
        cv.imshow('video2', img2)
        k = cv.waitKey(1) & 0xFF
        if k == 27 :
            break
    else:
        break
cap.release()
cv.destroyAllWindows()
```


```python
cap = cv.VideoCapture(0)
while(True):
    ret, img = cap.read()
    r,g,b = cv.split(img)
    #img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
    if ret == True:
        cv.imshow('video', img)
        #cv.imshow('video2', img2)
        cv.imshow('r', r)
        cv.imshow('g', g)
        cv.imshow('b', b)
        k = cv.waitKey(1) & 0xFF
        if k == 27 :
            break
    else:
        break
cap.release()
cv.destroyAllWindows()
```


```python

```
