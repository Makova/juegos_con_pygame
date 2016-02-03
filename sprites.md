##Sprites

Al utilizar una serie de imágenes (la clase `Surface`) para manipularlas y redibujarlas en pantalla, hemos estado usando lo que se conoce como [sprites](http://es.wikipedia.org/wiki/Sprite_%28videojuegos%29).

En principio, podemos segur así y usar las herramientas de que nos provee la clase `surface` para manejar los sprites, modificarlos, moverlos, etc.

Pero **Pygame**, sin embargo, nos ofrece una clase `Sprite` para facilitarnos ese trabajo. Mas que para usarla directamente (que también se puede, como veremos) `Sprite` está concebida como una clase a partir de la que derivar nuestras propias clases.

> La clase `Sprite` forma parte de [la librería de **Pygame** sprite](http://www.pygame.org/docs/ref/sprite.html)

Los atributos más importantes de la clase `Sprite` son `image` y `rect`. El atributo `image` es un objeto `Surface` y sirve, lógicamente, para almacenar la apariencia de nuestro sprite. El atributo `rect` es un objeto `rect` que indica el rectángulo que contiene nuestro sprite y, como veremos enseguida, nos será muy útil para cosas como posicionarlo, calcular colisiones, etc.

Veamos un ejemplo sencillo de creación de un sprite (supongamos que es un monstruo para un juego):

```
# Creamos nuesto sprite:
mi_monstruo = pygame.sprite.Sprite()
# Le asignamos una imagen...
mi_monstruo.image = ImagenMonstruosa
# ...y un rectángulo
mi_monstruo.rect = mi_monstruo.image.get_rect()

# Posicionamos el monstruo:
mi_monstruo.rect.topleft = (100, 100)

# Y hacemos blit de la imagen y del rectángulo
Ventana.blit(mi_monstruo.image, mi_monstruo.rect)
```

En la primera línea creamos nuestro objeto *mi_monstruo* como una instancia de la clase pygame.sprite.Sprite

Le asignamos una imagen (que debe ser un objeto surface) y, seguidamente, un rectángulo (objeto rect).

Para asignar el objeto rect hemos usado un pequeño truco muy útil: Dado que el rectángulo de nuestro sprite va a ser del mismo tamaño que su imagen (esto no tiene por qué ser necesariamente cierto, pero normalmente sí lo es), usamos el método `get_rect()` de la clase surface para extraer el rectángulo de la imagen del sprite.

Ahora aprovechamos la propiedad topleft de rect para situar nuestro rectángulo (y, por tanto el sprite, con imagen y todo) en la pantalla.

el atributo `topleft` nos indica la esquina superior izquierda del sprite, pero `rect` tiene algunos métodos más para indicar la ubicación como, por ejemplo, `center`, donde indicamos el centro del sprite. 

> Todo el tratamiento de posiciones, colisiones, superposiciones, etc de nuestros sprites se hace de manera muy fácil gracias a la clase Rect. Puedes ver más detalles en [la documentación oficial](http://www.pygame.org/docs/ref/rect.html) 

Un detalle importante es que, cuando hacemos blit, lo hacemos de la imagen del sprite pasándole como coordenadas su rectángulo.

Pero hemos dicho que lo mejor es usar la clase sprite para crear nuestras propias clases, por lo que nuestro código, un poco mejorado, podría ser así: 

```
class Monstruo(pygame.sprite.Sprite):

    def __init__(self, coordenadas, imagen):
        pygame.sprite.Sprite.__init__(self)
        self.image = imagen
        self.rect = self.image.get_rect()
        self.rect.topleft = coordenadas

   def update(self, nuevas_coordenadas):
        self.rect.topleft = nuevas_coordenadas

mi_monstruo = Monstruo((100, 100), ImagenMonstruosa)
```

Hemos hecho lo mismo del ejemplo anterior pero creando una nueva clase Monstruo. Además, estamos usando el método `update` (que existe en la clase `Sprite` pero no hace nada por defecto) que nos resultará muy útil para actualizar todos los cambios  que sean necesarios en nuestro sprite (en este caso, lo usamos para cambiar la posición).

Documentación oficial de [la clase Sprite](http://www.pygame.org/docs/ref/sprite.html#pygame.sprite.Sprite)

Otra clase muy útil de la libería sprite es `Group`, que sirve para crear un objeto contenedor en el que agrupar varios sprites. Muchos de los métodos que se pueden usar para los sprites (especialmente los que tienen que ver com movimiento o colisiones) tienen versiones específicas para grupos de objetos.

De este modo, no hay que manejar uno por uno todos los aspectos de todos los sprites de, por ejemplo, las naves enemigas en un juego tipo *Space Invaders*.

Documentación oficial de [la clase Group](http://www.pygame.org/docs/ref/sprite.html#pygame.sprite.Group)
