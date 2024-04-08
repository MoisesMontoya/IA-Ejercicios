```python
import numpy as np
import cv2 as cv
import math
rostro = cv.CascadeClassifier('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media\\haarcascade_frontalface_alt.xml')
#Cargar XML del rostro
```


```python
cap = cv.VideoCapture(0)
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
    cv.imshow('rostros', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```

https://github.com/opencv/opencv/tree/master/data/haarcascades

https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_objdetect/py_face_detection/py_face_detection.html


https://docs.opencv.org/2.4/doc/user_guide/ug_traincascade.html
    


```python
import numpy as np
import cv2 as cv
rostro = cv.CascadeClassifier('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media\\haarcascade_frontalface_alt.xml')
```


```python
cap = cv.VideoCapture(0)
i = 0
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
        frame2 = frame[y:y+h, x:x+w]
        cv.imshow('rostro2', frame2)
        cv.imwrite('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media\\'+str(i)+'.png', frame2)
    cv.imshow('rostros', frame)
    i=i+1
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()

```

Mejora Reconocimeinto Facial


```python
import numpy as np 
import cv2 as cv

rostro = cv.CascadeClassifier('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media\\haarcascade_frontalface_alt.xml')
```


```python
cap = cv.VideoCapture(0)
i = 0
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        #frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
        frame2 = frame[y:y+h, x:x+w]
        cv.imshow('rostro2', frame2)
        frame2 = cv.resize(frame2, (200,200), interpolation = cv.INTER_AREA)
        cv.imwrite('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media'+str(i)+'.png', frame2) #Directorio donde se guardaran los rostros + nombre de la imagen
    #cv.imshow('rostros', frame)
    i=i+1
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```


```python
dataSet = 'C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media'
faces  = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet+'/'+face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath+'/'+faceName,0))
    label = label + 1
print(np.count_nonzero(np.array(labels)==0)) 

faceRecognizer = cv.face.EigenFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('Eigenface.xml')
```


```python
import cv2 as cv
import os 
```


```python
faceRecognizer = cv.face.EigenFaceRecognizer_create()
faceRecognizer.read('Eigenface.xml')

cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\media\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (200,200), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        #cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (0,0,0), 1, cv.LINE_AA)
        if result[1] > 2800:
            cv.putText(frame,'{}'.format(faces[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
        else:
            cv.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2)
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```


```python

```
