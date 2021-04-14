# FlappyBird en Python con Pygame

## Instalación de pygame

Para instalar pygame en nuestro ordenador, ejecutaremos el símbolo del sistema como administrador.

![cmd](images/cmd.png)

Enseguida ejecutaremos el siguiente comando:

`pip install pygame`

y esperaremos a que se instale pygame en nuestro equipo.

## Creación del juego

### Preparación del entorno de trabajo

En una carpeta nueva insertaremos los
[recursos](Recursos.zip 'Recursos.zip')
para el proyecto, que consisten en las imágenes, los sonidos y la fuente para el
mismo.

Enseguida crearemos en nuestra carpeta el archivo en donde escribiremos el
código, que en mi caso será `flap.py`. Para realizar esto, se recomienda abrir la carpeta de nuestro proyecto en Visual Studio Code. De esta forma, la estructura del proyecto
quedará de la siguiente forma:

```
├───assets/
│   └───...
├───sound/
│   └───...
├───04B_19.TTF
└───flap.py
```

### Importando pygame

Para comprobar que pygame se haya instalado correctamente, escribiremos el
siguiente código en `flap.py`:

<pre><code><div style="background: #B3FFC6">import pygame, sys, random</div></code></pre>

Lo ejecutaremos en el símbolo del sistema, que podremos abrir dentro de Visual Studio Code con el comando `Ctrl + Ñ`, de la siguiente forma:

`python flap.py`

Y como resultado nos debe mostrar algo así:

```
pygame 2.0.1 (SDL 2.0.14, Python 3.9.1)
Hello from the pygame community. https://www.pygame.org/contribute.html
```

### Creando la ventana del juego

Enseguida agregaremos lo siguiente en `flap.py`:

<pre><code>import pygame, sys, random
<div style="background: #B3FFC6">
pygame.init()
pantalla = pygame.display.set_mode((288,512))
reloj = pygame.time.Clock()

while True:
  for event in pygame.event.get():
    if event.type == pygame.QUIT:
      pygame.quit()
      sys.exit()

  pygame.display.update()
  reloj.tick(60)
</div></code></pre>

El código anterior se divide en dos partes. La primera es donde inicializaremos
el juego, y la segunda es el ciclo, en donde se actualizará el estado del juego
constantemente, y que consiste en el ciclo `while True:`.

Con `pygame.init()` inicializamos pygame,
con `pantalla = pygame.display.set_mode((288,512))` establecemos el ancho y alto
de nuestra ventana, y con `reloj = pygame.time.Clock()` creamos el reloj con el
que controlaremos el tiempo en el juego.

Dentro de nuestro ciclo (`while True:`) detectamos todos los eventos (`for event in pygame.event.get()`), y si el evento es
`pygame.QUIT`, que se activa cuando cerramos la ventana del juego, se finaliza
pygame (`pygame.quit()`) y se cierra el juego con `sys.exit()`.

Al final de nuestro ciclo actualizamos la ventana del juego (`pygame.display.update()`) y limitamos la
velocidad a la que este se ejecuta a 60 ciclos por segundo (`reloj.tick(60)`).

Si ejecutamos nuestro juego ahora lo único que veremos será una pantalla en
negro, que podremos cerrar ya que detectamos el evento `pygame.QUIT`.

![screen1](images/screen1.png)

### Añadiendo el fondo

Para añadir el fondo crearemos una superficie, que añadiremos a nuestra pantalla
en cada ciclo para que siempre se muestre, de la siguiente forma:

<pre><code><div style="background: #CCE5FF">...</div>pantalla = pygame.display.set_mode((288,512))
reloj = pygame.time.Clock()

<div style="background: #B3FFC6">superficie_fondo = pygame.image.load('assets/background-day.png').convert()

</div>while True:
  for event in pygame.event.get():
    if event.type == pygame.QUIT:
      pygame.quit()
      sys.exit()
<div style="background: #B3FFC6">
  pantalla.blit(superficie_fondo, (0,0))
</div>
  pygame.display.update()
  reloj.tick(60)</code></pre>

Con la línea `superficie_fondo = pygame.image.load('assets/background-day.png').convert()`
guardamos la imagen que utilizaremos como fondo en una variable para poderla
utilizar, y en la línea `pantalla.blit(superficie_fondo, (0,0))` ponemos esa
imagen en la pantalla, estableciendo sus coordenadas.

Si ejecutamos nuestro juego de nuevo, ahora nos mostrará una ventana con nuestro
fondo.

![screen2](images/screen2.png)

### Agregando el suelo

Para agregar el suelo haremos algo muy parecido a lo que hicimos con el fondo:

<pre><code><div style="background: #CCE5FF">...
</div>reloj = pygame.time.Clock()

superficie_fondo = pygame.image.load('assets/background-day.png').convert()
<div style="background: #B3FFC6">superficie_suelo = pygame.image.load('assets/base.png').convert()</div>
while True:
  for event in pygame.event.get():<div style="background: #CCE5FF">...
</div>      sys.exit()

  pantalla.blit(superficie_fondo,(0,0))
<div style="background: #B3FFC6">
  pantalla.blit(superficie_suelo,(0,450))
</div>
  pygame.display.update()
  reloj.tick(60)
</></code></pre>

Si ejecutamos nuestro código veremos algo así:

![screen3](images/screen3.png)

Ahora vamos a añadirle movimiento. Para ello crearemos una variable en donde
guardaremos la posición horizontal del suelo y la moveremos un poco a la
izquierda en cada ciclo, de la siguiente forma:

<pre><code><div style="background: #CCE5FF">...</div>
superficie_fondo = pygame.image.load('assets/background-day.png').convert()
superficie_suelo = pygame.image.load('assets/base.png').convert()
<div style="background: #B3FFC6">pos_suelo_x = 0</div>
while True:
  for event in pygame.event.get():
<div style="background: #CCE5FF"div>...</div>
  pantalla.blit(superficie_fondo, (0,0))

<div style="background: #FFCCD2">  pantalla.blit(superficie_suelo,(0,450))</div><div style="background: #B3FFC6">  pos_suelo_x -= 1
  pantalla.blit(superficie_suelo,(pos_suelo_x,450))</div>
  pygame.display.update()
  reloj.tick(60)
</div></code></pre>

Si ejecutamos nuestro juego veremos lo siguiente:

![screen4](images/screen4.gif)

¡Nuestro suelo desaparece! Para arreglarlo añadiremos un segundo suelo que esté
a la derecha del primero, y además resetearemos su posición cuando se hayan
movido el equivalente al ancho de la pantalla, de la siguiente forma:

<pre><code><div style="background: #CCE5FF">...</div>  pos_suelo_x -= 1
  pantalla.blit(superficie_suelo,(pos_suelo_x,450))
<div style="background: #B3FFC6">  pantalla.blit(superficie_suelo,(pos_suelo_x + 288,450))
  if pos_suelo_x <= -288:
    pos_suelo_x = 0</div>
  pygame.display.update()
  reloj.tick(60)
</></code></pre>

Si volvemos a ejecutar nuestro juego veremos que el suelo se mueve pero sin
desaparecer de nuestra pantalla.

![screen5](images/screen5.gif)
