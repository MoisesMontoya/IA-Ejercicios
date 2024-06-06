## Documentación codigo de Phaser Rebotes
Este documento ayudara a comprender el funcionamiento del codigo de PHASER de rebotes el cual engloba lo siguiente en su contenido.

## Variables Globales
En las variables globales se declaran todas aquellas variables que se ocuparan a lo largo de la compilación del codigo.

```jsx
javascriptCopy code
var w = 550;
var h = 550;
var jugador;
var fondo;
var bala;

var cursors;
var menu;

var arribaJ;
var abajoJ;
var izquierdaJ;
var derechaJ;
var quietoJ;
var posicionarJV = false;
var posicionarJH = false;

var balaX;
var balaY;
var velocidadBalaX = -1 * Math.floor(Math.random() * (300 - 500 + 1) + 500);
var velocidadBalaY = -1 * Math.floor(Math.random() * (300 - 500 + 1) + 500);

var nnNetwork, nnEntrenamiento, nnSalida, datosEntrenamiento = [];
var modoAuto = false, eCompleto = false;

```

## Inicialización del Juego
Esta parte del código ayuda a cargar los recursos del juego sobre todo el espacio donde se va a estar ejecuntando el codigo para el uso del mismo.
```jsx
javascriptCopy code
var juego = new Phaser.Game(w, h, Phaser.CANVAS, '', { preload: preload, create: create, update: update, render: render });

```

### `preload()`

Carga los recursos necesarios para el juego. Esta parte es para los imports del juego, en este caso el fondo, el personaje, el menu y la bala.
```jsx
javascriptCopy code
function preload() {
    juego.load.image('fondo', 'assets/game/space.jpg');
    juego.load.spritesheet('mono', 'assets/sprites/altair.png', 32, 48);
    juego.load.image('menu', 'assets/game/menu.png');
    juego.load.image('bala', 'assets/sprites/purple_ball.png');
}

```

### `create()`

Inicializa los elementos del juego, configura la física y añade el jugador y la bala. Ademas se declara lo del movimiento que debe de ser por la flechas de dirección.

```jsx
javascriptCopy code
function create() {
    juego.physics.startSystem(Phaser.Physics.ARCADE);
    juego.physics.arcade.gravity.y = 0;
    juego.time.desiredFps = 30;

    fondo = juego.add.tileSprite(0, 0, w, h, 'fondo');
    jugador = juego.add.sprite(w / 2, h / 2, 'mono');

    juego.physics.enable(jugador);
    jugador.body.collideWorldBounds = true;
    var corre = jugador.animations.add('corre', [8, 9, 10, 11]);
    jugador.animations.play('corre', 10, true);

    bala = juego.add.sprite(150, 240, 'bala');
    juego.physics.enable(bala);
    bala.body.collideWorldBounds = true;
    bala.body.bounce.set(1);
    bala.body.velocity.x = velocidadBalaX;
    bala.body.velocity.y = velocidadBalaY;

    pausaL = juego.add.text(w - 100, 20, 'Pausa', { font: '20px Arial', fill: '#fff' });
    pausaL.inputEnabled = true;
    pausaL.events.onInputUp.add(pausa, self);
    juego.input.onDown.add(mPausa, self);

    cursors = juego.input.keyboard.createCursorKeys();

    nnNetwork = new synaptic.Architect.Perceptron(3, 6, 6, 6, 5);
    nnEntrenamiento = new synaptic.Trainer(nnNetwork);
}

```

## Funciones de la Red Neuronal

### `enRedNeural()`

Entrena la red neuronal con los datos de entrenamiento. Además que usa los principios del perceptron.

```jsx
javascriptCopy code
function enRedNeural() {
    nnEntrenamiento.train(datosEntrenamiento, { rate: 0.0003, iterations: 10000, shuffle: true });
}

```

### Funciones de Entrenamiento

- `datosDeEntrenamientoQuieto(param_entrada)`
- `datosDeEntrenamientoVertical(param_entrada)`
- `datosDeEntrenamientoHorizontal(param_entrada)`

Estas funciones activan la red neuronal y calculan las probabilidades para determinar la dirección del jugador.

```jsx
javascriptCopy code
function datosDeEntrenamientoQuieto(param_entrada) {
    nnSalida = nnNetwork.activate(param_entrada);
    var quietoJugador = Math.round(nnSalida[4] * 100);
    return quietoJugador >= 20;
}

function datosDeEntrenamientoVertical(param_entrada) {
    nnSalida = nnNetwork.activate(param_entrada);
    var arribaJugador = Math.round(nnSalida[2] * 100);
    var abajoJugador = Math.round(nnSalida[3] * 100);
    return nnSalida[2] >= nnSalida[3];
}

function datosDeEntrenamientoHorizontal(param_entrada) {
    nnSalida = nnNetwork.activate(param_entrada);
    var izquierdaJugador = Math.round(nnSalida[0] * 100);
    var derechaJugador = Math.round(nnSalida[1] * 100);
    return nnSalida[0] >= nnSalida[1];
}

```

## Funciones del Juego

### `pausa()`

Pausa el juego y muestra el menú de pausa.

```jsx
javascriptCopy code
function pausa() {
    juego.paused = true;
    menu = juego.add.sprite(w / 2, h / 2, 'menu');
    menu.anchor.setTo(0.5, 0.5);
}

```

### `mPausa(event)`

Maneja la reanudación del juego desde el menú de pausa.

```jsx
javascriptCopy code
function mPausa(event) {
    if (juego.paused) {
        var menu_x1 = w / 2 - 270 / 2, menu_x2 = w / 2 + 270 / 2,
            menu_y1 = h / 2 - 180 / 2, menu_y2 = h / 2 + 180 / 2;

        var mouse_x = event.x,
            mouse_y = event.y;

        if (mouse_x > menu_x1 && mouse_x < menu_x2 && mouse_y > menu_y1 && mouse_y < menu_y2) {
            if (mouse_x >= menu_x1 && mouse_x <= menu_x2 && mouse_y >= menu_y1 && mouse_y <= menu_y1 + 90) {
                eCompleto = false;
                datosEntrenamiento = [];
                modoAuto = false;
            } else if (mouse_x >= menu_x1 && mouse_x <= menu_x2 && mouse_y >= menu_y1 + 90 && mouse_y <= menu_y2) {
                if (!eCompleto) {
                    console.log("", "Entrenamiento " + datosEntrenamiento.length + " valores");
                    enRedNeural();
                    console.log("Listo para jugar automaticamente");
                    eCompleto = true;
                }
                modoAuto = true;
            }
            menu.destroy();
            resetGame();
            juego.paused = false;
        }
    }
}

```

### `resetGame()`

Resetea la posición y velocidad del jugador y de la bala.

```jsx
javascriptCopy code
function resetGame() {
    jugador.x = w / 2;
    jugador.y = h / 2;
    jugador.body.velocity.x = 0;
    jugador.body.velocity.y = 0;

    bala.x = 0;
    bala.y = 0;
    bala.body.velocity.x = velocidadBalaX;
    bala.body.velocity.y = velocidadBalaY;
}

```

### `update()`

Actualiza el estado del juego en cada frame.

```jsx
javascriptCopy code
function update() {
    fondo.tilePosition.x -= 1;

    juego.physics.arcade.collide(bala, jugador, colisionH, null, this);

    jugador.body.velocity.x = 0;
    jugador.body.velocity.y = 0;

    arribaJ = 0;
    abajoJ = 0;
    izquierdaJ = 0;
    derechaJ = 0;
    quietoJ = 0;

    balaX = bala.position.x - jugador.position.x;
    balaY = bala.position.y - jugador.position.y;
    var distanciaEuclidiana = Math.sqrt(balaX * balaX + balaY * balaY);

    if (modoAuto == false && cursors.left.isDown && jugador.body.position.x >= 155) {
        jugador.body.velocity.x = -300;
    } else if (modoAuto == false && cursors.right.isDown && jugador.body.position.x <= 355) {
        jugador.body.velocity.x = 300;
    }

    if (modoAuto == false && cursors.up.isDown && jugador.body.position.y >= 155) {
        jugador.body.velocity.y = -300;
    } else if (modoAuto == false && cursors.down.isDown && jugador.body.position.y <= 355) {
        jugador.body.velocity.y = 300;
    }

    if (jugador.body.velocity.x != 0 || jugador.body.velocity.y != 0) {
        quietoJ = 1;
    }

    if (balaX > 0) {
        izquierdaJ = 1;
    } else {
        derechaJ = 1;
    }

    if (balaY > 0) {
        arribaJ = 1;
    } else {
        abajoJ = 1;
    }

    if (modoAuto == true) {
        if (datosDeEntrenamientoQuieto([balaX, balaY, distanciaEuclidiana])) {
            if (distanciaEuclidiana <= 250) {
                movimientoAdecuado(
                    datosDeEntrenamientoVertical([balaX, balaY, distanciaEuclidiana]),
                    datosDeEntrenamientoHorizontal([balaX, balaY, distanciaEuclidiana]),
                    distanciaEuclidiana
                );
                ajustarJugador();
            } else if (distanciaEuclidiana >= 150) {
                jugador.body.velocity.x = 0;
                jugador.body.velocity.y = 0;
            }
        }
    }

    if (modoAuto == false && bala.position.x >= 0 && bala.position.x <= w && bala.position.y >= 0 && bala.position.y <= h) {
        datosEntrenamiento.push({
            input: [balaX, balaY, distanciaEuclidiana],
            output: [izquierdaJ, derechaJ, arribaJ, abajoJ, quietoJ]
        });
    }
}

```

### `movimientoAdecuado(vertical, horizontal, distancia)`

Determina el movimiento adecuado para el jugador basado en los datos de la red neuronal.

```jsx
javascriptCopy code
function movimientoAdecuado(vertical, horizontal, distancia) {
    if (horizontal == true) {
        if (posicionarJH == false) {
            if (balaX < 0) {
                jugador.body.velocity.x = 250;
                posicionarJH = true;
            } else {
                jugador.body.velocity.x = -250;
                posicionarJH = true;
            }
        }
    } else {
        if (posicionarJH == false) {
            if (balaX < 0) {
                jugador.body.velocity.x = -250;
                posicionarJH = true;
            } else {
                jugador.body.velocity.x = 250;
                posicionarJH = true;
            }
        }
    }

    if (vertical == true) {
        if (posicionarJV == false) {
            if (balaY < 0) {
                jugador.body.velocity.y = 250;
                posicionarJV = true;
            } else {
                jugador.body.velocity.y = -250;
                posicionarJV = true;
            }
        }
    } else {
        if (posicionarJV == false) {
            if (balaY < 0) {
                jugador.body.velocity.y = -250;
                posicionarJV = true;
            } else {
                jugador.body.velocity.y = 250;
                posicionarJV = true;
            }
        }
    }
}

```

### `ajustarJugador()`

Ajusta la velocidad del jugador a cero después de un pequeño retraso.

```jsx
javascriptCopy code
function ajustarJugador() {
    juego.time.events.add(Phaser.Timer.SECOND * 1, function () {
        jugador.body.velocity.x = 0;
        jugador.body.velocity.y = 0;
        posicionarJV = false;
        posicionarJH = false;
    }, this);
}

```

### `render()`

Renderiza los elementos del juego (aquí no se hace nada explícitamente).

```jsx
javascriptCopy code
function render() {}

```

### `colisionH()`

Muestra un mensaje de colisión y reinicia el juego.

```jsx
javascriptCopy code
function colisionH() {
    alert("Impacto detectado: Reiniciando posición de jugador y bala");
    resetGame();
}

```
## Codigo Adicional
Este codigo adicional es un codigo que funciona con la limitacion de movimiento lo cual hacia que el personaje no pudiera moverser más alla de un recuadro imaginario el cual media 210 x 210 pixeles. Esta parte se encuentra en las condiciones del update del codigo.

```jsx

function create() {
    juego.physics.startSystem(Phaser.Physics.ARCADE);
    juego.physics.arcade.gravity.y = 0; // No gravedad para permitir movimiento libre
    juego.time.desiredFps = 30;

    fondo = juego.add.tileSprite(0, 0, w, h, 'fondo');
    jugador = juego.add.sprite(w / 2, h / 2, 'mono');

    juego.physics.enable(jugador);
    jugador.body.collideWorldBounds = true; // Evitar que el jugador salga de los limites del mundo
    var corre = jugador.animations.add('corre', [8, 9, 10, 11]); 
    jugador.animations.play('corre', 10, true); 

    // Añadir la bala en la esquina superior derecha
    bala = juego.add.sprite(150, 240, 'bala');
    juego.physics.enable(bala);
    bala.body.collideWorldBounds = true; // Evitar que la bala salga de los límites del mundo
    bala.body.bounce.set(1); // Hacer que la bala rebote
    bala.body.velocity.x = velocidadBalaX;
    bala.body.velocity.y = velocidadBalaY;

    // Añadir texto de pausa
    pausaL = juego.add.text(w - 100, 20, 'Pausa', { font: '20px Arial', fill: '#fff' });
    pausaL.inputEnabled = true;
    pausaL.events.onInputUp.add(pausa, self);
    juego.input.onDown.add(mPausa, self);

    // Creación de teclas de dirección
    cursors = juego.input.keyboard.createCursorKeys();
    //

    nnNetwork =  new synaptic.Architect.Perceptron(3, 6, 6, 6, 5);
    nnEntrenamiento = new synaptic.Trainer(nnNetwork);
}


function enRedNeural(){
    nnEntrenamiento.train(datosEntrenamiento, {rate: 0.0003, iterations: 10000, shuffle: true});
}

function datosDeEntrenamientoQuieto(param_entrada) {
    nnSalida = nnNetwork.activate(param_entrada);
    var quietoJugador = Math.round( nnSalida[4]*100 );
    console.log("Valor"," Quieto%:  " + quietoJugador);

    return quietoJugador >= 20;
}

function datosDeEntrenamientoVertical(param_entrada) {
    nnSalida = nnNetwork.activate(param_entrada);
    var arribaJugador = Math.round( nnSalida[2]*100 );
    var abajoJugador = Math.round( nnSalida[3]*100 );
    console.log("Valor"," Arriba%:  " + arribaJugador + " Abajo%:  " + abajoJugador);

    return nnSalida[2] >= nnSalida[3];
}

function datosDeEntrenamientoHorizontal(param_entrada) {
    nnSalida = nnNetwork.activate(param_entrada);
    var izquierdaJugador = Math.round( nnSalida[0]*100 );
    var derechaJugador = Math.round( nnSalida[1]*100 );
    console.log("Valor"," Izquierda%:  " + izquierdaJugador + " Derecha%:  " + derechaJugador);

    return nnSalida[0] >= nnSalida[1];
}

function pausa() {
    juego.paused = true; // Pausar el juego
    menu = juego.add.sprite(w / 2, h / 2, 'menu'); // Añadir menú de pausa
    menu.anchor.setTo(0.5, 0.5);
}

function mPausa(event){
    if(juego.paused){
        var menu_x1 = w/2 - 270/2, menu_x2 = w/2 + 270/2,
            menu_y1 = h/2 - 180/2, menu_y2 = h/2 + 180/2;

        var mouse_x = event.x  ,
            mouse_y = event.y  ;

        if(mouse_x > menu_x1 && mouse_x < menu_x2 && mouse_y > menu_y1 && mouse_y < menu_y2 ){
            if(mouse_x >=menu_x1 && mouse_x <=menu_x2 && mouse_y >=menu_y1 && mouse_y <=menu_y1+90){
                eCompleto=false;
                datosEntrenamiento = [];
                modoAuto = false;
            }else if (mouse_x >=menu_x1 && mouse_x <=menu_x2 && mouse_y >=menu_y1+90 && mouse_y <=menu_y2) {
                if(!eCompleto) {
                    console.log("","Entrenamiento "+ datosEntrenamiento.length +" valores" );
                    enRedNeural();
                    console.log("Listo para jugar automaticamente");
                    eCompleto=true;
                }
                modoAuto = true;
            }
            menu.destroy();
            resetGame(); // Resetear el juego
            juego.paused = false;
        }
    }
}

function resetGame() {
    // Resetear la posición y velocidad del jugador
    jugador.x = w / 2;
    jugador.y = h / 2;
    jugador.body.velocity.x = 0;
    jugador.body.velocity.y = 0;

    // Resetear la posición y velocidad de la bala
    bala.x = 0;
    bala.y = 0;
    bala.body.velocity.x = velocidadBalaX;
    bala.body.velocity.y = velocidadBalaY;
}

function update() {
    fondo.tilePosition.x -= 1; // Mover el fondo para crear efecto de desplazamiento

    juego.physics.arcade.collide(bala, jugador, colisionH, null, this); // Colision de la bala con el jugador

    // Por cada frame detener el movimiento del jugador
    jugador.body.velocity.x = 0;
    jugador.body.velocity.y = 0;

    // Variables de salida al aprender
    arribaJ = 0;
    abajoJ = 0;
    izquierdaJ = 0;
    derechaJ = 0;
    quietoJ = 0;

    // Variables de entrada al aprender
    balaX = bala.position.x - jugador.position.x;
    balaY = bala.position.y - jugador.position.y;
    var distanciaEuclidania = Math.sqrt(balaX * balaX + balaY * balaY);
        
    // Movimiento del jugador en horizontal
        if (modoAuto == false && cursors.left.isDown && jugador.body.position.x >=155 )
        {
            jugador.body.velocity.x = -300;
        }
        else if (modoAuto == false && cursors.right.isDown && jugador.body.position.x <= 355)
        {
            jugador.body.velocity.x = 300;
        }
    

    // Movimiento del jugador vertical
        if (modoAuto == false && cursors.up.isDown && jugador.body.position.y >=155)
        {
            jugador.body.velocity.y = -300;
        }
        else if (modoAuto == false && cursors.down.isDown && jugador.body.position.y <= 355)
        {
            jugador.body.velocity.y =  300;
        }

    // Notar si el jugador se está moviendo
    if (jugador.body.velocity.x != 0 || jugador.body.velocity.y != 0) {
        quietoJ = 1;
    }

    // Actualizacion de las variables por cuadrante en horizontal
    if ( balaX > 0 ) {
        izquierdaJ = 1;
    } else {
        derechaJ = 1;
    }

    // Actualizacion de las variables por cuadrante en horizontal
    if ( balaY > 0 ) {
        arribaJ = 1;
    } else {
        abajoJ = 1;
    }

    // Decisiones a tomar en el modo automatico
    if ( modoAuto == true ) {
        if ( datosDeEntrenamientoQuieto([ balaX, balaY, distanciaEuclidania ]) ) {
            if (distanciaEuclidania <= 250) {
                movimientoAdecuado( 
                    datosDeEntrenamientoVertical([ balaX, balaY, distanciaEuclidania ]),
                    datosDeEntrenamientoHorizontal([ balaX, balaY, distanciaEuclidania ]),
                    distanciaEuclidania 
                );
                ajustarJugador();
            } else if ( distanciaEuclidania >= 150) {
                jugador.body.velocity.x = 0;
                jugador.body.velocity.y = 0;
            }
        }
    }


    // Concatenar valores para despues entrenar la red neuronal
    if(modoAuto == false && bala.position.x >= 0 && bala.position.y >= 0 ){
        datosEntrenamiento.push({
            'input' : [balaX, balaY, distanciaEuclidania],
            'output' : [izquierdaJ, derechaJ, arribaJ, abajoJ, quietoJ] 
        });

        console.log("Arriba: "  + arribaJ + " " + "Abajo: " + abajoJ);
        console.log("Izquierda: "  + izquierdaJ + " " + "Derecha: " + derechaJ);
        console.log("Quieto: " + quietoJ);
        console.log("----------------------------------------------------------");
        console.log("Bala X: " + balaX + " " + "Bala Y: " + balaY);
        console.log("Distancia euclidania: " + distanciaEuclidania);
        console.log("----------------------------------------------------------");
    }

}

function movimientoAdecuado(verticalJ, horizontalJ, distancia) {
    if ( verticalJ && !posicionarJV) {
        jugador.body.velocity.y = -300;
    } else if ( !verticalJ && !posicionarJV && distancia <= 200 ) {
        jugador.body.velocity.y = 300;
    }

    if ( horizontalJ && !posicionarJH) {
        jugador.body.velocity.x = -300;
    } else if ( !horizontalJ && !posicionarJH && distancia <= 200 ) {
        jugador.body.velocity.x = 300;
    }
}

function ajustarJugador () {
    if ( jugador.body.position.x > 750) {
        jugador.body.velocity.x = -200;
        posicionarJH = true;
    } else if ( jugador.body.position.x < 50 ) {
        jugador.body.velocity.x = 200;
        posicionarJH = true;
    } else if ( posicionarJH && (jugador.body.position.x >= 50 && jugador.body.position.x <= 750) ) {
        jugador.body.position.x = w/2;
    }

    if ( jugador.body.position.y > 350 ) {
        jugador.body.velocity.y = -200;
        posicionarJV = true;
    } else if ( jugador.body.position.y < 50 ) {
        jugador.body.velocity.y = 200;
        posicionarJV = true;
    } else if ( posicionarJV && (jugador.body.position.y >= 50 && jugador.body.position.y <= 350) ) {
        jugador.body.position.y = h/2;
    }
}


function colisionH() {
    pausa(); // Pausar el juego en caso de colisión
}

function render() {
    // Opcionalmente, renderizar el estado del juego o información adicional
}

```