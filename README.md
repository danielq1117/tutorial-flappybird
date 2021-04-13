# FlappyBird en Python con Pygame

## Instalación de pygame

Para instalar pygame en nuestro ordenador, ejecutaremos el símbolo del sistema como administrador.

![cmd](images/cmd.png)

Enseguida ejecutaremos el siguiente comando:

`pip install pygame`

y esperaremos a que se instale pygame en nuestro equipo.

## Creación del juego

### Preparación del entorno de trabajo

En una carpeta nueva crearemos insertaremos los
[recursos](Recursos.zip 'Recursos.zip')
para el proyecto, que consisten en las imágenes, los sonidos y la fuente para el
mismo.

Enseguida crearemos en nuestra carpeta el archivo en donde escribiremos el
código, que en mi caso será `flap.py`. De esta forma, la estructura del proyecto
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

```python
import pygame, sys, random
```

Lo ejecutaremos en el símbolo del sistema de la siguiente forma:

`python flap.py`

Y como resultado nos debe mostrar algo así:

```
pygame 2.0.1 (SDL 2.0.14, Python 3.9.1)
Hello from the pygame community. https://www.pygame.org/contribute.html
```

### Creando la ventana del juego

Enseguida escribiremos lo siguiente en `flap.py`:

```python
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
```

El código anterior se divide en dos partes. La primera es donde inicializaremos
el juego, y la segunda es el ciclo, en donde se actualizará el estado del juego
constantemente, y que consiste en el ciclo `while True:`.

Con `pygame.init()` inicializamos pygame,
con `pantalla = pygame.display.set_mode((288,512))` establecemos el ancho y alto
de nuestra ventana, y con `reloj = pygame.time.Clock()` creamos el reloj con el
que controlaremos el tiempo en el juego.

Dentro de nuestro ciclo detectamos todos los eventos, y si el evento es
`pygame.QUIT`, que se activa cuando cerramos la ventana del juego, se finaliza
pygame y se cierra el juego con `sys.exit()`.

Al final de nuestro ciclo actualizamos la ventana del juego y limitamos la
velocidad a la que este se ejecuta a 60 ciclos por segundo.

Si ejecutamos nuestro juego ahora lo único que veremos será una pantalla en
negro, que podremos cerrar ya que detectamos el evento `pygame.QUIT`.

![screen1](images/screen1.png)

### Añadiendo el fondo

Para añadir el fondo crearemos una superficie, que añadiremos a nuestra pantalla
en cada ciclo para que siempre se muestre, de la siguiente forma:

```python
...
reloj = pygame.time.Clock()

superficie_fondo = pygame.image.load('assets/background-day.png').convert()

while True:
...
      sys.exit()

  pantalla.blit(superficie_fondo, (0,0))

  pygame.display.update()
...
```

Con la línea `superficie_fondo = pygame.image.load('assets/background-day.png').convert()`
guardamos la imagen que utilizaremos como fondo en una variable para poderla
utilizar, y en la línea `pantalla.blit(superficie_fondo, (0,0))` ponemos esa
imagen en la pantalla, estableciendo sus coordenadas.

Si ejecutamos nuestro juego de nuevo, ahora nos mostrará una ventana con nuestro
fondo.

![screen2](images/screen2.png)

### Agregando el suelo

Para agregar el suelo haremos algo muy parecido a lo que hicimos con el fondo:

```python
...
superficie_fondo = pygame.image.load('assets/background-day.png').convert()
superficie_suelo = pygame.image.load('assets/base.png').convert()

while True:
...
  pantalla.blit(superficie_fondo, (0,0))

  pantalla.blit(superficie_suelo,(0,450))
...
```

Si ejecutamos nuestro código veremos algo así:

![screen3](images/screen3.png)

Ahora vamos a añadirle movimiento. Para ello crearemos una variable en donde
guardaremos la posición horizontal del suelo y la moveremos un poco a la
izquierda en cada ciclo, de la siguiente forma:

```python
...
superficie_suelo = pygame.image.load('assets/base.png').convert()
pos_suelo_x = 0

while True:
...
  pantalla.blit(superficie_fondo, (0,0))

  pos_suelo_x -= 1
  #pantalla.blit(superficie_suelo,(0,450))
  pantalla.blit(superficie_suelo,(pos_suelo_x,450))
...
```

Si ejecutamos nuestro juego veremos lo siguiente:

![screen4](images/screen4.gif)

¡Nuestro suelo desaparece! Para arreglarlo añadiremos un segundo suelo que esté
a la derecha del primero, y además resetearemos su posición cuando se hayan
movido el equivalente al ancho de la pantalla, de la siguiente forma:

```python
...
  pantalla.blit(superficie_suelo,(pos_suelo_x,450))
  pantalla.blit(superficie_suelo,(pos_suelo_x + 288,450))
  if pos_suelo_x <= -288:
    pos_suelo_x = 0
...
```

Si volvemos a ejecutar nuestro juego veremos que el suelo se mueve pero sin
desaparecer de nuestra pantalla.
