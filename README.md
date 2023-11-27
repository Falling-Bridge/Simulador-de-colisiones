Simulador de Colisiones

Creador: 
	Lucas Daniel Morales Oyanedel

Colaboradores:
	Antonio Benavides Puentes
	Pablo Villagran 


Instrucciones de uso

Dado que para el proyecto me he visto forzado a modificar la libería 'SDL_Rect.', para poder compilar y ejecutar el código, deberá usar la librería que está
en la carpeta 'librería' del link de github.

los siguientes son comandos modificables en el archivo makefile:

para compilar el código, se deberá usar el comando 'mingw32-make compilar'
para ejecutar el código, se deberá usar el comando 'mingw32-make run'
para usa gdb en el código, se deberá usar el comando 'mingw32-make debug'

Explicación del código

Se utiliza el tipo de struct 'SDL_Rect *particulas, en donde los parametros son los siguientes:

	particulas.x (posición en x) : esto se regula con un '%' de la dimension 'ancho' de la pantalla del usuario (cuando se lee un archivo y cuando se
                                 agrega una partícula con el comando explicado más abajo en visualizar)
        particulas.y (posición en y) : esto se regula con un '%' de la dimension 'ancho' de la pantalla del usuario (cuando se lee un archivo y cuando se 				       
                                       grega una partícula con el comando explicado más abajo en visualizar)
        particulas.h (altura de la partícula) : esto se asigna mediante el valor definido de 'size' (que puede ser modificado, cambiando el valor que representa)
        particulas.w (ancho de la partícula) : esto se asigna mediante el valor definido de 'size' (que puede ser modificado, cambiando el valor que representa)
        particulas.d (direccion que tiene la partícula) : esto se regula con un '%8', dado que se entienden como direcciones posibles (derecha == 0, arriba
                                                          derecha == 1, arriba == 2, arriba izquierda == 3, izquierda == 4, abajo izquierda == 5,
                                                          abajo == 6, abajo derecha == 7. Que se suman a 8 valores posibles por ende el '%8)
        particulas.p (peso que tiene la partícula) : esto se define mediante un '%weigth' cuando se lee un archivo y cuando se agrega una partícula, con un
                                                     rand() % weigth + 1 (para que pueda tomar el valor de weigth y sea distinto de 0)

En el código tengo las siguientes funciones:
	-leercsv
	-leerbinario
	-leertxt
	-Estadisticas
	-Movimiento
	-visualizar
	-main

Explicación de cada función: (se usa unsigned int para asegurar que son enteros positivos los datos que entran)

	leercsv:

		Se llama a leercsv, cuando se detecta que la variable 'caracter' es igual a 'c'
		La función posteriormente procede a buscar enteros con el siguiente formato:
			%u,%u,%u,%u (parametro.x , parametro.y, parametro. d, parametro.p) (posisicón en x, posición en y, direccion, peso)
		y lo hace hasta que ya no encuentre más caracteres en ese formato o esté en el final del archivo

	leerbinario:
	
		Se llama a leerbinario, cuando se detecta que la variable 'caracter' es igual a 'b'
		la funcion posteriormente procede a tomar grupos de 32 enteros compuesto de'0's y '1's y los almacena en el arreglo  'grupo'. Se usa
    corrimiento de 	bits para obtener el valor en base10 de grupo (que se tratan como numeros en base2) y se almacena en la variable 'decimal',
    que luego se asocia al 	struct

		Se usa el valor de cuenta para ir agregando los valores correspondientes al struct:
			cuenta == 0 ; grupo[32] = particulas.x; posición en x
			cuenta == 1 ; grupo[32] = particulas.y; posición en y
			cuenta == 2 ; grupo[32] = particulas.d; direccion
			cuenta == 3 ; grupo[32] = particulas.p; peso

	leertexto:
		Se llama a leertexto, cuando se detecta que la variable 'caracter' es igual a 't'
		la función procede a buscar enteros en el formato (%u,%u,%u,%u) 
    (parametro.x , parametro.y, parametro. d, parametro.p) (posisicón en x, posición en y, direccion, peso)

si se detecta que la variable 'caracter' es distinta de 'c' o 'b' o 't'; no se ejecutará el resto del código y saldrá el mensaje 
"Archivo no tiene el formato especificado\n"

	Estadisticas:

		El único propósito de esta función es mostrar los valores actuales de cada variable, sean estas : 
			'cantidadparticulas' 'cantidadcolisiones' 'speed' 'delay'
		Para esto se crea una 'línea' en la esquina superior izquierda en la que se muestran seguidos los valores

	movimiento:

		Esta es la función regula todo el movimiento de la partícula:
			 -se usa el valor de particula[i].d para decidir como se moverá a través de la pantalla mediante el switch que lo tiene como parámetro,
        esto es donde está el comentario '//Como se mueve'
			 -si colisiona contra uno de los bordes, se usa la porción de codigo con el comentario '// Manejar rebotes en los bordes'
			  que decidirá una direccion nueva (y por ende un nuevo tipo de movimiento por la pantalla), dependiendo de en que borde colisiona
			 -si colisionan entre ellas (una partícula ocupa el mismo espacio que otra), se les quitará una cantidad de 1 al parámetro
        particula.p (a la particula [i] y [j]), después de colisionar se decide una dirección nueva (que afectará como se traslado por la pantalla):
				  -si el peso de la particula en la posición [i] es mayor, la particula[j] decide una nueva dirección
				  -si el peso de la particula en la posición [j] es mayor, la particula[i] decide una nueva dirección
				  -si el peso es igual para ambas particulas, la particula[j] y la particula[i] deciden una nueva dirección

	visualizar:
		
		Esta funcion es la encargada de renderizar todos los procesos que vienen desde 'movimiento' y los datos que se obtienen de las funciones 		leercsv/binario/txt. aparte cuenta con unos comandos activados mediantes pulsaciones de teclas, que son los siguientes:

			-si se presiona la tecla 'q', se finalizaran todos los procesos que esten activos y se terminará la ejecución del código
			-si se presiona la tecla '1', se cambiará el color que tiene el fondo y las partículas
			-si se presiona la tecla '+', se agregará una particula en una posición random de la pantilla, con atributos fijos de tamaño y peso 
       (estos últimos pueden variar modificandose las definiciones de size y weigth respectivamente)
			-si se presiona la tecla '-', se elimina la última partícula agregada al struct de particulas
			-si se presiona la tecla '2', se 'eliminan' (se libera el espacio de memoria) todas las partículas en pantalla
			-si se presiona la tecla '3', se le agrega una cantidad de entre 0 a 14 a la variable 'speed'
			-si se presiona la tecla '4', se le quita una cantidad de entre 0 a 14 a la variable 'speed'
			-si se presiona la tecla 's', se guarda la posición actual de la particulas en el archivo de nombre 'particuladedios' en la carpeta 
       'Colisionador de Adrones' y se terminan todos los procesos. El archivo creado se leerá como csv
			-si se presiona la tecla '5', se mostrarán las estádisticas, sea cantidad de particulas, cantidad de colisiones, speed, delay
			-si se presiona la tecla '6', se ocultarán las estádisticas
			-si se presiona la tecla '7', se le agregar una cantidad de 1 a la variable delay (que es cuan rápido ocurre un proceso)
			-si se presiona la tecla '8', se le quitará una cantidad de 1 a la variable delay, mientras esta sea mayor a 1

	main: 
		-Se encarga de llamar a las funciones leer
		-Se encarga de crear una ventana que se adecua al tamaño de pantalla del usuario 
		-Se encarga de crear el renderer que se usa en visualizar
		-se encarga de crear el puntero *font que se usa en la función Estadísticas
		-se encarga de que todos los parámetros leidos por las funciones leer estén dentro de los límites tanto de la pantalla ('.x'  y '.y') y que 		
     los demás parámetros esten dentro de las limitaciones establecidas por size, weigth y la definición de dirección ('.h' '.w', '.p' y '.d')
		-se encarga de llamar a visualizar (que es la función encargada de terminar con todos los procesos si sucede una condición de termino, como
     cuando se presiona la tecla 'q' o 's')
		

	

