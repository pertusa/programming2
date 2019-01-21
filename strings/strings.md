# Tema 2 - Cadenas de caracteres

La mayoría de programas que desarrollamos tienen que trabajar en algún momento con datos textuales. Por ejemplo, mostrar un mensaje por pantalla a un usuario o leer el nombre de un cliente introducido por teclado implica manipular cadenas de texto.

El lenguaje C++ nos proporciona dos formas de representar las cadenas de texto:

* Cadenas de caracteres al estilo C

* La clase `string` de  C++

En este tema veremos en primer lugar las cadenas en estilo C, para luego centrarnos en cómo trabajar con la clase `string` de C++.

La clase `string` facilita mucho el trabajo con cadenas, por lo que será nuestra opción preferida en la asignatura de Programación 2. La única circunstancia en la que tendremos que utilizar necesariamente cadenas al estilo C será cuando trabajemos con ficheros binarios. Veremos el por qué en el Tema 3.

## Cadenas de caracteres en C

### Declaración e inicialización

Esta forma de manipular texto es originaria del lenguaje C pero puede utilizarse también en C++.

Las cadenas en C se representan con un array de caracteres \(es decir, de tipo `char`\) terminado en el carácter nulo \(`'\0'`\). Como todo array, tienen un tamaño fijo que se establece en el momento de declararlo y ya no puede variar a lo largo de la ejecución del programa.

Ejemplo:

```
char cad[10];
```

Este código define una cadena de caracteres, llamada `cad`, de 10 elementos. Como hay que reservar un espacio para introducir el carácter nulo de final de cadena, la variable `cad` del ejemplo anterior podría almacenar como máximo 9 caracteres.

El lenguaje C/C++ nos permite inicializar las cadenas de caracteres con texto dentro de comillas dobles \(`""`\).

Ejemplo:

```cpp
char cad1[5] = "hola";
char cad2[] = "hola";
```

Las dos declaraciones anteriores son equivalentes. En el primer caso, a `cad1` le asignamos el tamaño `5` para poder almacenar los cuatro caracteres de `"hola"` junto con el carácter nulo. En el segundo caso con `cad2`, se puede ver que no es necesario indicar el tamaño de la cadena cuando hacemos una declaración con inicialización: el compilador asignará al array el tamaño exacto que necesita para almacenar la cadena con la que se inicializa \(en este caso `5`\).

Otro detalle importante que muestra este ejemplo es que no hace falta poner explícitamente el carácter nulo al final de la cadena constante definida con comillas dobles. El compilador automáticamente coloca el `'\0'` al final de la cadena cuando se inicializa el array.

A continuación se muestra cómo sería la representación en memoria en C/C++ de cualquiera de las dos cadenas del ejemplo anterior:

!\[Falta imagen\]\(./images/fig\_1.jpg "Representación en memoria de la cadena ''Hola''"\)

La numeración superior \(de 0 a 4\) indica el índice que ocuparía cada uno de los caracteres en el array. La numeración inferior \(de 1001 a 1005\) representa la posición de memoria que ocuparía cada carácter. Se trata de direcciones de memoria ficticias simplificadas para este ejemplo \(en realidad una dirección válida sería algo como esto: `0x7ffef832d670`\). Lo importante es ver que cada caracter de una cadena se almacena en posiciones consecutivas de la memoria, cosa que sucede siempre con los elementos de un array, sean del tipo que sean.

Otra forma de inicializar una cadena es hacerlo carácter a carácter.

Ejemplo:

```cpp
char cad1[5] = {'h','o','l','a','\0'};
char cad2[] = {'h','o','l','a','\0'};
```

Esta inicialización sería equivalente a la del ejemplo anterior. En este caso sí que sería necesario introducir explícitamente el carácter `'\0'` al final de la cadena. Si declaráramos lo siguiente:

```cpp
char cad[]={'h','o','l','a'};
```

estaríamos definiendo un array de cuatro caracteres con los elementos `'h'`, `'o'`, `'l'` y `'a'`, pero no se consideraría una cadena de caracteres "bien formada", ya que no acaba en el carácter nulo y por lo tanto no podríamos aplicar correctamente sobre ella las funciones que nos facilita el lenguaje C y C++ para el manejo de cadenas \(y que veremos más adelante en este mismo tema\).

Al igual que para arrays de otros tipos, no es necesario usar todo el espacio reservado para la cadena cuando se declara.

Ejemplo:

```cpp
char cad[100] = "Hola";
```

Aquí estaríamos reservando `100` posiciones de memoria, aunque solo estaríamos ocupando 5 en este momento \(4 letras más el carácter nulo\). Las otras 95 posiciones quedarían reservadas pero sin inicializar.

La cadena vacía se representa con las comillas dobles, una a continuación de la otra y sin espacio en medio:

```cpp
char cadenaVacia[] = "";
```

El array `cadenaVacia` tendría tamaño 1, ya que solo almacenaría el carácter `'\0'`.

### Entrada y salida

#### Salida por pantalla con `cout`

Para mostrar cadenas por pantalla se puede utilizar `cout`, como con cualquier otro tipo simple \(`int`, `float`, etc.\).

Ejemplo:

```cpp
char cad[] = "Hola a todo el mundo";
cout << cad;
```

Este código mostraría por pantalla `Hola a todo el mundo`.

#### Lectura de teclado con `cin` y `>>`

La lectura de información por teclado se realiza con `cin` y el operador `>>`, de manera similar a como se hace con el resto de tipos simples, pero con alguna particularidad:

* Ignora los espacios en blanco que se introducen antes del primer carácter válido de la cadena. Es decir, si el usuario escribe `"   hola"`, con tres espacios en blanco delante, `cin` los ignorará y empezará a almacenar caracteres a partir de la `h`
* Después de haber leído un carácter válido, termina de leer cuando encuentra el primer _blanco_ \(espacio, tabulador o salto de línea\). Ese blanco se deja en el buffer de teclado para la siguiente lectura

Ejemplo:

```cpp
char cad[20];
cin >> cad;
```

Este programa quedaría a la espera de que el usuario introdujera la cadena por teclado.

#### Lectura de teclado con `cin` y `getline`

Leer de teclado usando `cin` y `>>` puede generar dos problemas:

* Problema 1: ¿Y si la cadena tiene espacios en blanco? Si por ejemplo el usuario escribiera `hola a todos`, el programa anterior leería `hola`, hasta encontrar el primer espacio en blanco, y dejaría de leer
* Problema 2: El operador `>>` no limita el número de caracteres que se leen. ¿Y si la cadena escrita no cabe en el vector? Aquí podemos tener un problema serio, porque estaremos tratando de escribir en zonas de memoria fuera del vector. Si en el ejemplo anterior el usuario escribiera `supercalifragilisticoespialidoso`, se produciría un error en la memoria al haber sobrepasado el tamaño del vector.

Una forma de solucionar estos dos problemas es utilizar la función `getline`, que pueden leer cadenas con blancos y controlar el número de caracteres que se almacenan.

Ejemplo:

```cpp
const int TAM = 10;
char cad[TAM];
cin.getline(cad,TAM);
```

En este ejemplo, `getline` lee como máximo `TAM-1` caracteres o hasta que llegue al final de línea. El `'\n'` del final de línea se lee pero no se mete en la cadena `cad`. La función `getline` se encarga de añadir `'\0'` al final de lo que ha leído. Por esa razón solo lee `TAM-1` caracteres, dejando un espacio reservado para el carácter nulo.

Si el usuario introdujera `hola a todos`, el programa almacenariá en `cad` la cadena `hola a to` \(los espacios en blanco cuentan como un carácter más\).

Esta función tiene también un problema. ¿Qué sucede si el usuario escribe más caracteres de los que caben en la cadena? En este caso, los caracteres sobrantes se quedan en el buffer de teclado y probocan un fallo en la siguiente lectura.

Ejemplo:

```cpp
char cad1[10];
char cad2[10];

cin.getline(cad1,10);
cout << "Cadena 1: " << cad1 << endl;
cin.getline(cad2,10);
cout << "Cadena 2: " << cad2 << endl;
```

En este programa, si el usuario introdujera `hola a todos` la salida mostrada por pantalla sería:

```
Cadena 1: hola a to
Cadena 2:
```

#### Problemas del uso combinado de `>>` y `getline`

Hemos visto dos formas de hacer lectura de teclado: mediante el operador `>>` y mediante `getline`. Cuando estos dos operadores se combinan, se pueden dar situaciones no deseadas.

Ejemplo:

```cpp
int num;
char cad[1000];

cout << "Escribe un numero: ";
cin >> num;
cout << "El numero leido es " << num << endl;

cout << "Escribe una cadena: " ;
cin.getline(cad,1000);
cout << "La cadena leida es: " << cad << endl;
```

En este ejemplo, si cuando el programa muestra por pantalla `Escribe un numero:` el usuario escribe `10`, por ejemplo, la salida completa por pantalla que se produce sería:

```
Escribe un numero: 10
El numero leido es 10
Escribe una cadena: La cadena leida es:
```

Vemos que no se le llega a preguntar al usuario por la cadena. ¿Por qué sucede esto?

En el código anterior, primero se lee un `int` mediante `>>`, para luego leer una cadena con `getline`. Cuando se lee `10` con el operador `>>`, éste deja de leer cuando encuentra el primer espacio en blanco \(el salto de línea en este caso, cuando se pulsa la tecla _intro_ después de escribir el `10`\) y deja ese salto de línea en el buffer. Cuando ejecuta `getline`, lo primero que se encuentra en el buffer es el salto de línea `'\n'`, por lo que termina de leer y almacena en la variable `cad` una cadena vacía.

Una solución para solucionar este problema es extraer el `'\n'` del buffer de teclado antes de hacer la siguiente lectura. Esto lo podemos hacer con el método `ignore` de la siguiente manera:

```cpp
...
cin >> num;
cin.ignore();
...
```

Aquí, `cin.ignore()` saca un carácter del buffer de teclado y lo descarta.

### Funciones de la librería `string.h`

La librería estándar `string.h` de C ofrece una serie de funciones para trabajar con cadenas de caracteres. Para poder utilizarlas hay que importar la librería correspondiente al comienzo de nuestro código utilizando la siguiente instrucción:

```cpp
#include <string.h>
```

Entre las funciones más importantes que proporciona la librería estándar, tenemos `strlen`, `strcmp` y `strcpy`.

#### `strlen`

Esta función devuelve un `int` con  el número de caracteres que contiene la cadena.

Ejemplo:

```cpp
char cad[20] = "adios";
cout << strlen(cad);
```

Este ejemplo devolvería `5`, que es el número de caracteres que contiene `cad` \(no `20` que sería el tamaño del array, ni `6` que sería el número de espacios de memoria ocupados si tenemos en cuenta que hay un `'\0'` al final\).

#### `strcmp`

Esta función compara dos cadenas en orden lexicográfico, es decir, el orden que se da en un diccionario:

* La letra 'a' es más pequeña que la letra 'b'
* La cadena "adeu" es más pequeña que "adios", porque los dos primeros caracteres son iguales pero en la tercera posición 'i' es mayor que 'e' y ya no se miran los caracteres restantes
* Las letras minúsculas son mayores que sus correspondientes mayúsculas \('a' &gt; 'A'\)
* Las letras son mayores que los números \('A' &gt; '1'\)

De la lista anterior, los dos últimos puntos no son obvios. Este comprtamiento viene dado por el código ASCII de cada caracter. El código ASCII es una representación numérica que tiene cada carácter en la memoria del ordenador.

A continuación se muestra la tabla de códigos ASCII de los 128 primeros caracteres:

![Falta imagen](./images/fig_2.png "Tabla ASCII")

El código ASCII del carácter '1' es 49, mientras que el de la letra 'A' es 65 y el de la letra 'a' es 97. Por esta razón, 'a' &gt; 'A' &gt; '1'.

`strcmp` devuelve `-1` si la primera cadena es menor que la segunda, `0` si son iguales y `1` si la segunda cadena es mayor.

Ejemplo:

```cpp
char cad1[] = "adios";
char cad2[] = "adeu";

cout << strcmp(cad1,cad2) << endl;
cout << strcmp(cad2,cad1) << endl;
cout << strcmp(cad1,cad1) << endl;
```

Este código mostraría por pantalla:

```
1
-1
0
```

Por tanto, si queremos comprobar si dos cadenas introducidas por teclado son iguales, lo podemos hacer de la siguiente manera:

```cpp
char cad1[1000];
char cad2[1000];

cout << "Introduce la cadena 1: ";
cin >> cad1;
cout << "Introduce la cadena 2: ";
cin >> cad2;

if(strcmp(cad1,cad2)==0)
{
    cout << "Las cadenas son iguales" << endl;
}
else
{
    cout << "Las cadenas son diferentes" << endl;
}
```

Otra manera más compacta de expresar `strcmp(cad1,cad2)==0` sería `!strcmp(cad1,cad2)`.

#### `strcpy`

Esta función permite copiar una cadena en otra:

```cpp
char cad[10];
strcpy(cad,"hola");
```

Las cadenas de caracteres en C son arrays y por tanto no se pueden asignar directamente. El siguiente código produciría un error de compilación:

```cpp
char cad[10];
cad = "hola";
```

El siguiente código también daría error de compilación:

```cpp
char cad1[10] = "hola";
char cad2[10];
cad2 = cad1;
```

Al tratarse de arrays, debería de hacerse la copia elemento a elemento \(carácter a carácter\) de una cadena a otra. La función `strcpy` nos evita tener que hacer ese tedioso proceso.

Un detalle importante es que la cadena receptora tiene que tener tamaño suficiente para almacenar la cadena que queremos guardar.

Ejemplo:

```cpp
char cad[10];
strcpy(cad,"Hoy es un dia fantastico para salir de paseo");
```

Este código produciría accesos a zonas de memoria no reservadas y un más que probable fallo de segmentación, al tratar de copiar una cadena de mayor tamaño de lo que admite la variable. Hay que tener siempre en cuenta que tiene que haber también espacio suficiente en la cadena para el `'\0'`.

#### Otras funciones

Las funciones `strncmp` y `strncpy` son similares a las dos funciones anteriores, pero con la diferencia de que solo comparan o copian los `n` primeros caracteres. Por ejemplo:

```cpp
char cad[10];
strncpy(cad,"Hola, mundo",4);
cad[4] = '\0'
```

En este ejemplo, se copian los `4` primeros caracteres de `"Hola, mundo"` en `cad`. El carácter nulo `'\0'` se debe de añadir explícitamente al final de los caracteres copiados. En este ejemplo, al copiar `4` caracteres, estos ocuparn las posiciones 0, 1, 2 y 3 del array `cad`, por eso el carácter nulo se añade en la posición siguiente, en la `4`.

Dos ejemplos más de funciones interesantes que trabajan con cadenas de caracteres, aunque estas no pertenecen a la librería `stdlib.h`, son `atoi` y `atof`. La primerar sirve para convertir una cadena de texto que representa un valor entero a su equivalente valor de tipo `int`. La segunda función, exactamente igual pero para el caso de valores reales. Estas dos funciones están definidas en la librería estándar `cstdlib`, por lo que habrá que incluirla al comienzo de nuestro código si queremos poder utilizarlas:

```cpp
#include <cstdlib>
```

Ejemplo:

```cpp
char cad1[] = "100";
int n = atoi(cad1);

char cad2[] = "10.5";
float f = atof(cad2);
```

En este código, la variable `n` tomará el valor `100` y la variable `f` el valor `10.5`.

## La clase `string` en C++

### Declaración e inicialización

La librería C++ estándar proporciona la clase `string` que da soporte a todas las operaciones sobre cadenas de texto mencionadas anteriormente, además de muchas otras funcionalidades.

La gran ventaja con respecto a las cadenas de caracteres en C es que la clase `string` tiene un tamaño variable que puede cambiar a lo largo de la ejecución del programa en función de la cadena que queramos almacenar: puede aumentar si queremos almacenar una cadena más grande o puede disminuir para no desperdiciar memoria si queremos almacenar una más pequeña.

La clase `string` usa intérnamente arrays de caracteres para almacenar los datos, pero el manejo de la memoria y la localización del carácter nulo lo hace de manera transparente la propia clase, que es lo que simplifica su uso.

El concepto de "clase" y su diferencia con un tipo simple lo veremos en el Tema 5, cuando nos adentremos en el paradigma de programación orientada a objetos. Para utilizar la terminología adecuada, en lugar de "variable de tipo `string`" deberíamos decir "objeto de la clase `string`", y en lugar de "funciones específicas" de `string` deberíamos de hablar de "métodos". No obstante, para no anticiparnos a los contenidos sobre programación orientada a objetos, en este tema seguiremos utilizando la terminología habitual y hablaremos de "tipos", "variables" y "funciones" en lugar de "clases", "objetos" y "métodos".

Para hacer uso de algunas de las funciones que vamos a mencionar en este apartado es necesario incluir la librería del mismo nombre:

```cpp
#include <string>
```

Las variables de tipo `string` se declaran como cualquier otro tipo de dato: ponemos el tipo, el nombre dado a la variable y opcionalmente un valor de inicialización.

Ejemplo:

```cpp
string cad1;
string cad2 = "hola";
const string cad3 = "hola";
```

Este código muestra una variable sin inicializar \(`cad1`\), una variable con el valor inicial `"hola"` \(`cad2`\) y una constante con ese mismo valor inicial \(`cad3`\). Como puede verse en este ejemplo, no se debe usar la sintaxis de corchetes ni indicar el tamaño de la cadena como se hacía con las cadenas en C.

El paso de parámetros a una función, ya sea por valor o referencia, es como con cualquier dato simple \(`int`, `float`, etc.\).

Ejemplo:

```cpp
#include <iostream>
#include <string>

using namespace std;

void analizarCadena(string cad1, string &cad2)
{
    ...
}

int main()
{
    string cad1 = "hola";
    string cad2 = "adios";

    analizarCadena(cad1,cad2);
}
```

En este código se declaran dos variables de tipo `string` en la función principal, `cad1` y `cad2`. Ambas se pasan a la función `analizarCadena`, la primera por valor y la segunda por referencia.

### Entrada y salida

Al igual que con las cadenas de caracteres de C, se utiliza `cout` para mostrar el contenido de un `string` por pantalla.

Ejemplo:

```cpp
string cad = "Hola a todo el mundo";
cout << cad;
```

Este código mostraría por pantalla `Hola a todo el mundo`.

Para leer información de teclado, se puede utilizar `cin` y el operador `>>`, como ocurría con las cadenas en C, produciéndose el mismo resultado:

* Ignora los espacios en blanco que se introducen antes del primer carácter válido de la cadena
* Después de haber leído un carácter válido termina de leer cuando encuentra el primer blanco \(espacio, tabulador o salto de línea\). Ese blanco se deja en el buffer de teclado para la siguiente lectura

Ejemplo:

```cpp
string cad;
cin >> cad;
```

Si la cadena contiene espacios en blanco y queremos leerla entera, podemos utilizar también la función `getline` que utilizábamos para cadenas de C, aunque en este caso la sintaxis es diferente:

```cpp
string cad;
getline(cin,cad);
```

La ventaja que tiene usar `getline` con el tipo `string` es que no limita el número de caracteres leídos y, por tanto, no debe de indicarse este número como parámetro de la función.

Si queremos leer hasta la aparición de un determinado carácter \(por defecto lee hasta que encuentra el salto de línea `'\n'`\), podemos indicarlo como parámetro de `getline`.

Ejemplo:

```cpp
string cad;
getline(cin,cad,',');
```

En este ejemplo se leería de teclado hasta la primera aparición del carácter `','`.

Hay que tener en cuenta que, en caso de combinar lecturas de teclado con el operador `>>` y con `getline`, tendríamos el mismo problema que ya se mencionó con las cadenas en C.

### Funciones de la librería `string`

La librería estándar `string` proporciona una serie de funciones que facilitan el trabajo con cadenas.

Al tratarse de una clase en lugar de un tipo simple, las funciones \(en realidad se llaman _métodos_ a las funciones de una clase\) de la clase `string` se invocan poniendo un `.` detrás del nombre de la variable. Como se comentó más arriba, veremos esto con detalle en el Tema 5, cuando introduzcamos la programación orientada a objetos.

Entre las funciones más importantes que proporciona la librería `string`, tenemos `length`, `find`, `replace` y `erase`.

#### `length`

Esta función permite obtener el número de caracteres que contiene una cadena. Su prototipo es el siguiente:

```cpp
unsigned int length();
```

No recibe ningún parámetro y devuelve como resultado un valor de tipo `unsigned int`, es decir, un entero sin signo, ya que el número de caracteres de una cadena nunca puede ser un valor negativo. La palabra reservada `unsigned` del lenguaje C/C++ es un modificador de tipo que indica que el valor almacenado es siempre positivo \(no puede tener signo negativo\).

Ejemplo:

```cpp
string cad = "hola";
unsigned int tam = cad.length();
```

Este ejemplo almacenaría el valor `4` en la variable `tam`.

#### `find`

Esta función permite buscar una subcadena dentro de otra. El prototipo de la función es el siguiente:

```cpp
unsigned int find(const string str, unsigned int pos = 0);
```

El primer parámetro es la subcadena que se quiere buscar, mientras que el segundo parámetro indica a partir de qué posición de la cadena queremos comenzar la búsqueda de la subcadena \(si no se indica nada ese valor será `0` y empezará a buscar por el principio de la cadena\). La función devuelve un valor entero indicando la posición dentro de la cadena \(`0` es la primera posición\) donde ha encontrado la subcadena. Si no encuentra la subcadena buscada, devuelve la constante `string::npos`.

Ejemplo:

```cpp
string a = "Hay una taza en esta cocina con tazas";
string b = "taza";

// Longitud de a
unsigned int tam = a.length();

// Buscamos la primera aparición de la subcadena "taza" dentro de la cadena "Hay una taza..."
unsigned int encontrado = a.find(b);

if(encontrado != string::npos)
    cout << "Encontrada primera << b << en la posición " << encontrado << endl;
else
    cout << "Palabra << b << no encontrada";

// Buscamos la segunda aparición de la subcadena "taza"
encontrado = a.find(b,encontrado+b.length());
if(encontrado != string::npos)
    cout << "Encontrada segunda << b << en la posición " << encontrado << endl;
else
    cout << "Palabra << b << no encontrada";
```

`find` termina cuando encuentra la primera aparición de la subcadena. En la primera búsqueda \(`a.find(b)`\) se empieza desde el inicio de la cadena, por lo que para al encontrar la primera aparición de la subcadena. En la segunda búsqueda \(`a.find(b,encontrado+b.length())`\), se comienza justo a continuación de la aparición de la primera subcadena, por lo que devolverá la segunda subcadena que pueda encontrar en la cadena. Este proceso se podría repetir introduciéndolo dentro de un bucle si nos interesara recuperar todas las apariciones de una subcadena dentro de otra.

#### `replace`

Esta función permite reemplazar una subcadena dentro de una cadena. Su prototipo es:

```cpp
string& replace(unsigned int pos, unsigned int tam, const string str);
```

El primer parámetro indica la posición dentro de la cadena donde comenzarán a reemplazarse caracteres. El segundo parámetro indica el número de caracteres que se van a reemplazar. Finalmente, el tercer parámetro indica la subcadena que se va a insertar como reemplazo. La función modifica directamente la cadena sobre la que se aplica, por lo que no es necesario asignar el valor que devuleve a otra variable.

Ejemplo:

```cpp
string a = "Hay una taza en esta cocina con tazas";

a.replace(8,4,"botella");
cout << a << endl;
```

La salida por pantalla de este ejemplo sería `Hay una botella en esta cocina con tazas`.

En este ejemplo, se han sustituido dentro de la cadena `a` un total de `4` caracteres comenzando en la posición `8` y se ha insertado en su lugar la subcadena `"botella"`.

#### `erase`

Esta función permite eliminar un conjunto de caracteres de una cadena, o eliminarla por completo. Su prototipo es el siguiente:

```cpp
string& erase(unsigned int pos = 0, unsigned int tam = npos);
```

El primer parámetro indica a partir de qué posición se empieza la eliminación de caracteres. El segundo parámetro indica cuántos caracteres se van a eliminar. Si no se indica ningún parámetro, `erase` elimina todos los caracteres de la cadena.

Ejemplo:

```cpp
string cad = "Hola a todo el mundo";
cad.erase(3,11);
cout << cad;
```

La salida por pantalla de este código sería `Hola mundo`.

### Operaciones con `string`

A las variables de tipo `string` se les pueden aplicar una serie de operadores aritméticos y de comparación.

Para asignar una cadena a otra, basta con usar el operador de igualdad `=`.

Ejemplo:

```cpp
string cad1 = "Hola";
string cad2;
cad2 = cad1;
```

Recuerda que esto no se podía hacer directamente con cadenas de caracteres en C y tenía que utilizarse la función `strcpy`.

La concatenación de cadenas \(es decir, añadir una cadena a continuación de otra\) se lleva a cabo con el operador `+`.

Ejemplo:

```cpp
string cad1 = "Hola";
string cad2 = "mundo";
string cad3 = cad1 + ", " + cad2 + "!";
cout << cad3;
```

Este código mostraría por pantalla `Hola, mundo!`

Para comparar cadenas, se pueden usar los mismos operadores que utilizamos para comparar números: `==`, `!=`, `>`, `<`, `>=` y `<=`.

Ejemplo:

```cpp
string cad1;
string cad2;

cin >> cad1;
cin >> cad2;

if(cad1 > cad2)
{
    cout << "La primera cadena es mayor que la segunda" << endl;
}
else if(cad1 < cad2)
{
    cout << "La primera cadena es menor que la segunda" << endl;
}
else if(cad1 == cad2)
{
    cout << "Ambas cadenas son iguales" << endl;
}
```

Para acceder a los distintos caracteres de un `string` podemos utilizar la misma sintaxis que utilizamos para acceder a cualquier array, mediante `[]`.

Ejemplo:

```cpp
string cad = "Hola";

for(int i = 0;i < cad.length();i++)
{
    cout << cad[i] << endl;
}
```

Este código realiza un bucle que muestra por pantalla una cadena carácter a carácter:

```
H
o
l
a
```

También se puede cambiar el valor de un carácter concreto del `string` utilizando esta sintaxis.

Ejemplo:

```cpp
string cad = "hola";
cad[0] = 'p';
cad[3] = 'o';
cout << cad;
```

Este código sustituye el primer y cuarto carácter de la cadena y muestra por pantalla `polo`.

Es importante tener en cuenta aquí que solo podemos asignar caracteres a posiciones de la cadena que ya existan.

Ejemplo:

```cpp
string cad = "hola";
cad[5] = '!';
cout << cad;
```

En este ejemplo se está tratando de cambiar el valor del carácter en la posición `5` de la cadena, pero dicha posición no existe, ya que la variable `string` solo tiene reservado espacio para almacenar la cadena `"hola"`, que va de la posición 0 a la 3.

La salida por pantalla sería `Hola`, ya que el carácter `!` no se puede almacenar en la posición especificada.

### Conversión entre `string` y números

Para convertir un número entero o real a `string` podemos utilizar la función `to_string`.

Ejemplo:

```cpp
int n = 100;
string numero = to_string(n);
```

Esta función pertenece a la versión C++11 del lenguaje, por lo que para poder utilizarla en nuestro código habría que compilar añadiendo a `g++` el parámetro `std=c++11`. Por ejemplo, si nuestro código se llama`prog.cc` deberíamos de compilarlo con el comando `g++ -std=c++11 prog.cc`.

Para convertir de cadena a número entero o real, podemos utilizar las mismas funciones que vimos para cadenas en C, `atoi` y `atof`, pero teniendo en cuenta que estas funciones esperan como entrada un array de caracteres y no un tipo `string`. Por ello, habrá que hacer una conversión previa utilizando la función `c_str` de la clase `string`, que permite convertir un `string` a cadena de caracteres en C. Este método, cuando se aplica a una variable de tipo `string`, devuelve una cadena de caracteres con su contenido.

Ejemplo:

```cpp
string cad1 = "100";
int n1 = atoi(cad1.c_str());

string cad2 = "10.5";
float n2 = atof(cad2.c_str());
```

### Sacar las palabras de un `string`

Dada una cadena de texto almacenada en un `string`, podemos extraer cada una de las palabras que contiene \(asumiendo que están separadas por espacios en blanco\) utilizando la clase `stringstream`. Para poder utilizar esta clase hay que importar la correspondiente librería al comienzo de nuestro código:

```cpp
#include <sstream>
```

Ejemplo:

```cpp
stringstream ss("Hola mundo cruel 1 32 2.3");
string s;

while(ss >> s)
{
    cout << "Palabra: " << s << endl;
}
```

En este código, para cada iteración del bucle `while`, el operador `>>` lee de `ss` hasta que encuentra un espacio en blanco y lo almacena en `s`. `stringstream` se comporta como `cin` y se lee de la misma manera, ya que ambos representan flujos de caracteres.

## Conversión entre cadenas en C y `string`

Para convertir una cadena en C a un `string`, podemos crear una variable de tipo `string` y utilizar directamente el operador `=` para asignarle su valor.

Ejemplo:

```cpp
char cad1[] = "hola";
string cad2 = cad1;
```

Para convertir un `string` a cadena de caracteres en C, podemos usar la función `c_str` de la clase `string` que mencionamos más arriba, junto con la función `strcpy` para hacer la copia entre cadenas.

Ejemplo:

```cpp
string cad1 = "hola";
char cad2[10];
strcpy(cad2,cad1.c_str());
```

## Comparativa entre cadenas de caracteres en C y `string`

Resumimos en la siguiente tabla cómo se hacen las mismas cosas utilizando cadenas de caracteres en C y utilizando el tipo `string`.

| vectores de caracters | `string` |
| ---: | :--- |
| `char cad[TAM];` | `string s;` |
| `char cad[] = "hola";` | `string s = "hola";` |
|  |  |
| `strlen(cad);` | `s.length();` |
| `cin.getline(cad,TAM);` | `getline(cin,s);` |
| `if (!strcmp(c1,c2)){...}` | `if (s1 == s2){...}` |
| `strcpy(c1,c2);` | `s1 = s2;` |
| `strcat(c1,c2);` | `s1 = s1 + s2;` |
|  |  |
| `strcpy(cad,s.c_str());` | `s = cad;` |
|  |  |
| Terminan con `\0` | NO terminan con `\0` |
| Tamaño reservado fijo | El tamaño reservado puede crecer |
| Tamaño ocupado variable | Tamaño ocupado = tamaño reservado |
|  |  |
| Se usan en ficheros binarios | NO se pueden usar en ficheros binarios |



