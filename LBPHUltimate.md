```python
import cv2 as cv
import os 
import numpy as np 
```

Acceso de Camara codigo en LBPH


```python
dataSet = 'C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\dataset\\Persones'
faces  = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet+'\\'+face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath+'\\'+faceName,0))
    label = label + 1
#print(np.count_nonzero(np.array(labels)==0)) 
faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\dataset\\LBPHPersonesFix2.xml')
```

    ['Angel', 'Dancy', 'Edgar', 'George', 'Joeeee', 'jonhy', 'Leo', 'LeoC', 'moy', 'Pancho']
    

Generacion de Codigo XML para el Reconocimiento Facial.
Este codigo a traves del Haarcascade ayuda a detectar rasgos faciales que ayudaran a tomar las snapshots del rostro y empezara a generar todos los datesets de rostros que ocuparemos para hacer la deteccion de rostros.


```python
import cv2 as cv
import os 

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\dataset\\LBPHPersonesFix2.xml')
dataSet = 'C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\dataset\\Persones'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(1)
rostro = cv.CascadeClassifier('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (255,255,0), 1, cv.LINE_AA)
        if result[1] < 100:
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

Codigo para la identificación de emociones
Ya con los diferentes datasets generados para cada una de las emociones, lo compilamos usando el xml generado con el LBPH que tiene mejor rendimiento que usando el TrainerGui, ya cuando compila, abre la camara y este dependiendo de las expresiones que se haga frente a la camara este lo detectara pero para ello debera contar con condiciones similares con las se tomó el dataset.


```python
import cv2 as cv
import os 

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\dataset\\EmocionesLBPHZwei.xml')
dataSet = 'C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\dataset\\Emociones3'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\MsMon\\Desktop\\Proyectos\\IA\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (255,255,0), 1, cv.LINE_AA)
        if result[1] < 100:
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
