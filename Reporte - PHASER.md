# - Reporte por Angel Moisés Montoya Montes de Oca
# - Explicacion del Procesamiento de PHASER y las capas que utiliza para el aprendizaje
Utiliza la biblioteca Synaptic.js para crear y entrenar una red neuronal. La red neuronal en este contexto se utiliza para controlar el comportamiento de un personaje en un juego. Vamos a revisar cómo funcionan las capas de la red neuronal en este código:
Inicialización de la red neuronal:
Se define una red neuronal con “nnNetwork = new synaptic.Architect.Perceptron”(2, 6, 6, 2); Esta red tiene una arquitectura de perceptrón multicapa con una capa de entrada de 2 neuronas, dos capas ocultas de 6 neuronas cada una, y una capa de salida de 2 neuronas.
Entrenamiento de la red neuronal:
La función “datosDeEntrenamiento” recibe como entrada un conjunto de datos (en este caso, el desplazamiento de la bala y la velocidad de la bala) y devuelve la salida correspondiente de la red neuronal. Esta salida se utiliza para determinar si el personaje debe saltar o no en el juego.
Antes de usar la red neuronal, se debe entrenar con datos de entrenamiento. Los datos de entrenamiento se generan dinámicamente mientras se juega o se recopilan de alguna manera.
La función “enRedNeural” se encarga de entrenar la red neuronal con los datos de entrenamiento utilizando el algoritmo de entrenamiento especificado.
Uso de la red neuronal en el juego:
Cuando se activa el modo automático (modoAuto = true), en el ciclo de actualización (update) del juego, se llama a la función “datosDeEntrenamiento” con el desplazamiento de la bala y la velocidad de la bala como entrada para obtener la salida de la red neuronal.
Dependiendo de la salida de la red neuronal, el personaje saltará o no automáticamente.
Recopilación de datos de entrenamiento:
En el modo de juego manual (modoAuto = false), se recopilan datos de entrenamiento mientras se juega. Estos datos consisten en el desplazamiento de la bala, la velocidad de la bala y el estado del personaje (si está en el aire o en el suelo) en ese momento.
Estos datos se almacenan en el arreglo “datosEntrenamiento”, que luego se utiliza para entrenar la red neuronal.
El aprendizaje por refuerzo implica que el agente (en este caso, el personaje del juego controlado por la IA) aprenda a través de la interacción con un entorno, recibiendo retroalimentación en forma de recompensas o penalizaciones según su comportamiento.
Aquí hay algunas sugerencias sobre cómo podrías modificar el código para implementar el aprendizaje por refuerzo:
Definir una función de recompensa:
Crea una función que evalúe el estado del juego y asigne una recompensa al agente según su desempeño. Por ejemplo, podrías otorgar una recompensa positiva si el agente esquivó una bala con éxito y una recompensa negativa si fue golpeado por una bala.
Modificar la función de entrenamiento:
# - Posible mejoras para en caso de querer mejorar el modelo de PHASER.
En lugar de entrenar la red neuronal únicamente con datos predefinidos, puedes actualizar los pesos de la red neuronal en función de las recompensas recibidas por el agente durante el juego. Esto implica utilizar un algoritmo de aprendizaje por refuerzo, como Q-Learning o Aprendizaje Profundo por Refuerzo (RL), para actualizar los pesos de la red.
Actualizar la función de decisión:
Modifica la función que decide si el agente debe saltar o no para que utilice la red neuronal entrenada y tome decisiones basadas en las observaciones del entorno y en las recompensas esperadas.
Entrenamiento en línea o por lotes:
Decide si deseas entrenar la red neuronal en línea, es decir, durante la ejecución del juego, o si prefieres realizar un entrenamiento por lotes, donde recopilas datos de varias sesiones de juego y luego entrenas la red fuera de línea.
Exploración vs. Explotación:
Implementa una estrategia de exploración para que el agente pueda descubrir nuevas acciones y evitar quedarse atrapado en comportamientos subóptimos. Esto implica que el agente tome decisiones aleatorias ocasionalmente en lugar de seguir siempre la política aprendida.
Refinar la función de recompensa y el diseño de la red neuronal:
Experimenta con diferentes funciones de recompensa y arquitecturas de red neuronal para mejorar el rendimiento del agente.
Al incorporar estos cambios, tu IA aprenderá a través de la experiencia, adaptándose y mejorando su rendimiento en el juego con el tiempo. Agregar más capas a la red neuronal puede aumentar su capacidad para aprender representaciones más complejas de los datos de entrada, lo que podría llevar a un mejor rendimiento en el juego. Sin embargo, debes tener en cuenta algunas consideraciones:
Complejidad del problema: Si el problema que estás abordando es relativamente simple, agregar capas adicionales a la red neuronal puede no ser necesario y podría llevar a un sobreajuste. Por otro lado, si el problema es más complejo, como requerir estrategias de juego más sofisticadas, agregar capas adicionales podría ser beneficioso.
Cantidad de datos de entrenamiento: Agregar capas adicionales aumentará el número de parámetros en la red neuronal, lo que significa que requerirás más datos de entrenamiento para entrenar efectivamente la red. Si no tienes suficientes datos de entrenamiento, la red podría sobre ajustarse a los datos disponibles y no generalizar bien a nuevas situaciones.
Tiempo de entrenamiento: Cuantas más capas tenga la red neuronal, más tiempo tomará entrenarla. Debes considerar si tienes los recursos computacionales necesarios para entrenar una red neuronal más grande en un tiempo razonable.
Regularización: Con redes neuronales más grandes, es importante implementar técnicas de regularización, como la disminución del aprendizaje o la regularización L1/L2, para evitar el sobreajuste.
Experimentación y validación: Antes de decidir agregar capas adicionales, realiza experimentos y valida el rendimiento de la red neuronal en un conjunto de datos de prueba. Esto te ayudará a determinar si la adición de capas mejora realmente el rendimiento del modelo.
