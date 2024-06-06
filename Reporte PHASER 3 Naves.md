# Juego con Phaser y Redes Neuronales

Este código implementa un juego utilizando el framework Phaser y redes neuronales para automatizar el movimiento del jugador y las decisiones de juego.

## Variables

### Dimensiones del juego

- `w`: Ancho del juego.
- `h`: Alto del juego.

### Elementos del juego

- `jugador`: Sprite del jugador.
- `fondo`: Sprite del fondo.
- `bala`, `bala2`, `bala3`: Sprites de las balas.
- `balaD`, `balaD2`, `balaD3`: Indicadores de si las balas han sido disparadas.
- `nave`: Sprite de la nave enemiga.

### Controles del jugador

- `salto`: Tecla para saltar.
- `izquierda`: Tecla para moverse a la izquierda.
- `derecha`: Tecla para moverse a la derecha.

### Elementos del menú

- `menu`: Sprite del menú de pausa.

### Velocidades y desplazamientos

- `velocidadBala`, `velocidadBala2`, `velocidadBala3`: Velocidades de las balas.
- `despBala`, `despBala2`, `despBala3`: Desplazamientos de las balas respecto al jugador.

### Estatus del jugador

- `estatusAire`, `estatuSuelo`: Estatus de si el jugador está en el aire o en el suelo.
- `estatusIzquierda`, `estatusQuieto`, `estatusDerecha`: Estatus de si el jugador se está moviendo a la izquierda, está quieto o se está moviendo a la derecha.

### Redes neuronales

- `nnNetwork`, `nnEntrenamiento`, `nnSalida`, `datosEntrenamiento`: Variables relacionadas con la primera red neuronal.
- `nnNetwork2`, `nnEntrenamiento2`, `nnSalida2`, `datosEntrenamiento2`: Variables relacionadas con la segunda red neuronal.
- `modoAuto`, `eCompleto`: Indicadores del modo automático y si el entrenamiento de las redes neuronales ha sido completado.

## Configuración del juego

### Creación del juego

Se crea el juego con el ancho `w` y el alto `h`.

### Carga de recursos

Se cargan las imágenes necesarias para el juego.

### Inicialización del juego

Se inicializan los elementos del juego, como el fondo, las balas, el jugador, etc. Se habilitan las físicas de ARCADE de Phaser y se establece la gravedad.

## Funciones del juego

### Entrenamiento de la red neuronal

Se entrena la red neuronal utilizando los datos de entrenamiento.

### Datos de entrenamiento

Se activa la red neuronal con los datos de entrada y se obtiene la salida correspondiente.

### Pausa del juego

Se pausa el juego y se muestra un menú de pausa.

### Manejo de la pausa

Se maneja la lógica de la pausa del juego.

### Reseteo de variables

Se resetean las variables del juego.

### Reseteo de las balas

Se resetean las balas para ser disparadas nuevamente.

### Control de movimiento del jugador

Se implementan las funciones para controlar el movimiento del jugador: saltar, moverse a la izquierda y moverse a la derecha.

## Actualización del juego

Se actualiza el estado del juego en cada fotograma, manejando las colisiones, el movimiento del fondo, las acciones del jugador y la interacción con el modo automático.

## Funciones de Disparo y Colisiones

### Disparo de Balas

Se implementan las funciones para disparar las balas.

### Colisión del Jugador con Balas

Se implementa la lógica de la colisión del jugador con las balas.

## Funciones de Renderizado

Se define la función de renderizado del juego.

Estas son las funciones principales del juego documentadas en Markdown.

## Codigo completo utilizado en el juego

```jsx
var w=800;
var h=400;
var jugador;
var fondo;

var bala, balaD=false, nave;
var bala2, balaD2=false;
var bala3, balaD3=false;

var salto;
var izquierda;
var derecha;

var menu;

var velocidadBala;
var despBala;

var velocidadBala2;
var despBala2;

var velocidadBala3;
var despBala3;

var estatusAire;
var estatuSuelo;
var estatusIzquierda;
var estatusQuieto;
var estatusDerecha;

var nnNetwork , nnEntrenamiento, nnSalida, datosEntrenamiento=[];
var nnNetwork2 , nnEntrenamiento2, nnSalida2, datosEntrenamiento2=[];
var modoAuto = false, eCompleto=false;

var juego = new Phaser.Game(w, h, Phaser.CANVAS, '', { preload: preload, create: create, update: update, render:render});

function preload() {
    juego.load.image('fondo', 'assets/game/fondo.jpg');
    juego.load.spritesheet('mono', 'assets/sprites/altair.png',32 ,48);
    juego.load.image('nave', 'assets/game/ufo.png');
    juego.load.image('bala', 'assets/sprites/purple_ball.png');
    juego.load.image('bala2', 'assets/sprites/purple_ball.png');
    juego.load.image('bala3', 'assets/sprites/purple_ball.png');
    juego.load.image('menu', 'assets/game/menu.png');
}



function create() {

    juego.physics.startSystem(Phaser.Physics.ARCADE);
    juego.physics.arcade.gravity.y = 800;
    juego.time.desiredFps = 30;

    fondo = juego.add.tileSprite(0, 0, w, h, 'fondo');
    nave = juego.add.sprite(w-100, h-70, 'nave');
    nave = juego.add.sprite(15, 0,'nave');
    nave = juego.add.sprite(w-250, 0,'nave');
    bala = juego.add.sprite(w-100, h, 'bala');
    bala2 = juego.add.sprite(55, 0, 'bala2');
    bala3 = juego.add.sprite(w-240, 0, 'bala3');
    jugador = juego.add.sprite(50, h, 'mono');


    juego.physics.enable(jugador);
    jugador.body.collideWorldBounds = true;
    var corre = jugador.animations.add('corre',[8,9,10,11]);
    jugador.animations.play('corre', 10, true);

    juego.physics.enable(bala);
    bala.body.collideWorldBounds = true;

    juego.physics.enable(bala2);
    bala2.body.collideWorldBounds = true;

    juego.physics.enable(bala3);
    bala3.body.collideWorldBounds = true;

    pausaL = juego.add.text(w - 100, 20, 'Pausa', { font: '20px Arial', fill: '#fff' });
    pausaL.inputEnabled = true;
    pausaL.events.onInputUp.add(pausa, self);
    juego.input.onDown.add(mPausa, self);

    salto = juego.input.keyboard.addKey(Phaser.Keyboard.SPACEBAR);
    izquierda = juego.input.keyboard.addKey(Phaser.Keyboard.LEFT);
    derecha = juego.input.keyboard.addKey(Phaser.Keyboard.RIGHT);

    
    nnNetwork =  new synaptic.Architect.Perceptron(4, 6, 6, 2);
    nnEntrenamiento = new synaptic.Trainer(nnNetwork);
    nnNetwork2 =  new synaptic.Architect.Perceptron(2, 6, 6, 2);
    nnEntrenamiento2 = new synaptic.Trainer(nnNetwork2);

}

function enRedNeural(){
    nnEntrenamiento.train(datosEntrenamiento, {rate: 0.0003, iterations: 10000, shuffle: true});
    nnEntrenamiento2.train(datosEntrenamiento2, {rate: 0.0003, iterations: 10000, shuffle: true});
}

function datosDeEntrenamiento(param_entrada){
    console.log("Entrada",param_entrada[0]+" "+param_entrada[1]+param_entrada[2]+" "+param_entrada[3]);
    nnSalida = nnNetwork.activate(param_entrada);
    var aire=Math.round( nnSalida[0]*100 );
    var piso=Math.round( nnSalida[1]*100 );
    return nnSalida[0]>=nnSalida[1];
}

function datosDeEntrenamientoMove(param_entrada){
    console.log("Entrada",param_entrada[0]+" "+param_entrada[1]);
    nnSalida2 = nnNetwork2.activate(param_entrada);
    var derechaMove=Math.round( nnSalida2[0]*100 );
    var quietoMove=Math.round( nnSalida2[1]*100 );
    return nnSalida2[0]>=nnSalida2[1];
}



function pausa(){
    juego.paused = true;
    menu = juego.add.sprite(w/2,h/2, 'menu');
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
                    console.log("","Entrenamiento brincar "+ datosEntrenamiento.length +" valores" );
                    console.log("","Entrenamiento mover "+ datosEntrenamiento2.length +" valores" );
                    enRedNeural();
                    eCompleto=true;
                }
                modoAuto = true;
            }

            menu.destroy();
            resetVariables();
            juego.paused = false;

        }
    }
}


function resetVariables(){
    jugador.body.velocity.x=0;
    jugador.body.velocity.y=0;
    bala.body.velocity.x = 0;
    bala.position.x = w-100;
    jugador.position.x=50;
    balaD=false;

    bala2.body.velocity.x = 0;
    bala2.position.x = 55;
    bala2.position.y = 0;
    balaD2=false;

    bala3.body.velocity.x = 0;
    bala3.position.x = w-240;
    bala3.position.y = -50;
    balaD3=false;
}

function resetBala() {
    bala.body.velocity.x = 0;
    bala.position.x = w-100;
    balaD=false;
}

function resetBala2() {
    bala2.body.velocity.y = 0;
    bala2.position.x = 55;
    bala2.position.y = 0;
    balaD2=false;
}

function resetBala3() {
    bala3.body.velocity.x = 0;
    bala3.position.x = w-240;
    bala3.position.y = -50;
    balaD3=false;
}

function saltar(){
    jugador.body.velocity.y = -270;
}

function atras(){
    jugador.body.velocity.x = -100;
}

function adelante(){
    if (jugador.body.position.x <= 200  ){ 
    jugador.body.velocity.x = 100 ;
    } else {
        jugador.body.velocity.x = 0;
    }
}

function update() {
    
    console.log(jugador.body.position.x);

    fondo.tilePosition.x -= 1; 

    juego.physics.arcade.collide(bala, jugador, colisionH, null, this);
    juego.physics.arcade.collide(bala2, jugador, colisionH, null, this);
    juego.physics.arcade.collide(bala3, jugador, colisionH, null, this);

    estatuSuelo = 1;
    estatusAire = 0;

    estatusQuieto = 1;
    estatusDerecha = 0;

    if(!jugador.body.onFloor()) {
        estatuSuelo = 0;
        estatusAire = 1;
    }

    if (jugador.position.x > 50) {
        estatusQuieto = 0;
        estatusDerecha = 1;
    }
	
    despBala = Math.floor( jugador.position.x - bala.position.x );
    despBala2 = Math.floor( -jugador.position.y + bala2.position.y );
    despBala3 = Math.floor( jugador.position.x - bala3.position.x );

    if( modoAuto==false && salto.isDown &&  jugador.body.onFloor() ){
        saltar();
    }
   
    if( modoAuto==false && izquierda.isDown ){
        atras();
    }

    if( modoAuto==false && derecha.isDown ){
        adelante();
    }
    
    if( modoAuto == true  && bala.position.x>0 && jugador.body.onFloor()) {

        if( datosDeEntrenamiento( [despBala , velocidadBala, despBala3, velocidadBala3] )  ){
            saltar();
        } 
    }

    if( modoAuto == true  && bala2.position.y>0) {

        if( datosDeEntrenamientoMove( [despBala2 , velocidadBala2] )  ){
            adelante();
        } 
    }

    if( balaD==false ){
        disparo();
    }
    if( balaD2==false ){
        disparo2();
    }
    if( balaD3==false ){
        disparo3();
    }

    if( bala.position.x <= 0  ){
        resetBala();
    }

    if( bala2.position.y >= 320  ){
        resetBala2();
    }

    if( bala3.position.x <=  0  ){
        resetBala3();
    }
    
    if( modoAuto ==false  && bala.position.x > 0 ){

        datosEntrenamiento.push({
            'input' :  [despBala , velocidadBala, despBala3, velocidadBala3],
            'output':  [estatusAire , estatuSuelo ]  
        });

        datosEntrenamiento2.push({
            'input' :  [despBala2 , velocidadBala2],
            'output':  [estatusDerecha , estatusQuieto ]  
        });
        
       console.log("Aie: "  + estatusAire + " " + "Suelo: " + estatuSuelo);
    }
}

function disparo(){
    velocidadBala =  -1 * velocidadRandom(300,500);
    bala.body.velocity.y = 0 ;
    bala.body.velocity.x = velocidadBala ;
    balaD=true;
}

function disparo2(){
    velocidadBala2 = -50;
    bala2.body.velocity.y = velocidadBala2;
    bala2.body.velocity.x = 0;
    balaD2=true;
}

function disparo3(){
    velocidadBala3 = -300;
    bala3.body.velocity.y = 0;
    bala3.body.velocity.x = velocidadBala3;
    balaD3=true;
}

function colisionH(){
    pausa();
}

function velocidadRandom(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

function render(){

}

```