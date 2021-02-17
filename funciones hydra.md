##generadores de textura
oscilador
osc(frecuencia,velocidad,color)
	frecuencia default (60)
	velocidad default (0.1)
	color default (0)
			
ruido
noise(escala,velocidad)
	escala default (10)
	velocidad default (0.1)

formas
shape(lados,radio,suavizado)
	lados default (3)
	radio default (0.3)
	suavizado default (0.01)
			 
voronoi
voronoi(escala,velocidad,blending)
	default (5)
	default (0.3)
	default (0.3)

color
solid(r,g,b)

camara web
s0.initCam()
src(s0).out

stream por web
pb.setName("ola123") // setear nombre del stream en una ventana
s0.initStream("ola123") // recibir el stream en la otra ventana

feedback
.operador(src(o0))

operadores
adicion
.add(textura,cantidad)
	cantidad default (0.5)
  
combinacion
.blend(textura,cantidad)
	cantidad default (0.5)

diferencia
.diff(textura)

multiplicacion
.mult(textura,cantidad)	
	cantidad default (1)

manipulacion de variables
	 
oscilacion entre 0 y 1
	()=>Math.sin(time)
se puede modificar el rango y la duracion del oscilador
	 ()=>0.7*Math.sin(time/11)
Ted Davis tiene más ejemplos los cuales se podrian aplicar a hydra (https://ffd8.github.io/oscillation-sandbox/)
	Se le debe anteponer Math. a las funciones (sin,cos,etc) y cambiar la x por time

secuencia
se puede crear una secuencia de valores, se puede ajustar su velocidad y suavizado 
	[n1,n2,n3,n4].fast(1).smooth()

geometria
kaleidoscopio
.kaleid(n° de lados)
	n° de lados default (4)

pixelacion
.pixelate(n° de pixeles en X, n° de pixeles en Y)
	n° de pixeles X default (20)
	n° de pixeles Y default (20)

repeticion
.repeat(n° repeticiones en x, n° repeticiones en y, desplazamiento en x, desplazamiento en y)
	n° de repeticiones en X (e Y) default (3)
	desplazamiento en X (e Y) default (0)

repeticion en X (o Y)
.repeatX(n° repeticiones, desplazamiento)
	n° de repeticiones default (3)
	desplazamiento default (0)

rotacion
.rotate(angulo en radianes,velocidad)
	angulo en radianes default (10)
	velocidad default (0)

escala
.scale(tamaño global, multiplo en X, multiplo en Y)
	tamaño global default (1.5)
	multiplo en X default (1)
	multiple en Y default (1)


desplazamiento en X (o Y)
.scrollX(cantidad de desplazamiento,velocidad)
	desplazamiento default (0.5)
	velocidad default (0)

moduladores
modulacion
.modulate(textura,cantidad)
	cantidad default (0.1)

modulacion del tono (hue)
.modulateHue(textura con color,cantidad)
	cantidad default (0.1)

modulacion de kaleidoscopio
.modulateKaleid(textura,n° de lados)
	n° de lados default (4)

modulacion de pixelacion
.modulatePixelate(textura, cantidad de modulacion, cantidad de pixeles por lado)
	cantidad de modulacion default (10)
	cantidad de pixeles por lado default (3)

modulacion de repeticion
.modulateRepeat(textura, n° de repeticiones en X, n° de repeticiones en Y, desplazamiento en X, desplazamiento en Y)
	n° de repeticiones en X (e Y) default (3)
	desplazamiento en X (e Y) default (0.5)
		
modulacion de repeticion en X (o Y)
.modulateRepeatX(textura, n° de repeticiones, desplazamiento)
	n° de repeticiones default (3)
	desplazamiento default (0.5)
		
modulacion de rotacion 
.modulateRotate(textura, cantidad de modulacion, rotacion pre-modulacion en radianes)
	cantidad de modulacion default (1)
	rotacion pre-modulacion default (0)

modulacion de escala
.modulateScale(textura, cantidad de modulacion, escala pre-modulacion)
	cantidad de modulacion default (1)
	escala pre-modulacion default (1)

modulacion de desplazamiento en X (o Y)
.modulateScrollX(textura, cantidad de modulacion, velocidad desplazamiento pre-modulacion)
	cantidad de modulacion default (0.5)
	velocidad desplazamiento pre-modulacion default (0)

efectos de color
brillo
.brightness(cantidad)
	cantidad default (0.4)

contraste
.contrast(cantidad)
	cantidad default (1.6)

color
.color(r,g,b)

invertir
.invert(cantidad)
	cantidad default (1)

tono(hue)
.hue(cantidad)

luminosidad
.luma(umbral,tolerancia)
	umbral default (0.5)
	tolerancia default (0.1)

saturacion
.saturate(cantidad)
  	cantidad default (2)
