# Reporte del Juego - Jorge Alberto Juárez Rocha

## Resumen
Es un juego "simple" a primera vista, corriendo directamente en un HTML version 5 y código JS. Contiene sus propios Assets y no parece ser demasiado complejo.

## Variables
- `w` y `h`: Definen el ancho y alto del juego.
- `jugador`: Representa al personaje controlado por el jugador.
- `fondo`: El fondo del juego.
- `bala`: La bala que se dispara desde la nave.
- `nave`: La nave desde donde se dispara la bala.
- `salto`: Almacena la tecla que activa el salto del jugador.
- `nnNetwork`, `nnEntrenamiento`, `nnSalida`, `datosEntrenamiento`: Variables relacionadas con la red neuronal artificial utilizada en el juego.
- `modoAuto`, `eCompleto`: Controlan si el juego está en modo automático y si se ha completado el entrenamiento de la red neuronal, respectivamente.

## Funciones
- `preload`: Carga los recursos del juego, como imágenes y sprites.
- `create`: Inicializa el juego, crea los elementos del juego y activa la física de Phaser.
- `update`: Función que se ejecuta en cada fotograma del juego, maneja la lógica del juego, como el movimiento del jugador y las colisiones.
- `render`: Función de renderizado que actualmente no se utiliza.
- `pausa`: Pausa el juego y muestra un menú de pausa.
- `mPausa`: Maneja la lógica cuando se pausa el juego y se interactúa con el menú de pausa.
- `resetVariables`: Restablece las variables del juego a sus valores iniciales.
- `saltar`: Hace que el jugador salte.
- `disparo`: Dispara la bala desde la nave.
- `colisionH`: Maneja la colisión entre la bala y el jugador.

### Preparación del Juego
- En la función `preload`, se cargan los recursos del juego, como imágenes y sprites.
- En la función `create`, se inicializan los elementos del juego, se configura el sistema de física Arcade de Phaser y se crean los objetos del juego, como el jugador, la bala y el fondo.

### Actualización del Juego
- La función `update` se ejecuta en cada fotograma del juego y maneja la lógica del juego.
- Se actualiza el movimiento del fondo para simular el movimiento del jugador.
- Se verifica la colisión entre la bala y el jugador, y se realiza una acción en consecuencia.
- Se controla el movimiento del jugador y se manejan las interacciones del usuario, como el salto manual del jugador.

### Paradigma de Programación
El código sigue principalmente el paradigma de programación orientada a objetos. Se utilizan objetos y clases para representar entidades del juego, como el jugador, la bala y la nave. Se utilizan funciones para modularizar y organizar la lógica del juego en unidades más manejables.  también se observan elementos de programación imperativa, especialmente en el ciclo de juego `update`, donde se ejecutan acciones basadas en condiciones y eventos del juego.

La lógica del juego se basa en la interacción entre el jugador, la bala y el entorno del juego, utilizando funciones y objetos para organizar y controlar el flujo del juego. También se usa la programación imperativa para manejar la lógica del juego en tiempo real.

### Interacción con el Jugador
- El jugador representado por el sprite llamado "mono" puede controlar manualmente el salto del personaje principal presionando la tecla de espacio.
- Cuando se presiona la tecla de espacio (`salto`), si el jugador está en el suelo (`jugador.body.onFloor()`), se aplica una velocidad vertical negativa al jugador para que salte hacia arriba.
- Puede cilckear las opciones de `Auto Mode` y `Manual Mode`


### Físicas en Phaser
Phaser utiliza un motor de física Arcade que permite simular colisiones, gravedad y otros comportamientos físicos en el juego. En este juego, se activa el sistema de física Arcade en la función `create` con la línea `juego.physics.startSystem(Phaser.Physics.ARCADE);`. La gravedad se establece en 800 píxeles por segundo al cuadrado, lo que significa que los objetos caen rápidamente hacia abajo.

### Bala y Nave
- La bala y la nave son sprites que se crean en la función `create`. La posición inicial de la bala está detrás de la nave, lista para ser disparada.
- Cuando se dispara la bala, se le asigna una velocidad horizontal negativa para que se mueva hacia la izquierda. La velocidad se establece en un valor aleatorio entre 300 y 800 píxeles por segundo, y su componente vertical se mantiene en cero.
- La posición de la bala se actualiza en la función `update` en cada fotograma del juego. Si la bala alcanza el borde izquierdo del juego (`bala.position.x <= 0`), se restablecen sus variables para prepararla para un nuevo disparo.


### Colisión entre la Bala y el Jugador
- Se detecta la colisión entre la bala y el jugador utilizando `juego.physics.arcade.collide(bala, jugador, colisionH, null, this);`. Cuando ocurre una colisión, se llama a la función `colisionH`, que maneja la colisión.
- En este caso, la función `colisionH` pausa el juego y muestra un menú de pausa.

### Manejo de la Bala
- La función `disparo` se encarga de disparar la bala. Se establece una velocidad horizontal negativa para la bala, y su velocidad vertical se mantiene en cero para que se dispare en línea recta hacia la izquierda.
- La función `resetVariables` se llama después de que la bala alcanza el borde del juego o colisiona con el jugador. Restablece las variables de la bala para prepararla para un nuevo disparo.

Las físicas en Phaser se utilizan para simular el movimiento de la bala, del jugador y detectar colisiones entre ellos. La interacción con el jugador se maneja principalmente mediante eventos de entrada, mientras que el movimiento y la interacción de la bala se controlan mediante funciones específicas en el ciclo de actualización del juego.

## Uso de Red Neuronal
- `nnNetwork`: Define una red neuronal con arquitectura Perceptrón.
- `datosDeEntrenamiento`: Utiliza la red neuronal para predecir si el jugador debe saltar o no en función de la posición relativa de la bala y su velocidad.
- `enRedNeural`: Entrena la red neuronal con los datos de entrenamiento.
- `datosEntrenamiento`: Almacena los datos de entrenamiento para la red neuronal.

## Otros Detalles
- El fondo se mueve continuamente hacia la izquierda para dar la sensación de movimiento.
- El jugador puede controlar manualmente el salto presionando la tecla de espacio.
- Cuando la bala colisiona con el jugador, el juego se pausa y se muestra un menú de pausa.

EL juego combina elementos de control manual del jugador con la capacidad de la inteligencia artificial para predecir y controlar ciertos aspectos del juego, como el salto del jugador.

Cabe destacar que la parte de IA no parece tener un modelo amplio, por lo que "pierde" con bastante regularidad