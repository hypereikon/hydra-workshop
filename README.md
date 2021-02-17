# Olivia Jack - Workshop: Code and Democratic Tools
Intersections, Feminism, Technology & Digital Humanities network (IFTe http://ifte.network/)
13.11.2020
led by [Olivia Jack](https://ojack.xyz)

Traducido por hypereikon

## Intro a hydra
Hydra es una plataforma basada en el navegador para el live-coding de visuales, donde cada ventana de navegador conectada puede ser usada como un nodo de un sintetizador modular de video y distribuido.

Construido usando WebRTC (streaming web peer-to-peer) y WebGL, hydra permite a cada navegador/dispositivo/persona conectado sacar una señal de video o stream, y recibir y modificar streams de otros navegadores/dispositivos/personas. La API esta inspirada por la sintesis modular analoga, donde multiples fuentes de video (osciladores, camaras, ventanas de aplicaciones, etc) pueden ser transformadas, moduladas y compuestas mediante la combinacion de secuencias de funciones.
## Para empezar
Anda a [https://hydra.ojack.xyz](https://hydra.ojack.xyz) (se recomienda Chrome o Chromium) y sigue las instrucciones en pantalla.

Otros comandos del editor:
* CTRL-Enter: ejecutar una linea de codigo
* CTRL-Shift-Enter: ejecutar todo el codigo (ademas lo guarda en la URL)
* ALT-Enter: ejecutar un bloque de codigo
* CTRL-Shift-H: ocultar o mostrar codigo
* CTRL-Shift-F: formatear el codigo usando [Prettier](https://prettier.io/)
* CTRL-Shift-S: guardar un screenshot y descargarlo como archivo
* CTRL-Shift-G: compartir en la libreria de usuarios (si esta disponible). Shares to [@hydra_patterns](https://twitter.com/hydra_patterns)
### Funciones basicas
Renderiza un oscilador con parametros de frecuencia, velocidad y offset rgb. Escribe lo siguiente en el editor y presiona 'CTRL-shift-Enter'
```
osc(2, 0.1, 0.8).out()
```
osc() renderiza el oscilador, y .out() hace que se muestre en la pantalla.

girar el oscilador 0.8 radianes
```
osc(20, 0.1, 0.8).rotate(0.8).out()
```
pixelar la salida de la funcion anterior
```
osc(20, 0.1, 0.8).rotate(0.8).pixelate(20, 30).out()
```
Intenta experimentar con cualquiera de las funciones bajo las categorias de 'source', 'geometry' o 'color' [aqui](https://ojack.xyz/hydra-functions/).
Cada cadena de funciones debe siempre empezar con una funcion de 'source', y despues aplicarle cualquier funcion o secuencia de funciones de 'color' y 'geometry'.
```
gradient().rotate(0, 2.6).repeat(3, 3, 0.5).out()
```
#### Usar fuentes externas
Ademas de las fuentes internas como osc(), shape(), gradient() y noise(), hydra puede usar fuentes externas como una webcam, ventanas de aplicaciones, videos, imagenes.
Hay cuatro buffers de entrada que pueden conectarse a fuentes externas, llamados s0, s1, s2 y s3.

inicializar webcam en el buffer de entrada 's0':
```
s0.initCam() 
```
renderiza la salida de s0 a la pantalla
```
src(s0).out()
```
kaleidoscopio webcam:
```
s0.initCam() // inicializa una webcam en el buffer de entrada s0
src(s0).kaleid(4).out() // transforma la webcam a un kaleidoscopio
```
Tener en cuenta que 's0.initCam()' solo necesita ejecutarse una vez para inicializar la webcam. Puedes borrar la linea una vez ya ejecutada.

Tambien puedes usar pestañas del navegador o ventanas de otras aplicaciones como una fuente de entrada (solo en navegadores basados en chrome):
```
s1.initScreen()
src(s1).out()
```
### Mezclar distintas fuentes
Por defecto, hydra permite generar cuatro distintos buffers de salida que pueden renderizar diferentes visuales. Los outputs son o0, o1, o2 y o3.

renderizar al buffer de salida o1:
```
osc(8, 0.1, 0.3).out(o1)
render(o1) // renderiza los contenidos de o1
```
Si no se especifica el buffer de salida en out(), es renderizado al buffer o0.
Para mostrar todos los buffers a la vez:
```
render()
```
Renderiza la webcam al buffer de salida o0, y un oscilador al buffer de salida o1:
```
s0.initCam() // inicializar la webcam en el buffer de entrada s0
src(s0).out(o0) // renderizar la webcam al buffer de salida o0
osc(8, 0.1, 0.3).out(o1)
render() // mostrar las cuatro salidas
```
Los buffers de salida puedes ser mezclados y compuestos.
Empieza con el buffer o0, mezclalo con o1, y muestra el resultado en o2:
```
src(o0).blend(o1).out(o2)
```
La funciones para mezclar blend(), diff(), mult() y add() performan operaciones aritmeticas para combinar la textura de entrada con la textura base, parecido a los modos de blend en photoshop. Revisa las funciones 'blend' en https://ojack.xyz/hydra-functions/

Tambien puedes mezclar multiples entradas en una misma salida (sin necesariamente renderizar a otro buffer):
```
osc(10)
  .rotate(0.5)
  .diff(osc(200))
  .out()
```
### Modulacion
Mientras que las funciones de mezclar combinan los colores de distintas fuentes, las funciones de modulacion usan los colores de una textura para afectar las coordinadas de la otra textura.
Esto crea un tipo de displacement o efecto de warping.

modulate(textura, cantidad) usa el canal rojo y verde de la textura de entrada para modificar las coordinadas x e y de la textura base. Mas informacion en: https://lumen-app.com/guide/modulation/
```
osc(21, 0).modulate(o1).out(o0)
osc(40).rotate(1.57).out(o1)
```
Tambien puedes mezclar multiples fuentes:
```
osc(10)
  .rotate(0.5)
  .diff(osc(200))
  .out()
```
## Conectarse a streams remotos
Cualquier instancia de hydra puede usar otras instancias de hydra como fuente de entrada. Hydra usa webrtc (real time webstreaming) para compartir streams de video ente ventanas. EL modulo incluido rtc-patch-bay gestiona las conexiones entre ventanas conectadas, y tambien puede ser usado como un modulo por si solo para convertir cualquier sitio web en una fuente para Hydra.

Para empezar, abre hydra simultaneamente en dos ventanas separadas.
En una de ellas, setea el nombre para el stream:
```
pb.setName("ola")
```
El titulo de la ventana deberia cambiar al nombre seteado al ser ejecutado lo anterior.

Desde la otra ventana, inicializas el stream mediante .initStream().
```
s0.initStream("ola")
```
renderizarlo:
```
s0.initStream("myGraphics")
src(s0).out()
```
Las conexiones a veces toman un tiempo en establecerse; puedes revisar la consola del navegador para ver el progreso.
Para ver las fuentes disponibles, ejecuta lo siguiente en la consola:
```
pb.list()
```
## Links and resources
#### Hydra resources:
* [list of hydra functions](https://ojack.xyz/hydra-functions/)
* [documentation on github](https://github.com/ojack/hydra)
* [tutorials and examples](https://github.com/ojack/hydra/tree/master/examples)
* [gallery of user-generated sketches](https://twitter.com/hydra_patterns?lang=es)
* [Hydra Book](https://hydra-book.naotohieda.com/#/)
* [a talk about the motivations for creating hydra](https://www.youtube.com/watch?v=cw7tPDrFIQg).
 #### Libraries and tools used:
 * [Regl: functional webgl](http://regl.party/)
 * glitch.io: hosting for sandbox signalling server
 * codemirror: browser-based text editor
 * simple-peer
 ### Inspiration:
 * Space-Time Dynamics in Video Feedback (1984). [video](https://www.youtube.com/watch?v=B4Kn3djJMCE) and [paper](http://csc.ucdavis.edu/~cmg/papers/Crutchfield.PhysicaD1984.pdf) by Jim Crutchfield about using analog video feedback to model complex systems.
 * [Satellite Arts Project (1977) - Kit Galloway and Sherrie Rabinowitz](http://www.ecafe.com/getty/SA/)
 * [Sandin Image Processor](http://www.audiovisualizers.com/toolshak/vidsynth/sandin/sandin.htm)
 * [kynd - reactive buffers experiment](https://kynd.github.io/reactive_buffers_experiment/)
 #### Related projects:
 * [Lumen app (osx application)](https://lumen-app.com/)
 * [Vsynth (package for MaxMSP)](https://cycling74.com/forums/vsynth-package)
 * [VEDA (VJ system within atom)](https://veda.gl/)
 * [The Force](https://videodromm.com/The_Force/)
