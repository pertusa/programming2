#Tema 3 - Ficheros

En este tema veremos como trabajar con ficheros de texto y también con ficheros binarios. Aprenderemos a realizar las operaciones básicas: apertura, lectura, escritura y cierre. Además veremos los principales problemas que nos podemos encontrar al trabajar con ficheros y sus soluciones.

##Ficheros de texto

###Definición

Los ficheros de texto (también llamados **ficheros con formato**) son aquellos cuyo contenido solo son caracteres imprimibles (en código ASCII son los caracteres a partir del código 32, el cual corresponde al espacio en blanco: ' '). Éstos pueden estar creados con diversos juegos de caracteres: ASCII (el código por defecto de los ficheros de texto), EBCDIC, Unicode (en su codificación más comúnmente usada, UTF-8).
Un juego de caracteres o código de caracteres consiste en asignar un número a cada símbolo escrito: letras, números, símbolos, etc.
Su contenido no tiene porque seguir ningún formato o disposición especial (no se puede 'decorar' el texto). En algunos casos tienen caracteres especiales para indicar los finales de linea (EOLN por *End of line*) y para señalar el final de fichero (EOF por *End of File*).

Ejemplos de ficheros de texto:

* Un programa en C++, concretamente el fichero que contiene su código fuente sin compilar
* Una página web, cuyo contenido está escrito en HTML
* Un fichero de configuración de un sistema operativo (.ini en windows, .conf en Unix)
* Un fichero 'makefile' de dependencias de compilación de un proyecto en C++

###Declaración

Para poder trabajar con ficheros de texto, debemos incluir la biblioteca de funciones *fstream*:
```cpp
#include <fstream>
```
Para declarar una variable de tipo fichero tenemos varias opciones, según el uso que le vayamos a dar.

* Si solo queremos acceder para leer el contenido del fichero:
```cpp
ifstream fich_lectura;
```
* Si solo vamos a usar el fichero para escribir en él:
```cpp
ofstream fich_escritura;
```
* Si necesitamos acceder para ambas operaciones (lectura y escritura)
```cpp
fstream fich_txt_lect_escr;
```
Aunque usar ficheros de texto en ambos modos simultáneamente es algo poco frecuente.

###Operaciones de apertura y cierre

Para trabajar con un fichero, es necesario abrirlo, y para ello se usa la función
```cpp
open(const char[] nombre, const int modo)
```
Por ejemplo:
```cpp
ifstream fichero; // declaramos la variable del fichero
const char nombre[]="mifichero.txt"; // declaramos una variable con el nombre del fichero
fichero.open(nombre,ios::in); // abrimos el fichero en modo lectura
```
Si tenemos el nombre del fichero almacenado en una variable de tipo *string*, entonces deberemos usar la función de conversión ```c_str()```, por ejemplo:
```cpp
ifstream fichero; // variable del fichero
string nombre="mifichero.txt"; // variable tipo string con el nombre del fichero
fichero.open(nombre.c_str(),ios::in); // abrimos el fichero en modo lectura
```
En función de como vayamos a trabajar con el fichero (sólo lectura, sólo escritura, lectura y escritura, añadir al final, etc.), tendremos que utilizar un modo de apertura diferente. En C++ tenemos los siguientes modos de apertura:

| Modo | Observaciones | Constante en C++ |
| -- | -- | -- |
| Lectura | Sólo consultas, coloca el cursor al principio | ```ios::in``` |
| Escritura | Sólo escritura, colocando el cursor al principio, puede sobreescribir contenidos | ```ios::out``` |
| Lectura y escritura | Permite leer y escribir contenidos en el fichero | <code>ios::in &#124; ios::out</code> |
| Añadir al final | Sólo escritura y coloca el cursor al final del fichero para añadir contenidos | <code>ios::out &#124; ios::app</code> |
| Ir al final | Este modo permite moverse al final del fichero tras abrirlo | ```ios::ate``` |
| Fichero binario | Este modo permite abrir ficheros binarios, lo veremos en el siguiente apartado | ```ios::binary``` |
| Truncar/eliminar contenido | Este modo permite borrar TODO el contenido del fichero tras abrirlo en modo escritura | ```ios::trunc``` |

Existe una forma de realizar una declaración y apertura de un fichero con una sola instrucción.
```cpp
ifstream fichero("datos.txt");
```
Con esta instrucción abrimos un fichero llamado *datos.txt* en modo lectura (```ios::in```) y lo asignamos a la variable ```fichero```.
```cpp
ofstream ficheroSalida("resultado.txt");
```
Esta instrucción nos permite abrir un fichero llamado *resultado.txt* en modo escritura (```ios::out```) y queda asignado a la variable ```ficheroSalida```.

La operación de abrir un fichero es delicada en tanto que puede producir errores en tiempo de ejecución. Por lo que es importante, tras intentar abrir un fichero, comprobar si éste está correctamente abierto y listo para trabajar. Con la función ```is_open()``` podemos verificar si efectivamente nuestro fichero está abierto o no.
```cpp
if (fichero.is_open()) {
   // ya se puede trabajar con el ...
} else {
   // error de apertura
}
```
Finalmente, tras trabajar con un fichero, tenemos que cerrarlo, para liberar recursos y permitir que otros procesos/programas puedan trabajar con el. Para ello, usamos la función ```close()```:
```cpp
fichero.close();
```
###Lectura de ficheros de texto
####Detección de final de fichero
Durante el trabajo (consulta de contenidos) con un fichero tenemos que saber en qué momento hemos llegado al final del mismo y parar de leer (para no incurrir en un error). Para saber si estamos en el final de un fichero tras una o varias operaciones de lectura, se usa el método ```eof()```. Por tanto la forma típica de recorrer el contenido de un fichero es mediante un bucle que tras cada operación de lectura, comprueba si estamos al final del fichero:
```cpp
ifstream fichero;
...
while(!fichero.eof()) { ... }
```
Éste método devuelve un valor booleano (*true* o *false*) en función de si estamos o no al final. Cuando realizamos una operación de lectura de un dato (carácter, número, etc.) que no está en el fichero, devuelve *true*. Pero, **cuidado**, despues de haber leido **el último dato válido** todavía devolverá *false*. Por tanto, hace falta realizar una lectura adicional (cuyo resultado se debe descartar, pues son datos no válidos) para provocar la detección del final de fichero, y por tanto que la función retorne *true*.
####Ejemplos de lectura de un fichero
Vamos a ver varias formas de leer el contenido de un fichero de texto:
#####Primer ejemplo
```cpp
...
if (fichero.is_open()) {
   string s="";
   getline(fichero,s);
   // otra opción: fichero.getline(cad,tCAD);
   // siendo ’cad’ de tipo ’char []’ y tCAD el tamaño de la cadena 'cad'.
   while (!fichero.eof()) {
      // hacer algo con 's'
      getline(fichero,s);
   }
   fichero.close();
}
```
Éste código se puede simplificar más. Además el bucle tiene un problema al final, si el fichero finaliza con un '\n' entonces produce una iteración de más. Eso se podría solucionar añadiendo una instrucción *if* que compruebe el contenido de la variable *s* antes de hacer algo con ella, antes de utilizarla.
#####Segundo ejemplo, más compacto
```cpp
ifstream fichero("datos.txt");

if (fichero.is_open()) {
   string s;
   while (getline(fichero,s)) {
      // hacer algo con ’s’
   }
   fichero.close();
}
```
Esta otra forma de leer un fichero es más sencilla y **recomendable**. Aquí no usamos ```eof()``` porque la misma función ```getline()``` cuando no puede leer *'falla'* y devuelve un valor *false* que provoca la salida del bucle.
#####Lectura carácter a carácter
```cpp
ifstream fichero("datos.txt");

if (fichero.is_open()) {
   char c; // Tambien puede ser de otros tipos: int, float, etc.
   while (fichero >> c) {
      // hacer algo con ’c’
   }
   fichero.close();
}
```
Este ejemplo muestra como trabajar con un fichero leyendolo carácter a carácter. Los ficheros en C++ son objetos de la clase *stream*, los cuales permiten ser accedidos como buffers de entrada/salida, usando los operadores ```>>``` y ```<<```, de la misma forma que hacemos con los buffers *cin* y *cout* de entrada/salida estándar.

Otro ejemplo, similar al anterior, leemos el fichero carácter a carácter, pero cuyo contenido tiene espacios en blanco:

```cpp
if (fichero.is_open())   {
   char c;
   while (fichero.get(c)) {
      // hacer algo con ’c’
   }
 fichero.close();
}
```

#####Lectura de ficheros de texto con contenido 'conocido'
En este ejemplo leeremos el contenido de un fichero que tiene una o más lineas cuyo contenido es un *string* y a continuación dos números enteros. Por ejemplo, un fichero con esta estructura seria:
```
hola 123 1024
mundo 43 23
```
El código:
```cpp
ifstream fichero("ejemplo.txt");

if (fichero.is_open()) {
   string s;
   int i,j;
   while (fichero >> s) {// Leer string 
      fichero >> i; // Leer primer numero
      fichero >> j; // Leer segundo numero
      cout << "Read: " << s << "," << i << "," << j << endl;
   }
}
```

Otro ejemplo más:
En este ejemplo leemos los datos de un fichero cuya estructura es: primero un número entero, que indica los nombres que hay a continuación y luego una sucesión de nombres separados por espacios o retornos de carro, por ejemplo:
```
3 pedro antonio jordi
```
Y el código
```cpp
if (fichero.is_open()) {
   string s;
   int cuantos;
   fichero >> cuantos;
   for(int i=0; i<cuantos; i++) {
      fichero >> s;
      cout << "Nombre(" << i+1 << ")=" << s << endl;
   }
   fichero.close();
}
```
Lo primero que hacemos tras abrir el fichero es leer el número para saber cuantos nombres tenemos que leer. A continuación, mediante un bucle *for* leemos tantas *strings* como nos indica *cuantos* y los mostramos en pantalla.

###Escritura de ficheros de texto
Para escribir en un fichero de texto usaremos el operador de volcado ```<<```, porque tal y como dijimos anteriormente, los ficheros son objetos *stream* y pueden ser usados como buffers de entrada/salida de datos.
```cpp
ofstream fich_salida;
...
fich_salida.open("resultados.txt",ios::out);
if (fich_salida.is_open()) {
   fich_salida << "El resultado es: " << numentero << endl;
   ...
   fich_salida.close();
}
```
En este ejemplo, simplemente abrimos un fichero en modo escritura y volvamos o escribimos en él una frase y una variable llamada *numentero*. Se debe tener en cuenta que tal y como está escrito este ejemplo, el contenido del fichero será eliminado en cada ejecución y sustituido por el nuevo resultado.

Otro Ejemplo:
```cpp
ofstream fich_salida;
...
fich_salida.open("resultados.txt",ios::out | ios::app);
if (fich_salida.is_open()) {
   fich_salida << "Datos: ";
   for (int i=0; i<totales; i++) {
      fich_salida << resultados[i] << " ";
   }
   fich_salida << endl; // fin de linea al final del volcado de datos
   ...
   fich_salida.close();
}
```
En este otro ejemplo:

* volcamos una línea de texto con el literal 'Datos: ' seguido de una sucesión de datos obtenidos durante el recorrido de un array llamado 'resultados'. Cada dato estará separado por un espacio en blanco.
* además, el fichero no será truncado en la apertura y su contenido se mantendrá. Los datos de sucesivas ejecuciones serán añadidos al final del mismo.

###Ejercicios

####Ejercicio 3.1:

#####Implementa un programa que lea un fichero llamado 'fichero.txt' e imprima por pantalla las lineas del fichero que contienen la cadena 'Hola'.

####Ejercicio 3.2:

#####Escribe un programa que lea un fichero llamado 'fichero.txt' y escriba en un segundo fichero, llamado 'FICHERO.TXT' el contenido del primero con todas sus letras (carácteres alfanuméricos) pasados a mayúsculas. Ejemplo:

| fichero.txt | FICHERO.TXT |
| --- | --- |
| Hola, mundo. | HOLA, MUNDO. |
| Como estamos? | COMO ESTAMOS? |
| Adios y 1000 veces adios | ADIOS Y 1000 VECES ADIOS |

####Ejercicio 3.3:

#####Haz un programa que lea dos ficheros de texto llamados 'f1.txt' y 'f2.txt' y escriba por pantalla las lineas que sean distintas en cada fichero (número de línea, y '< ' delante de la linea del primer fichero y '> ' precediendo la linea del segundo. Ejemplo:

| f1.txt | f2.txt |
| --- | --- |
| Hola, mundo. | Hola, mundo. |
| Me llamo andreu | Me llamo Andreu |
| Adios, adios | Hasta la vista |

El resultado debe ser:

```
Línea 2:
< Me llamo andreu
> Me llamo Andreu
Línea 3:
< Adios, adios
> Hasta la vista
```

####Ejercicio 3.4:

#####Diseña una función "finfichero" que reciba dos parámetros: el primero debe ser un número entero positivo *n*, y el segundo el nombre de un fichero de texto. La función deberá mostrar en pantalla las últimas *n* lineas del fichero. Ejemplo:

```
finfichero(3, "texto.txt");
```

Tenemos dos opciones para implementar esta función:

* Solución *bestia*: leer el fichero para contar las líneas que tiene, y volver a leer el fichero para escribir las n líneas finales. Problema: si el fichero tiene **muchas lineas**??
* Otra solución: Utilizar un vector de string de tamaño *n* que almacene en todo momento las n últimas líneas leídas (aunque al principio tendrá menos de n líneas)

####Ejercicio 3.5:

#####Dados dos ficheros de texto "f1.txt" y "f2.txt", en los que cada línea es una serie de números separados por ":", y suponiendo que las líneas están ordenadas por el primer número de menor a mayor en los dos ficheros, haz un programa que lea los dos ficheros línea por línea y escriba en el fichero “f3.txt” las líneas comunes a ambos ficheros, como en el siguiente ejemplo:

| f1.txt | f2.txt | f3.txt |
| -------- | -------- | -------- |
| 10:4543:23 | 10:334:110 | 10:4543:23:334:110 |
| 15:1:234:67 | 12:222:222 | 15:1:234:67:881:44 |
| 17:188:22 | 15:881:44 | 20:111:22:454:313 |
| 20:111:22 | 20:454:313 | |

<!-- TODO: poner los demás ejercicios -->

##Ficheros binarios

###Definición

Un fichero binario es una secuencia de bits (habitualmente agrupados de ocho en ocho, es decir, en bytes). En estos ficheros la información se almacena tal y como estan alojados en la memoria del ordenador, no se convierten en caracteres al gardarlos en el fichero. También son llamados *ficheros sin formato*.

Normalmente, para guardar/leer información en un fichero binario se usan registros (structs). De esta forma, es posible acceder directamente al *n-ésimo* elemento sin tener que leer los *n-1* anteriores. Es por esto que se dice que los ficheros binarios son ficheros de acceso directo (o aleatorio) y los ficheros de texto son de acceso secuencial.

###Declaración

Al igual que con los ficheros de texto, para poder trabajar con este tipo de ficheros, debemos incluir la biblioteca de funciones *fstream*:
```cpp
#include <fstream>
```
Para declarar una variable de tipo fichero binario, según el uso que le vayamos a dar, tenemos:

* Solo para lecturas/consultas al fichero:
```cpp
ifstream fich_lectura;
```
* Solo para escribir en él:
```cpp
ofstream fich_escritura;
```
* Si necesitamos acceder para ambas operaciones (lectura y escritura)
```cpp
fstream fich_lect_escr;
```
###Operaciones de apertura y cierre

Para abrir un fichero binario, también usamos la función ```open```, pero tenemos que añadir un modo adicional: ```ios::binary```.

* Apertura para lectura:
```cpp
fich_lectura.open("mifichero.dat", ios::in | ios::binary);
```
* Apertura para escritura:
```cpp
fich_escritura.open("mifichero.dat", ios:out | ios::binary);
```
* Modo abreviado, declaración y apertura en una sola instrucción:
```cpp
ifstream fbl("mifichero.dat", ios::binary); // fichero binaro de lectura
ofstream fbe("otrofichero.dat", ios::binary); // fichero binario de escritura
```
Combinando varios modos de apertura podemos obtener:

* Ficheros binarios de lectura y escritura: ```ios::in | ios::out | ios::binary```
* Ficheros binarios para ascritura/añadir al final: ```ios::out | ios::app || ios::binary```

Finalmente, para cerrar un fichero binario, igual que con los de texto, se usa la función ```close()```.
```cpp
fich_binario.close();
```
###Lectura de ficheros binarios

Para leer ficheros finarios se utiliza la función ```read(registro, tamaño)``` a la que le tenemos que pasar dos argumentos:

* El registro donde se almacenarán los datos tras la operación de lectura.
* La cantidad de bytes que deseamos leer. La cual se obtiene calculando el tamaño del registro con la función ```sizeof()```. 

En el siguiente ejemplo leemos el contenido completo de un fichero binario formado por registros de tipo *TIPOCIUDAD*.

```cpp
typedef struct { ... } TIPOCIUDAD;
...
TIPOCIUDAD ciudad;
ifstream fbl;

fbl.open("mifichero.dat",ios::in | ios::binary);

if (fbl.is_open()) {
   while (fbl.read((char *)&ciudad, sizeof(ciudad))) {
      // procesar ’ciudad’
   }
   fbl.close();
}
```
Una vez comprobado que el fichero ha sido correctamente abierto, leemos su contenido mediante un bucle, invocando a cada iteración a la función *read*, pasándole como argumentos el registro *ciudad* y su tamaño. En el cuerpo del bucle *while* trabajaremos con los datos de la ciudad recien leida. Finalmente, cuando lleguemos al final de fichero, el *while* finalizará y cerraremos el fichero.

`NOTA: Para leer el contenido de un fichero binario, tenemos que saber su estructura (el tipo de resgistro o *struct* usado para almacenar datos en él).`

También podemos acceder a un elemento directamente calculando su posición en el fichero en función del tamaño de los datos y la cantidad de elementos que hay antes. Para ello usamos la función ```seekg(posición, desde)```, donde:

* posición: es la posición (medida en bytes) donde queremos desplazar el cursor o puntero de lectura. Puede tener **un valor negativo**.
* desde: es la referencia o desplazamiento desde la que calcular la posición anterior. 

Esta puede tener los siguientes valores:

| Valor | ejemplo | relativo a |
| --- | --- | --- |
| ios::beg | fi.seekg(pos, ios::beg) | desde el inicio del fichero |
| ios::cur | fi.seekg(pos, ios::cur) | desde la posición actual del cursor de lectura |
| ios::end | fi.seekg(pos, ios::end) | desde el final del fichero |

Por ejemplo, si deseamos acceder y leer el tercer elemento, hariamos:
```cpp
...
if (fbl.is_open()) {
	// nos posicionamos justo ante el tercer elemento:
	fbl.seekg( (3-1)*sizeof(ciudad), ios::beg); // contamos el tercer registro de tamaño ciudad desde el principio
	fbl.read( (char *)&ciudad, sizeof(ciudad) );
}
...
``` 
En este otro ejemplo, queremos acceder al **último** elemento del fichero:
```cpp
...
if (fbl.is_open()) {
	// nos posicionamos justo ante el tercer elemento:
	fbl.seekg( (-1)*sizeof(ciudad), ios::end); 
	fbl.read( (char *)&ciudad, sizeof(ciudad) );
}
...
``` 
A la función *seekg* le pasamos una posición negativa, relativa al final del fichero.

###Escritura de ficheros binarios

Para guardar datos en un fichero binario se usa la función ```write(registro, tamaño)```, y de forma parecida a la función de lectura, se le pasan dos parámetros:

* El registro que contiene los datos que van a enviarse al fichero.
* La cantidad de bytes que vamos a escribir. Con la función ```sizeof()``` calcularemos la cantidad de bytes a escribir. 

En el siguiente ejemplo de código escribimos en un fichero binario llamado "mifichero.dat" un registro de tipo *TIPOCIUDAD*.

```cpp
typedef struct { ... } TIPOCIUDAD;
...
TIPOCIUDAD ciudad;
ofstream fbe("mifichero.dat", ios::binary);

if (fbe.is_open())
{
	// introducimos datos en el registro ’ciudad’
	ciudad... = ...;
	// escribimos en el fichero
	fbe.write((const char *)&ciudad, sizeof(ciudad));
	...
}
fbe.close();
```

Si deseamos escribir en una posición concreta del fichero, podemos usar la función ```seekp(posición, desde)```, los argumentos son análogos a la función *seekg*:

* posición: es la ubicación (en bytes) donde queremos mover el cursor o puntero de escritura. Si la posición **no existe** en el fichero, éste se alargará para hacer posible la operación de escritura.
* desde: es la referencia o desplazamiento desde la que calcular la posición anterior. Puede tener los mismos valores que en la función *seekg*.

Por ejemplo, si deseamos escribir o modificar el quinto elemento de un fichero:
```cpp
...
if (fbe.is_open()) {
	// nos posicionamos para escribir en el quinto elemento:
	fbl.seekp( (5-1)*sizeof(ciudad), ios::beg); 
	fbl.write( (const char *)&ciudad, sizeof(ciudad) );
}
...
``` 
En este caso, si en el fichero hay 5 o más registros, sobreescribiremos el quinto con el contenido de la variable *ciudad*, pero si hubiera menos de cinco elementos entonces el fichero crecerá para permitir escribir el dato en la quinta posición.

Si tenemos un registro que contiene un campo de tipo cadena de carácteres y queremos almacenarlo en un fichero binario, tenemos que usar un vector de carácteres en lugar de un *string*. Al hacer la conversión puede ser que tengamos que recortar el *string* para adecuarlo al tamaño del vector. Por ejemplo:
```cpp
typedef struct {
	int codigo;
	char nombre[MAXLONG];
} TIPOCIUDAD;

string s="Alicante";
...
TIPOCIUDAD ciudad;
ciudad.codigo=3;
strncpy(ciudad.nombre, s.c_str(), MAXLONG-1); // convertimos el string a vector de carácteres
ciudad.nombre[MAXLONG-1]='\0'; // strncpy no pone el \0 si no esta en la cadena original.
...
fichero.write((const char *)&ciudad, sizeof(ciudad)); // escribimos el registro
...
```

####Funciones tellg() y tellp()

Existen dos funciones que nos permiten obtener la posición actual del puntero (el de lectura y de escritura). Devuelven la posición en **bytes**. 

* para el puntero de lectura se usa ```tellg()```.
* para el de escritura usamos ```tellp()```.

Por ejemplo, si tenemos un fichero abierto y queremos obtener la cantidad de registros que contiene:
```cpp
// Colocamos el puntero de lectura al final:
fichero.seekg(0, ios::end);

// Obtenemos el número de registros del fichero
cout << fichero.tellg()/sizeof(elRegistro) << endl;
```
###Gestión de errores

Al trabajar con ficheros (tanto de texto como binarios) hay dos operaciones que son especialmente subceptibles de producir un error en tiempo de ejecución, que en ocasiones puede ser fatal:

* en la apertura de un fichero: fichero inexistente, falta de permisos, etc.
* en las operaciones de lectura/escritura (sobretodo las de escritura): permisos, fichero bloqueado, etc.

Para comprobar si la operación de apertura ha sido exitosa podemos usar la función ```is_open()``` que nos dice si el fichero esta correctamente abierto, tal y como hicimos al principio del capítulo.

Tras una operación de lectura o escritura, es recomendable comprobar si ha habido algún error, para ello tenemos el método ```fail()``` (entre otras funciones). Un ejemplo de uso:

```cpp
fbl.read( (char *)&registro, sizeof(registro) );
if (fichero.fail() && !fichero.eof() ) {
   ... // error de lectura 
}
```

En este otro ejemplo más elaborado leemos un fichero hasta el final y en cada operación de lectura comprobamos si se ha producido un error, en ese caso, salimos del bucle, emitimos un mensaje de error y cerramos el fichero.

```cpp
...//abrimos el fichero
if (fi.is_open()) {
   bool error=false; // no hay error por ahora
   string s;

   while (getline(fi,s) && !error) { // acabamos al llegar al final o bien si hay error
      // leemos y comprobamos si hay error
      if (fi.fail() && !fi.eof()) {
         error=true;
      } else {
         // Hacer algo con s
      }
   }

   if (error) {
      cout << "Error de lectura" << endl;
   }
   fi.close();
}
```
<!-- TODO: transpa 27, exercicis!!! -->

###Ejercicios

####Ejercicio 3.6:

#####Dado un fichero binario "alumnos.dat" que tiene registros de alumnos, con la siguiente información por cada alumno:
| dni | vector de 10 caracteres |
| ------------- | ----------------------- |
| apellidos | vector de 40 caracteres |
| nombre | vector de 20 caracteres |
| turno | entero |

**Apartado 1:** Haz un programa que imprima por pantalla el DNI de todos los alumnos del turno 7.
**Apartado 2:** Haz un programa que intercambie los alumnos de los turnos 4 y 8 (los turnos van del 1 al 10).

####Ejercicio 3.7:

#####Dado el fichero “"lumnos.dat" del ejercicio anterior, haz un programa que pase a mayúsculas el nombre y los apellidos del quinto alumno del fichero, y lo vuelva a escribir en el fichero.

####Ejercicio 3.8:

#####Diseña un programa que construya el fichero "alumnos.dat" a partir de un fichero de texto "alu.txt" en el que cada dato (dni, nombre, etc) está en una línea distinta. Ten en cuenta que en el fichero de texto el dni, nombre y apellidos pueden ser más largos que los tamaños especificados para el fichero binario, en cuyo caso se deben recortar.

####Ejercicio 3.9:

#####Escribe un programa que se encarge de la asignación automática de alumnos en 10 turnos de prácticas. A cada alumno se le asignará el turno correspondiente al último número de su DNI (a los alumnos con DNI acabado en 0 se les asignará el turno 10). Los datos de los alumnos están en un fichero "alumnos.dat" con la misma estructura que en los ejercicios anteriores. La asignación de turnos debe hacerse leyendo el fichero una sola vez, y sin almacenarlo en memoria. En cada paso se leerá la información correspondiente a un alumno, se calculará el turno que le corresponde, y se guardará el registro en la misma posición.

