# Tema 1 - Introducción

En este tema veremos un repaso de los contenidos de Programación 1, añadiendo conceptos sobre  diseño de algoritmos y programas, metodología y sintaxis de C++.

## Diseño de algoritmos y programas

Para hacer un programa es necesario crear uno o varios ficheros de código fuente escritos en un lenguaje de programación. Una vez tenemos el código, mediante un compilador podemos transformarlo en un programa ejecutable que es capaz de interpretar el ordenador.

Por tanto, es necesario escribir código fuente, compilarlo y como resultado ya podemos ejecutar nuestro programa. Sin embargo, antes de comenzar a picar código tenemos que analizar los **requerimientos** y pensar en el **diseño** de nuestro algoritmo.

Las fases de desarrollo de un programa son las siguientes:

1. Estudio de los requerimientos del problema
1. Diseño del algoritmo (a ser posible en papel)
1. Escritura del código fuente en el ordenador
1. Compilación del programa y corrección de errores
1. Ejecución del programa
1. Prueba de todos los casos posibles (o casi)

El proceso de escribir, compilar, ejecutar y probar debería ser iterativo, haciendo pruebas de funciones o módulos por separado del programa.

### 1. Requerimientos

Para empezar tenemos que tener claros los **requerimientos**, es decir, **qué** debe hacer nuestro programa. En algunos casos es sencillo ,por ejemplo, si queremos hacer un programa para mostrar los _n_ primeros números primos. Sin embargo en otros casos es mucho más complicado, por ejemplo, si queremos hacer un programa de contabilidad para una empresa.

Antes de comenzar a diseñar el programa es necesario tener un listado de requerimientos con todas las opciones de nuestro programa. Este es un listado de ejemplo para hacer un juego:

* Tendremos varios niveles ordenados por dificultad
* En cada nivel tendremos cerdos, pájaros y otros objetos con los que interactúan
* El usuario podrá lanzar un pájaro con un tirachinas para acabar con los cerdos
* Un cerdo se destruye si un pájaro o un objeto impacta contra él.
* Podemos tener varios tipos de pájaros.
* El usuario puede tocar un pájaro en vuelo para hacer acciones especiales que dependen de su tipo.
* etc...

Como ves, la lista de requerimientos puede ser muy larga. En las prácticas de Programación 2 (P2) daremos el listado de requerimientos de forma clara para que no haya dudas sobre qué debe hacer el programa y cómo debe interactuar el usuario.

### 2. Diseño del algoritmo

Una vez tenemos claros los requerimientos, tenemos que pensar **cómo** vamos a implementar nuestro programa. Esto se hace en la fase de diseño, e incluye detectar los tipos datos necesarios, decidir las funciones para trabajar con ellos, y elaborar un flujo de programa.

La fase de diseño es muy importante, y conviene hacerla en papel. Para esto, lo recomendable es pensar primero en qué datos tenemos (en el ejemplo anterior, pájaros, cerdos, objetos, niveles, paisajes de fondo) y qué vamos a hacer con cada uno de ellos (lanzarPájaro, impactarConCerdo, tocarPájaro, etc.) para inferir las funciones.

En el caso de los programas con interfaz gráfico (por ejemplo, programas para móviles), podemos hacer el diseño en el mismo entorno de programación, colocando pantallas, botones, vistas, etc. En el caso de un programa sin gráficos (como será en esta asignatura), el diseño se limita a crear los tipos de datos necesarios y las funciones para trabajar con ellos.

En teoría, con un buen diseño ya podemos hacer el código, y no es necesario rediseñar el programa. En la realidad, para programas complicados es probable que, a pesar de que el diseño sea bueno, nos toque rehacer algunas partes. Por ejemplo, podemos hacer una app para móvil y darnos cuenta de que un botón no queda todo lo bien que pensábamos en un sitio determinado, y esto nos obligue a cambiar el diseño. Independientemente, lo que está claro es que cuanto mejor diseñemos un programa más sencillo será continuar con las siguientes fases, por lo que es recomendable dedicarle tiempo para pensarlo bien.

### 3. Escritura del código

Una vez tenemos claro cómo vamos a hacer el programa, procederemos a **escribir el código fuente**, usando un editor de textos, o alternativamente un entorno de desarrollo integrado (IDE, _Integrated Development Environment_).

Es muy importante no escribir mucho código (por ejemplo más de 50 líneas) de golpe. Lo que debe hacerse es escribir unas pocas líneas de código (o una función), y después compilar. Tras arreglar los errores de compilación y probar que el código hace lo esperado podemos seguir escribiendo.

Si hacemos mucho código sin comprobarlo, lo más normal cuando intentemos compilarlo es que nos salgan muchísimos errores de compilación y no seamos capaces de resolverlos adecuadamente.

### 4. Compilación y corrección de errores

Un compilador interpreta o convierte nuestro código fuente en un programa que el ordenador puede ejecutar. Lo normal tras escribir código es que tengamos algunos errores de compilación. Estos errores hay que corregirlos, reescribiendo el código y compilando hasta que no se produzcan más errores.

Hay dos tipos de errores: los errores de compilación, que impiden generar un ejecutable, y los _warnings_ de compilación, que nos dejan generar el ejecutable pero nos avisan de que puede que haya algo mal. Es conveniente arreglar todos los _warnings_, ya que muchas veces nos avisan de algo que no está bien y acaba produciendo errores durante la siguiente fase, la ejecución del programa.

### 5. Ejecución

Una vez tenemos una parte de nuestro programa compilado, debemos ejecutarlo para ver que el código que hemos añadido hace lo esperado. A veces no es así, y nos toca volver a la fase 3, reescribiendo el código, compilándolo y volviendo a ejecutar.

### 6. Prueba

Tras ejecutar el programa hemos visto que todo funciona correctamente pero, ¿es así en todos los casos?. ¿Qué ocurre si, por ejemplo, el usuario introduce por teclado un valor incorrecto? ¿Sigue funcionando todo bien?

En principio es complicado controlar todos los posibles casos que pueden darse durante la ejecución del programa, pero hay algunos trucos para esto.

Por ejemplo, tenemos la siguiente función:

```cpp
float division(int a, int b) {
	float resultado = a/b;
	return resultado;
}
```

Hay dos errores de ejecución en el código, ¿Puedes verlos?

Cuando la ejecutemos, veremos enseguida el primer error.

```cpp
cout << division(3,4) << endl;
```

El resultado será 0, ya que hemos hecho una división entera. Podemos arreglarlo:

```cpp
float resultado = (float)a/b;
```

En cuanto uno de los dos operandos sea de tipo _float_, el resultado será _float_. Sin embargo, el segundo error es más complicado. ¿cómo lo encontramos?

La respuesta es que **para cada función debemos considerar todos los posibles valores de sus parámetros** y ver si la salida será correcta con ellos. Por ejemplo, en esta función recibimos dos enteros. Su valor puede ser positivo, negativo, o cero.

¿Si algún parámetro es negativo, funcionaría? Sí.
¿Si algún parámetro es cero, funcionaría? No, ya que no pueden hacerse divisiones por cero y b podría tener ese valor. Por tanto, para que la función fuera correcta tendríamos que hacer cambios:

```cpp
float division(int a, int b) {
	float resultado=0;
	if (b!=0) {
		resultado = (float)a/b;
	}
	else {
		cout << "Error, no se permiten divisiones por cero!" << endl;
	}
	return resultado;
}
```

## C++

Tras ver cómo hacer un programa en líneas generales, vamos a centrarnos ahora en el lenguaje C++, que es el que usaremos en Programación 2. Existen muchas referencias sobre C++, pero si quieres complementar este libro de apuntes recomendamos <a href="http://www.cplusplus.com">cplusplus</a> y <a href="http://www.minidosis.org">minidosis</a>.


<!---
XXX: FALTA INTEGRAR ESTO AQUI

\begin{frame}[fragile]
\frametitle{Funciones (4/4)}
\begin{itemize}


\subsection{Estructura de un programa}

\begin{frame}[fragile]
  \frametitle{Estructura típica de un programa}
v

\begin{footnotesize}
\begin{tabular}{l}
\verb!#include <!ficheros de cabecera estandar\verb!>! \\
... \\ \pause
\verb!#include "!ficheros de cabecera propios\verb!"!  \\
... \\ \pause
\verb!using namespace std;! \emph{permite usar bool (y string)} \\
... \\ \pause
\verb!const! ...  \emph{si no estan en el fichero de cabecera} \\
... \\
typedef ...  \emph{tipos no definidos previamente} \\
... \\ \pause
declaración de variables globales  \alert{¡¡¡PROHIBIDO!!!} \\
... \\
funciones \\
... \\
\verb!int main()! \\
\verb!{! \\
... \\
\verb!}! \\
\end{tabular}
\end{footnotesize}
\end{frame}
-->


### Elementos básicos

En un código fuente podemos encontrarnos distintos elementos básicos:

* Identificadores: Nombres de variables, funciones, constantes, etc.
* Constantes: 123, 12.3, 'a', etc.
* Palabras reservadas: `if`, `while`, etc.
* Símbolos: {}, (), [], ;, etc.
* Operadores: ++, --, +, \*, /, etc.
* Tipos de datos: `int`, `char`, `float`, `double`, `bool`, `void`, etc.

Por ejemplo, en este código:

```cpp
int main() {
  for (int i=0; i<10; i++)
    cout << "Hola mundo" << endl;
}
```

* Identificadores: `i`
* Constantes: 0, 10, `"Hola mundo"`
* Palabras reservadas: `for`, `main`, `std::cout`, `std::endl`
* Símbolos: `(`, `)`, `;``
* Operadores: =, <, ++, <<
* Tipos de datos: `int`

Vamos a ver en detalle cada uno de estos elementos.

#### Identificadores

Los identificadores son los nombres que le damos a nuestras constantes, variables o funciones. Los elige el programador, por lo que debe seguir una serie de recomendaciones:

* Los identificadores deben ser **significativos**, es decir, su nombre debe indicar para qué se utiliza. Estos son ejemplos correctos:

```cpp
int numeroAlumnos = 0;
void visualizarAlumnos(...)
```

Por convenio, en C++ se suele seguir la notación _lowerCamelCase_, es decir, el nombre de las variables y funciones debe empezar por una letra minúscula, y si hay más de una palabra la primera letra debe ser mayúscula, como en el ejemplo anterior.

Estos son ejemplos de nombres incorrectos:

```cpp
const int kOCHO=8; // No se debe llamar a una constante con su valor
int p,q,r,a,b; // Una sóla letra no es significativa
int contador1,contador2; // mejor int i,j;
```

También por convenio, en C++ los contadores empiezan por la letra _i_. Por tanto, si tenemos varios contadores deberíamos llamarlos _i,j,k_, etc.


Evidentemente, existen palabras reservadas que no se pueden utilizar como nombres definidos por el usuario. Por ejemplo, en C++ no podemos llamar a una variable con el nombre `int`, `long`, `friend`, `for`, etc.

El nombre de constantes se suele poner en mayúsculas, para distinguirlas de las variables.

#### Constantes

Podemos tener constantes de varios tipos

| Tipo   | Ejemplos                   |
|--------|----------------------------|
| int    | 123, 007, -4               |
| float  | 123.0, -0.4, .3, 1.23e-12  |
| char   | 'a', '1', ';', '\''        |
| char[] | "hola","","doble: \""      |
| bool   | true, false                |


Cuando las constantes aparecen directamente en el código con su valor, como en el siguiente ejemplo, se dice que son **implícitas**:

```cpp
if (i<255) {
  cout << "Valor correcto" << endl;
}
```
Sin embargo, si las declaramos con un nombre que las identifique, se dice que son **explícitas**:

```cpp
const int kMAXVALUE = 255;
const char kMESSAGE[] = "Valor correcto";

if (i<kMAXVALUE) {
  cout << kMESSAGE << endl;
}
```

Podemos declrar constantes explícitas de cualquier tipo:

```cpp
const int MAXALUMNOS=600;
const double PI=3.141592;
const char DESPEDIDA[] = "ADIOS";
```

La pregunta es, ¿cuándo debemos hacerlo?. Y la respuesta es: **casi siempre**, ya que deberíamos declarar como constantes aquellos valores que podríamos querer cambiar en futuras versiones del programa. Por ejemplo, es posible que queramos cambiar el texto de un mensaje, por lo que debemos declararlo como constante.

La principal ventaja de usar constantes es que normalmente en el código tenemos que usar un mismo valor muchas veces (por ejemplo, un mensaje de error). Si lo declaramos como una constante explícita y queremos cambiar su valor en un futuro, sólo tendríamos que hacer este cambio en la declaración. Si no lo declaramos como explícito, nos tocaría buscar en el código todas las líneas en las que se aparece este mensaje para cambiar su valor, lo cual es más incómodo y propenso a errores por despistes.

#### Variables

A diferencia de las constantes, que sólo pueden tener un valor fijo asignado, las variables pueden usarse para almacenar valores que cambien durante la ejecución del programa.

Es muy recomendable que siempre que se declare una variable de (tipo simple) se le asigne un valor de inicialización, bien en la misma línea o bien en la siguiente. Por ejemplo:

```cpp
 int numeroProfesores=0; // Inicialización en la misma línea

 int numeroAlumnos;
 numeroAlumnos = 10; // Inicialización en la siguiente línea
```

Si no inicializa una variable de tipo simple, su valor será el que haya en memoria en ese momento, es decir, cualquiera (no podemos controlarlo).

> Los tipos simples más comunes en C/C++ son `int`, `char`, `float`, `double`, `unsigned` y `bool`.

Este código sería erróneo:

```cpp
int i;
cout << i << endl; // El valor será aleatorio
```
##### Ámbitos

Todas las variables (y constantes) que declaramos en nuestro código tienen un ámbito. El ámbito de una variable o constante comienza cuando se declara, y termina cuando acaba el bloque de llaves que la contiene. Por tanto, sólo podemos usar las variables o constantes durante su ámbito.

```cpp
if (i<10) {
	int j = 20;
}
j = 10; // No podemos usar la variable j aquí porque se ha destruído cuando ha terminado el if

```

Ejemplo para un bucle:

```cpp
for (int i=0; i<10; i++) {
	cout << "Hola mundo" << endl;
}
i = 2; // No podemos usar la variable i porque ya se ha destruído

```

Podemos declarar (aunque no es nada conveniente) dos variables que se llamen igual dentro de la misma función, siempre que tengan un ámbito distinto. Por ejemplo, esto compilaría:


```cpp
int ncajas=0;
// ya se puede usar ncajas
if (i<10) {
  // se puede usar
  int ncajas=100; // Dentro podemos declarar otra variable con el mismo nombre (no aconsejable)
  cout << ncajas << endl; // imprime 100
}
cout << ncajas << endl; // imprime 0, ya que se usa la primera variable
```

##### Variables globales

Las variables normalmente se declaran dentro de una función, pero cuando se declaran fuera se llaman variables globales. En general, se recomienda no utilizar variables globales (son peligrosas) y en Programación 2 están terminantemente prohibidas.

¿Por qué prohibimos estas variables? Veamos un ejemplo de código en la que se puede ver lo complicadas que son de manejar.

```cpp
#include <iostream>
using namespace std;
int contador=10;

void cuentaAtras() {
  while (contador > 0) {
    cout << contador << " ";
    contador--;
  }
  cout << endl;
}

int main() {
  cuentaAtras();   // Imprime 10, 9, 8, ... 0
  cuentaAtras();   // Aqui no se imprime nada
}
```

En este ejemplo, tenemos una variable global llamada `contador` que inicialmente vale 10. Podemos ver que si llamamos a la función `cuentaAtras`, la primera vez hará una cosa y la segunda otra distinta.

Este código es muy complicado de controlar, ya que **una misma función con los mismos parámetros se comporta de forma distinta** dependiendo de en qué punto del código hagamos la llamada.

#### Tipos de datos

Estos son los principales **tipos de datos simples** en C++:

| Tipo   | Descripción                   |
|--------------|----------------------------|
|`short` | Entero corto |
| `int`  | Entero, el doble de bytes que `short` |
| `char` | Caracter |
| `float`| Real |
|`double`| Real, el doble de bytes que `float` |
|`bool` | Booleano|

Además, para los números enteros tenemos su versión sin signo (`unsigned short` y `unsigned int`). Si un entero corto (`short`) se representa con 2 bytes, su rango va desde -32768 a 32767. En cambio, si es `unsigned short`, podremos representar desde 0 a 65535, por lo que si sabemos que no podemos tener valores negativos podremos aprovechar para representar números más altos sin necesitar más bytes.

> El número de bytes necesarios cada tipo de dato depende de la plataforma (por ejemplo, si es sistema operativo es de 32 bits o 64 bits).

##### Conversión

En nuestro código podemos convertir una variable de un tipo a otro. Esta conversión puede ser implícita (el compilador lo hace por nosotros directamente), o explícita (tenemos que indicarle que lo haga). A continuación podemos ver ejemplos de conversiones **implícitas**:

| Conversión   | Ejemplos                   |
|--------------|----------------------------|
| char -> int  | int le = 'A' + 2;          |
| int -> float | float pi = 1 + 2.141592;   |
| float -> double  | double pidouble = pi; |
| bool -> int | int c = true; // c == 1   |
| int -> bool   | bool c = 77212; // b == true     |

En estos ejemplos anteriores no hay problemas de conversión, por que la variable destino es más general que la variable original. Por ejemplo, en un `int` (normalmente 2 bytes) siempre podremos almacenar un valor `char` (1 byte).

Sin embargo, hay otros casos en los que haceiendo la conversión podemos perder información. En estas situaciones el compilador mostrará un `warning`, y tendremos que forzar la conversión en el código, haciendo lo que conocemos como `casting`.

> Es importante no ignorar los _warnings_.

Vamos a ver algunos ejemplos de conversiones **explícitas**:

| Conversión   | Ejemplos                   |
|--------------|----------------------------|
| int -> char  | char c = (char)('A' + 2); // c valdrá 'C'    |
| float -> int | int epi = (int)pi;  // epi valdrá 3  |
| double -> float | double d = (double)pi; |

En el primer caso, si convertimos un número entero a un caracter podríamos tener problemas (si es mayor de 255 no nos cabría en un byte). Por tanto, el compilador nos da un _warning_ (a veces incluso un error) para que lo tengamos en cuenta. Para indicar explícitamente que queremos convertir un tipo a otro incluso si puede haber problemas, tenemos que hacer un _casting_, convirtiendo el tipo de dato mediante paréntesis antes del nombre de la variable.


##### Declaración de tipos

En C++, como en la mayoría de lenguajes de programación, se pueden definir tipos nuevos. Para empezar, podemos asignar alias a ciertos tipos de datos ya existentes, usando `typedef`:

```cpp
typedef int entero;  
entero i,j; // i,j son int

typedef bool logico,booleano;  // logico y booleano son bool
logico b;  // b es bool
```

También podemos declarar un array como un tipo:

```cpp
typedef char cadena[MAXCADENA];  // cadena es un array de char
```

En Programación 2 no recomendamos redefinir los tipos ya existentes, básicamente porque un programador de C++ no está acostumbrado a ver tipos de datos que se llamen _logico_ o _cadena_ en el código. Pensad que véis un código lleno de tipos de datos que no conocéis, habría que ir a la definición de los tipos para saber qué significan en cada caso, y esto no es cómodo a la hora de compartir y reutilizar código.

Además de redefinir tipos existentes, C++ nos permite crear nuevos tipos de datos. En Programación 2, los únicos tipos de datos nuevos que veremos serán los registros (en el último tema veremos cómo hacer clases, pero en realidad no son tipos de datos). Podemos declarar un registro de dos formas:

<!--- CAMBIADO TRASPAS --->

```cpp
 struct Alumno {      // Primera forma: 'Alumno' es un tipo en C++ (recomendado)
   int dni;           
   double nota;
 };
```


```cpp
 typedef struct {     // Segunda forma: 'Alumno' es un tipo en C
   int dni;
   double nota;
 } Alumno;
```

En ambos casos declaramos un tipo de dato llamado `Alumno`. Después, podemos crear una variable de este tipo de la siguiente forma:

```cpp
Alumno alu;
```

La principal ventaja de usar registros es que nos permiten agrupar variables. Imaginemos que tenemos la siguiente función:

```cpp
int gestionarAlumno(int dni, double nota, char nombre[], int turno, int numFaltas);
```

Si usáramos un registro `Alumno` para agrupar todos estos datos, la llamada sería más sencilla:

```cpp
int gestionarAlumno(Alumno alu);
```

Además, podríamos crear arrays:

```cpp
Alumno alumnos[100];
```

Para acceder a los datos de un registro podemos usar un punto (`.`) tras su nombre. Por ejemplo:

```cpp
Alumno a,b;

a.dni = 123133;  // asignacion a un campo
b = a;           // asignacion de un registro
```

La asignación directa de registros (como aparece en la última línea del ejemplo anterior) es posible, pero no debemos hacerlo si dentro del registro tenemos un array o un puntero. El motivo es que hay que tener mucho cuidado al asignar directamente un registro o un puntero, por razones que veremos en el tema de memoria dinámica.

#### Expresiones

Las expresiones se usan cuando queremos hacer cálculos y evalúan una o varias operaciones devolviendo un resultado. En una expresión tenemos operadores y operandos, como puede verse en el siguiente ejemplo:

```cpp
int i = 10 + 12; // Expresión con operadores 10 y 12, y operando +.
```

Las expresiones pueden ser de varios tipos.

##### Expresiones aritméticas

En C++ los principales operadores aritméticos son la suma (`+`), la resta (`-`), la multiplicación (`*`), la división (`/`), y el el resto o módulo (`\%`).

Si aparece un operando de tipo `char` o `bool`, se convierte implícitamente a `int`. Por ejemplo:

```cpp
int i = 'a' + 3; // i == 100
char c = 'A' + 3; // c == D
```

Cuando hacemos una división entre dos números enteros (`int`), el resultado es un número entero. Por ejemplo:

```cpp
float f1 = 7 / 2; // f1 == 3
float f2 =  (float)7 / 2; // f2 == 3.5
```

En cuanto uno de los dos operandos es `float` (o `double`), el resultado de la división es un número real, como puede verse en el ejemplo anterior.

El operador resto (`%`) devuelve el resto de una división entre dos números enteros. Por ejemplo:

```cpp
int resto = 30 % 7; // resto == 2
```

En las expresiones aritméticas hay operadores que tienen preferencia sobre otros. En concreto, los operadores `*` y `/` se evalúan antes que los operadores `+`y `-`.

```cpp
int n = 2+3*2;  // Se evalúa 2+6, ya que la multiplicación se hace antes
```

En caso de que haya varias operaciones en una expresión es recomendable usar paréntesis para evitar problemas. Por ejemplo:

```cpp
int n = 2+(3*(7/2.5));
```

##### Operadores de incremento y decremento

Los operadores `++` y `--` se usan para incrementar o decrementar el valor de una variable de tipo entero. Podemos usarlos antes o después de la variable:

```cpp
++i;  // Preincremento
i++;  // Postincremento
```

La diferencia entre ponerlos antes o después es importante, ya que no significa lo mismo. Si van en una línea como en el ejemplo anterior, el resultado es idéntico. Sin embargo, cuando van acompañadas de otras instrucciones, cambia:

```cpp
int i = 3;
int k = ++i;  // k == 4, i == 4

i=3;
int j = i++;  // j == 3, i == 4
```

Como puedes ver, con preincremento (o predecremento) la suma se hace **antes** de calcular el resto de la expresión. En cambio, con postincremento (o postdecremento), se hace **al final**. En el primer caso, primero se hace el incremento (`++i`), y luego se asigna este valor a `k`. Sin embargo, en el segundo caso se hace la asignación (`j = i`) y al finalizar se incrementa el valor de `i `.

Aunque se pueden utilizar en cualquier punto de una expresión, lo recomendable es no mezclar estos operadores en la misma instrucción, ya que es muy complicado predecir el resultado. Por ejemplo, ¿cuál sería el valor de `j` en este caso?:

```cpp
i = 3;
j = i++ + --i;  // valor de j?
```

> Piensa en la respuesta, y luego confirma el resultado ejecutando este código.

##### Expresiones relacionales

Las expresiones relacionales evalúan una operación, devolviendo únicamente `true` o `false`. Los principales operadores relacionales en C++ son `==`, `!=`, `>=`, `>`, `<=`, `<`.

Si los operandos son de distinto tipo, se convierten implícitamente al tipo más general. Por ejemplo:

```cpp
bool resultado = 2 < 3.4; // Internamente la operación es 2.0 < 3.4
```

En C++, los operandos se agrupan de dos en dos por la izquierda. Por tanto, para saber si `a < b < c`, tenemos que indicar lo siguiente:

```cpp
if (a < b && b < c) {
	// Se cumple la relación
}
```

##### Expresiones lógicas

Las expresiones lógicas evalúan una expresión de tipo lógico, devolviendo `true` o `false`. Los principales operadores lógicos en C++ son `!`, `&&`, `||`. La negación (`!`) devuelve `true` si lo que viene ab continuación es `false`, o `false` si lo que viene a continuación es `true`. Por ejemplo:

```cpp
int i = 2;
bool salir = false;

if (i == 2 && !salir) {
	// La condición es verdadera, entra aquí
}

```

Hay una característica de C++ que es importante conocer : la **evaluación en cortocirtuito**. Cuando tenemos una condición `||`, si el operando izquierdo es `true` el operando derecho no se llega a evaluar, ya que `true || x` siempre será `true`. Por ejemplo:

```cpp
int i = 2;
bool salir = false;

if (i == 2 || !salir) {
	// La variable salir no llega a comprobarse, ya que se cumple que i==2
}
```

Asimismo, cuando tenemos una condición `&&`, si el operando izquierdo es `false`, el operando derecho no se llega a evaluar, ya que `false && x` siempre será `false`. Por ejemplo:

```cpp
char v[] = "Hola mundo";
char letraBuscada = 'k';

for (int i = 0; i < strlen(v) && v[i]!=letraBuscada; i++) {
 	cout << v[i];   // El bucle imprime "Hola mundo"
}
```

Este fragmento de código imprime una cadena hasta que se encuentra con la letra `'k'`. Fíjate en la condición `&&`. Cuando `i==strelen(v)`, esta expresión será `false` y por tanto no llega a comprobarse `v[i]!=letraBuscada`. Si no existiera la evaluación en cortocircuito y esta segunda condición se comprobara, se produciría un fallo de segmentación (_segmentation fault_) al intentar mirar en una posición del array mayor de su tamaño. Si implementáramos la condición al revés, `v[i]!=letraBuscada && i<strlen(v)`, nos saldría este error. Por tanto, **el orden de los operandos en una expresión lógica es importante**.

#### Entrada / salida

En C++ disponemos de los _streams_ de entrada y salida _cin_ y _cout_, respectivamente. Podemos mostrar una variable por pantalla de la siguiente forma:

```cpp
int i = 3;
cout << i << endl;
```

Como ves, las "flechas" van en el sentido de la operación (se envían los datos a `cout`). Por defecto, todo lo que se envía a la salida estándar (`cout`) se imprime por pantalla. Sin embargo, podemos cambiar este comportamiento. Escribe este programa, llámalo `prueba.cc` y compílalo:


```cpp
#include <iostream>

using namespace std;

int main() {
	cout << "Hola mundo" << endl;
}
```

Y ejecútalo desde el terminal:

```bash
$ ./prueba
```

Verás que se muestra por pantalla el mensaje. Ahora vamos a ejecutarlo de otra forma:

```bash
$ ./prueba > salida.txt
```

En lugar de mostrarse por pantalla, la salida se ha guardado en el fichero `salida.txt`, que puedes abrir con cualquier editor de texto. A esto se le llama **redirección** de la salida estándar.

Igual que podemos mostrar algo, también podemos leer el valor de una variable desde teclado:

```cpp
#include <iostream>

using namespace std;

int main() {
	int n;
	cin >> n; // Leemos un valor entero por teclado
	cout << "He leido" << n << endl;
}
```

En este caso, leemos de la **entrada estándar** con `cin`. Los datos a leer dependerán del tipo de la variable. En este ejemplo, es un número entero, pero podría ser un caracter (sólo se leería una letra), o una cadena, por ejemplo. El operador de entrada lee desde teclado ignorando blancos y tabuladores hasta leer el tipo de dato de la variable que se le indica, y deja el puntero de lectura justo después.

Al igual que con `cout`, podemos leer varias variables seguidas:


```cpp
#include <iostream>

using namespace std;

int main() {
	int n;
	char c;
	cin >> n >> c; // Leemos un valor entero y un caracter por teclado
	cout << "He leido" << n << " y " << c << endl;
}
```

Cuando ejecutemos el programa quedará a la espera de que introduzcamos un número entero y a continuación un caracter.

> Cada vez que leamos algo con `cin >>` , es conveniente poner a continuación `cin.get();`, `cin.ignore();` o llamar a una función propia  `limpiarBuffer()`  para que no se nos quede en el _buffer_ el caracter de salto de línea, ya que puede dar problemas si se lee después una cadena como veremos en el tema de `strings`.

Vamos a ejecutar el programa anterior redireccionando la entrada estándar. Con cualquier editor de texto creamos un nuevo fichero llamado `entrada.txt`, y escribimos lo siguiente:

```bash
124 k
```

No te olvides de poner un salto de línea al final del fichero. A acontinuación, ejecutamos:

```bash
$ ./prueba < entrada.txt
```

Verás como el programa ya no pide los datos de la entrada, sino que los lee desde el fichero. Podemos también usar la redirección de entrada y salida a la vez:

```bash
$ ./prueba < entrada.txt > salida.txt
```

El programa leerá la entrada estándar desde el fichero `entrada.txt` y escribirá el resultado en `salida.txt`.

Existe un tercer _stream_, la salida de error (`cerr`). Cambia el programa anterior, reemplazando `cout` por `cerr`. Como verás, si ejecutamos el programa como hemos hecho la última vez, se mostrará la información por la pantalla y `entrada.txt` estará vacío. Esto ocurre porque lo que sale por la salida de error se redirecciona de forma distinta a lo que se muestra por la salida estándar. Para redireccionar el error, tenemos que poner:


```bash
$ ./prueba < entrada.txt 2> error.txt
```


La salida de error se suele usar para mostrar mensajes de error de nuestros programas. Por ejemplo, añade cualquier cosa al código anterior para que no compile. Ejecuta el compilador:

```bash
g++ -o prueba prueba.cc 2> errores.txt
```

Verás que los errores de compilación se han guardado en `errores.txt`, ya que el programa `g++` muestra los mensajes de error mediante `cerr`.


#### Control de flujo

El control de flujo de código nos permite añadir condiciones al código, de forma que nuestro programa ejecute sólo ciertas instrucciones en función de una condición.

##### `if`

La instrucción `if` ejecuta lo que hay a continuación siempre y cuando la condición sea `true`. Podemos también añadir un `else` para que se ejecuten instrucciones alternativas cuando no se cumple la condición:

```cpp
if (condicion) {
	// Instrucciones
}
else {
	// Otras instrucciones
}
```
##### `while`

La instrucción de control de flujo `while` crea un bucle que finaliza cuando se deja de cumplir una condición.

```cpp
while (condicion) {
	// Instrucciones
}
```

Es desaconsejable usar `||` en la condición de un `while`, ya que es muy complicado de controlar y poco intuitivo. Por ejemplo, piensa cuándo pararía el bucle en el siguiente código:

```cpp
bool encontrado=false;
int i=0;
while (i<10 || !encontrado) {
	cout << i << endl;
   i++;
}
```

Respuesta: Nunca, ya que se debe cumplir que `i<10` **y también** que `encontrado==true`.

> Nota: Para parar un programa que se ha quedado en un bucle infinito, pulsa `Ctrl + C`.

##### `for`

Las condiciones `for` son bucles con una inicialización y una instrucción que se ejecuta tras cada iteración:

```cpp
for (inicializacion; condicion; finalizacion) {
	// Instrucciones
}
```

Los `for` suelen ser necesariso cuando recorremos una serie de elementos, por ejemplo para recorrer un array:

```cpp
bool salir = false;
int array[] = {1,3,5,2,5,6,1,2};

for (int i=0; i<8 && !salir; i++) {
	cout << i << endl;
	if (array[i] == 6)
		salir=true;
}
```

En realidad un `for` es equivalente a una condición `while` pero usando menos código:

```cpp
inicializacion;
while (condicion) {
   // Instrucciones
   finalizacion;
}
```


##### `do-while`

Las instrucciones de control `do-while` son similares a las instrucciones `while`, con la diferencia de que la primera vez **siempre** ejecutan su contenido.

```cpp
do {
  // Instrucciones
} while (condicion);
```

Se usan, por ejemplo, cuando queremos mostrar un menú por pantalla.

##### `switch`

Las instrucciones `switch` se usan cuando queremos hacer una acción para una variable que puede tener varios valores distintos. Por ejemplo:

```cpp
switch (variable) {
  case valor1:
           // Instrucciones 1
           break;
  case valor2:
           // Instrucciones 2
           break;
  default:
           // Instrucciones 3
           break;
}
```

En C++ la variable (o constante) del `switch` debe ser de tipo entero o char (que se convierte implícitamente a entero). Por ejemplo:

```cpp
char c;
cin >> c;
switch(c) {
	case 'a':
		cout << "Seleccionado a" << endl;
		break;
	case 'b':
	case 'c':
		cout << "Seleccionado b o c" << endl;
		break;
	default:
		cout << "Valor desconocido" << endl;
		break;
}
```


Si la variable es de otro tipo como `float` o `double`, el programa no compilará.

> Como ves, tenemos que usar `break` en los `switch`. También se puede usar `break` para salir de un bucle `while`, `do` o `for`.


#### _Arrays_ y matrices

Un _array_ es un contenedor que permite almacenar una secuencia de variables o constantes. En C/C++, cuando declaramos un _array_ lo hacemos con un **tamaño fijo** que no puede cambiar en tiempo de ejecución.

Podemos usar una constante para indicar su tamaño:

```cpp
int arrayAlumnos[10];
char fila[kMAXTABLERO];
```

También podemos crear un _array_ inicializándolo con una serie de elementos. En este caso no hay que especificar el tamaño:

```cpp
int numeros[] = {1,3,5,2,5,6,1,2};
```

En C++ podemos crar un array con un tamaño asignado por una variable:

```cpp
int n;
cin >> n;
int v[n]; // El tamaño del array se conoce en tiempo de ejecución. NO RECOMENDABLE!
```

Pero el estándar del lenguaje no recomienda hacerlo así. Si el tamaño del _array_ no se conoce en tiempo de compilación, entonces es mejor usar vectores, como veremos a continuación.

Para acceder a los valores de un _array_ podemos usar corchetes: `[]`

```cpp
int numeros[] = {1,3,5,2,5,6,1,2};
cout << numeros[2] << endl; // Imprime 5
```

El índice de un _array_ comienza en la posición 0. Asimismo, nunca podemos sobrepasar el número de elementos de un _array_, ya que lo más probable es que se produzca un fallo de segmentación:

```cpp
int numeros[] = {1,3,5,2,5,6,1,2};

numeros[20] = 12; // Error, el array tiene menos de 20 elementos
numeros[8] = 3; // Error, el array tiene 8 elementos cuyas posiciones van de 0 a 7

for (int i=0; i<8; i++) {
	numeros[i] = 4; // Correcto
}
```

Es importante comprobar siempre los límites de un _array_ para no acceder fuera de ellos.

Una matriz es un _array_ de más de una dimensión:

```cpp
int matrix[10][10];

matrix[0][2]=31; // Asignación de un valor
```

#### Cadenas de caracteres en C

Las cadenas de caracteres en C son simplemente _arrays_ que contienen caracteres. Por ejemplo:

```cpp
const char cadena[]="Hola";
```

Para representar una cadena de caracteres usamos comillas dobles (`"`), mientras que para representar un sólo caracter usamos comillas simples (`'`).


La peculiaridad es que a estos _arrays_ C les añade el carácter nulo al final: `'\0'`.

| H | o | l | a |\0 |
|:---|:---|:---|:---|:---|
| <sub>0</sub> | <sub>1</sub> | <sub>2</sub> | <sub>3</sub> | <sub>4</sub> |


Es necesario que todas las cadenas de C acaben con el caracter nulo para que se ejecuten correctamente las funciones que trabajan sobre ellas, como `strlen`, `strcpy`, etc.

Al igual que cualquier otro _array_, si lo declaramos sin inicializarlo debemos especificar su tamaño:

```cpp
const int tCADENA = 10;
char cadena[tCADENA];
```

#### Vectores

Como hemos visto anteriormente, una vez hemos declarado un _array_ no podemos cambiar su tamaño. Si lo que queremos es usar _arrays_ de tamaño variable, entonces tenemos que usar los **vectores** de C++.

Un <a href=http://www.cplusplus.com/reference/vector/vector/>vector</a> es un _array_ con acceso eficiente a elementos y con la habilidad para cambiar automáticamente de tamaño cuando se añaden o se eliminan elementos. Estos vectores inicialmente pertenecían a la biblioteca _STL (Standard Template Library)_, que implementa una serie de contenedores dinámicos, algoritmos e iteradores.

Veamos un ejemplo de cómo usarlos:

```cpp
vector<int> v; // Declara un vector de enteros
vector<int> v2(3); // Declara un vector de 3 enteros
v.resize(4); // Cambia dinámicamente su tamanyo

v.push_back(12); // Añade un valor al final del vector

// Acceso a elementos
for (unsigned int i=0; i<v.size(); i++) {
      v[i]=23; // Asignación
}
```

Como puedes ver en este fragmento de código, podemos declarar un vector sólo indicando su tipo y sin especificar su tamaño:

```cpp
vector<int> v;
```

También podemos indicar un tamaño inicial:

```cpp
vector<int> v2(3);
```

Añadimos un elemento al final de un vector con `push_back()`, y podemos saber el tamaño efectivo de un vector con `size()`.

Estas son sólo algunas de las funciones de la clase vector, cuya referencia puedes encontrar <a href=http://www.cplusplus.com/reference/vector/vector/>aquí</a>. Además de estas, los vectores tienen funciones para ordenar sus elementos siguiendo el criterio que elijamos, para borrar un elemento redimensionando su tamaño, etc.

En Programación 2 trabajaremos bastante con vectores.

#### Tipos enumerados

Los tipos enumerados se utilizan cuando tenemos un rango determinado de posibles valores para una variable (o constante). A estos posibles valores se les llama enumeradores, y las variables de los tipos enumerados pueden tomar cualquier valor de estos enumeradores, como puede verse en el siguiente ejemplo:

```cpp
enum colors_e {black, blue, green, red}; // Definición del tipo enumerado

colors_e mycolor; // Declaración de una variable

mycolor = blue; // Asignación de un enumerador

if (mycolor == green) { // Comparación con un enumerador
	mycolor = red;
}
if (mycolor == 0) {  // Comparación con un entero (internamente, black)
	cout << "Black" << endl;
}
```

Internamente, los enumeradores se convierten implícitamente a números enteros y viceversa, como puede verse en el ejemplo anterior.


#### Funciones

Una función es un conjunto de líneas de código que realizan una tarea y, opcionalmente, puede devolver un valor. También puede recibir (opcionalmente) una serie de parámetros, por valor o por referencia.



```cpp
// Función que recibe dos parámetros por valor y uno por referencia, y devuelve un número entero

int funcion(int a, int b, int &c) {
	int ret = 0; // Declaramos una variable del tipo de retorno

	// Instrucción 1
	// Instrucción 2
	// ...

	return ret; // Devolvemos el resultado
}
```

Como puedes ver en el ejemplo anterior, cuando una función devuelve un valor, normalmente se declara la variable al principio y se hace un único return al final.

Una función no debería tener mucho código. Lo normal es que el código de la función "quepa" en la pantalla cuando se visualice con un editor (unas 50 líneas máximo). Si la función es más larga, lo recomendable es dividirla en varias funciones más cortas.

A veces no tenemos claro cuándo hay que crear una nueva función. Hay varios casos en los que hace falta, pero existe una regla sencilla: **si tienes que hacer copy-paste, entonces necesitas una función**. Cuando copiamos un trozo de código para pegarlo en otro lugar, es porque lo estamos usando dos veces. Cuando esto ocurre, es mejor crear una función para este trozo, de forma que el código quede más compacto, y además sea más fácil de mantener (si queremos hacer un cambio en el código de la función, sólo lo tendremos que hacer en un sitio, en lugar de en dos si no la hemos creado).  

> Importante: Es muy recomendable compilar y probar las funciones por separado, no esperar a tener todo
el programa para empezar a compilar y probar. Cada vez que tengamos una nueva función, hay que comprobar que funcione correctamente con cualquier número de parámetros, y cuando esté probada pasar a escribir la siguiente.

El compilador de C/C++ lee el código desde el principio al final, por lo que si hacemos una llamada a una función que está declarada más adelante obtendremos un error de compilación. Para evitarlo, se puede mover la función antes de su llamada, o bien indicar su declaración (también llamada cabecera o prototipo) antes. Este es un ejemplo del segundo tipo (declaración de cabecera):

```cpp
// Prototipo / cabecera / declaración de la función
int funcion(bool,char,double []);

char otraFuncion() {
   double vr[MAXNOTAS];
   a = funcion(true,'a',vr);  // Llamada a la función
}

// Cuerpo / implementación de la funcion
int funcion(bool comer,char opcion,double vectorNotas[]) {
  // Instrucciones
}
```

En Programación 2, tal como hace la mayoría de desarrolladores en C++, cuando tenemos todo el código en un único fichero recomendamos mover el código de la función en lugar de indicar su cabecera:

```cpp
// Cuerpo / implementación de la funcion
int funcion(bool comer,char opcion,double vectorNotas[]) {
  // Instrucciones
}

char otraFuncion() {
   double vr[MAXNOTAS];
   a = funcion(true,'a',vr);  // Llamada a la función
}
```

Esta segunda forma agiliza la escritura del código, ya que cuando vayamos a cambiar los parámetros de una función sólo tendremos que hacerlo en el cuerpo, en lugar de en dos sitios (cuerpo y declaración).

En C++, las funciones aceptan parámetros pasados por valor o por referencia (con `&`). Internamente, cuando una función recibe un parámetro por valor, ésta se **copia** en una variable local que se destruye cuando termina la función. De este modo, los cambios que se hagan sobre los valores de esta variable no tendrán efecto a la salida de la función. Cuando un parámetro se pasa por **referencia** no se hace una copia del mismo, por lo que los cambios realizados durante la función sí que afectarán a esta variable.

El problema surge cuando queremos pasar por valor una variable muy grande (por ejemplo, una cadena de 1 millón de caracteres). Si el compilador hace una copia, el rendimiento del programa se verá seriamente afectado (además de que necesitaremos mucha más memoria). Para evitar esto, en C++ se puede pasar un parámetro por referencia con `const`, como en el siguiente ejemplo:

```cpp
void funcion(const string &s)  {
	// El compilador no hace copia de s, pero si intentamos modificar esta variable nos da un error
}
```

Lo que le decimos al compilador con esta declaración es: "No hagas una copia de la variable, pero prometo no modificarla dentro de la función". De hecho, si la intentamos modificar nos saldrá un error de compilación.

> Por motivos pedagógicos en Programacón 2 no se deben pasar parámetros por referencia cuando no es necesario hacerlo, excepto si es con `const`.

En C/C++ hay un caso particular de paso por valor o referencia. Se trata de los _arrays_ y matrices, ya que estos siempre se pasan por referencia. La explicación la veremos en el tema de memoria dinámica, pero básicamente esto sucede porque internamente son punteros y en realidad lo que pasamos por valor o referencia es el puntero en sí.

```cpp
int sumaVM(int v[],int m[][MAXCOL]) {  // el tamano de la primera dimension no se indica
  // Instrucciones
}

sumaVM(vector,matriz);  // llamada, sin corchetes  
```

Como ves, tampoco hay que indicar el tamaño de la primera dimensión, pero si hay más dimensiones C++ obliga a ponerlas.

Existe también una función especial en C/C++, llamada `main`. La función `main` es la primera que se invoca cuando comienza un programa. Puede recibir parámetros (veremos cómo hacerlo en el tema de paso de parámetros), y devuelve un valor (aunque se puede dejar sin indicar, normalmente devuelve 0). Por ejemplo:

```cpp
int main() {
  // Instrucciones

  return 0; // Opcional
}
```

¿Cómo debe ser una función `main`?. Lo ideal es que viendo la función se sepa cuál va a ser el flujo del programa. Un ejemplo de una función `main` adecuada:

```cpp
int main() {
	int n;
	leer(n);

	if (n<0)
		cout << "Error, no puede ser negativo";
	else procesar(n);
}
```

Como puedes ver, hay poco código y se entiende qué hace más o menos el programa. A veces no es tan sencillo cuando el programa es muy largo, pero la idea es esta. Lo que **nunca** debe hacerse es **hacer todo el código en el main**, ni tampoco dejar un `main` con una sola llamada a una función que lo hace todo:

```cpp
int main() {
	principal();  // Incorrecto
}
```

### Argumentos del programa

La función `main` también puede recibir parámetros. Cuando lo hacemos, estamos pasando argumentos a nuestro programa, y esto sirve para la ejecución en lotes (de forma no interactiva). Un ejemplo de paso de argumentos es el que usa el programa `ls`:

```bash
ls -l -a
```

En este caso, pasamos dos parámetros: `-l` y `-a`. Con esta información, el programa `ls` mostrará los datos de una forma determinada. Imagínate que `ls` no admitiera parámetros. Cada vez que lo ejecutáramos, nos preguntaría si queremos mostrar los archivos ocultos, si queremos mostrar la información por líneas, etc, lo cual sería muy tedioso para el usuario.

Vamos a ver un ejemplo de un programa que admite parámetros desde la función `main`:

```cpp
int main(int argc, char *argv[]) {
	// En este punto, argc contiene el numero de parametros, y argv su valor.
	for (unsigned i=0; i<argc; i++) {
		cout << "Argv[" << i << "]=" << argv[i] << endl;
	}
}
```

Si ejecutamos el programa de esta forma:

```bash
./programa uno dos tres
```

Se imprimirá lo siguiente:

```bash
Argv[0]=./programa
Argv[1]=uno
Argv[2]=dos
Argv[3]=tres
```

Por tanto, la variable `argc` contiene el número de parámetros que ha introducido el usuario, y `argv` es un array de cadenas de caracteres y en cada posición contiene un valor introducido por el usuario.

En principio parece sencillo, pero se complica cuando tenemos muchos parámetros y pueden ir en cualquier orden. Por ejemplo, `g++` admite muchísimos parámetros y el orden puede ser importante:

```bash
g++ -Wall -o programa programa.cc -g
```

Gestionar toda esta variabilidad es algo complicado. Por ejemplo, si queremos un programa que acepte tres parámetros ("uno","dos" y "tres"), podemos usar un bucle como este:

```cpp
int main(int argc, char *argv[]) {

	  if (argc>4) {
			cout << "Sintaxis: " << argv[0] << " [uno | dos | tres]" << endl;
			return(-1);
		}

		for (unsigned i=1; i<argc; i++) {
			string arg = argv[i];

			if (arg=="uno")  {
				// Hacer algo con argumento uno
			}
			else if (arg=="dos") {
				// Hacer algo con argumento dos
			}
			else if (arg=="tres") {
				// Hacer algo con argumento tres
			}
			else {
				cout << "Sintaxis: " << argv[0] << " [uno | dos | tres]" << endl;
				return(-1);
			}
		}
}
```

A veces se puede complicar tanto que conviene usar una función aparte para gestionar los parámetros.

#### Ejercicio (argumentos)

Vamos a hacer un programa que imprima por pantalla los _n_ primeros números primos. Por defecto (si no se indican parámetros), el programa debe imprimir los primeros 10 números primos separados por espacios. Ejemplo:

```bash
./primos
1 2 3 5 7 11 13 17 19 23
```

El usuario debe poder también indicar las opciones `-L` y/o `-N n`.

La opción `-L` es para mostrar cada número en un linea distinta, y la opción `-N` seguida de un número es para mostrar los primeros `n` primos. Por ejemplo:

```bash
./primos -N 3
1 2 3
./primos -L -N 2
1
2
```

Si los parámetros indicados por el usuario no son correctos debe mostrarse un mensaje de error de sintaxis y finalizar el programa. Lo complicado es esta parte, es decir, comprobar que los parámetros son adecuados.

### Estructura tipica de un programa en C++

Para repasar, veamos la estructura típica de un programa en C++:

```cpp
#include <ficheros de cabecera estandar>
...
#include "ficheros de cabecera propios"
...
using namespace std; // permite usar bool (y string)
...
const ... // Declaración de constantes
...
typedef ... // Declaración de tipos de datos propios
...
// declaración de variables globales ¡¡¡PROHIBIDO!!!
...
// funciones
...
int main() {
...
}
```

### Compilación

El compilador de C++ que usaremos en la asignatura es GNU GCC, que viene por defecto en Linux. Podemos llamar al compilador desde un terminal poniendo `g++`. El compilador de C++ admite muchos parámetros, vamos a ver los más importantes:

* `-Wall` : Muestra todos los warnings, no sólo los más importantes (**recomendado en P2**).
* `-g` : Compila en un modo que facilita encontrar los errores mediante un depurador (**recomendado en P2**).
* `-o`: Sirve para indicar el nombre del ejecutable, que es el parámetro que viene a continuación de esta opción.
* `--version`: Muestra la versión actual del compilador.
* `-std=c++0x`: Usa el nuevo estándar de C++ que permite programación no tipada con `auto`, funciones `lambda`, bucles `for_each`, y funciones de concurrencia, entre otras. No lo usaremos en P2.


La forma recomendada en Programación 2 para compilar un programa es la siguiente:

```bash
g++ -Wall -g programa.cc -o programa
```

Es muy importante que justo a continuación de `-o` vaya el nombre del ejecutable, **no** el del código fuente. Si ponemos `-o programa.cc`, se nos borrará todo nuestro código fuente (es habitual perderlo en los primeros cursos, así que ten cuidado).


### Depuración

En algunas ocasiones el programa compila pero falla durante la ejecución. A veces es muy complicado encontrar qué línea del código ha provocado el error de ejecución. Afortunadamente, existen los **depuradores** (en inglés _debuggers_), unas herramientas que nos facilitan mucho esta tarea.


> Se llaman _debuggers_ porque encuentran _bugs_, que es como se conocen popularmente los errores de código. Este término viene porque en 1946, una polilla se introdujo accidentalmente en un circuito provocando errores en su programa. Cuando encontraron el problema, la pegaron con celo en el correspondiente informe de errores y escribieron: _First actual case of bug being found_.

![Bug](bug.jpg "Bug")

A continuación se indican algunos depuradores muy usados:

* <a href="http://valgrind.org">Valgrind</a>. Es el depurador que usaremos en los correctores de las prácticas. Detecta errores de memoria (acceso a componentes fuera de un vector, variables usadas sin inicializar, punteros que no apuntan a una zona reservada de memoria, etc.).
* <a href="https://www.gnu.org/software/gdb/">GDB</a>. Este depurador inicia nuestro programa, lo para cuando lo pedimos y mira el contenido de las variables. Si nuestro ejecutable da un fallo de segmentación, nos dice la línea de código dónde está el problema.
* Más ejemplos en Linux: <a href="https://www.gnu.org/software/ddd/">DDD</a>, <a href="https://wiki.gnome.org/Apps/Nemiver">Nevimer</a>, <a href="http://elinux.org/Electric_Fence">Electric Fence</a>, <a href="http://duma.sourceforge.net">DUMA</a>, etc.

Casi todos estos depuradores se pueden instalar desde el instalador de paquetes de Linux (es mejor así que hacerlo desde su web).


### Recordatorio

Por motivos pedagógicos, en Programacón 2 está **terminantemente prohibido usar   variables globales**.


### Referencias

Para aprender más sobre C++ recomendamos consultar las siguientes referencias:

<a href="http://www.cplusplus.com">http://www.cplusplus.com</a>

<a href="http://www.minidosis.org">http://www.minidosis.org</a>
