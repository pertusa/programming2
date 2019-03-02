# Unit 3 - Files

In this unit we will learn to work with both text and binary files to perform basic operations: opening, reading, writing and closing. In addition we will discuss the main problems that we can find when working with files, and their solutions.

A file is a series of data on a storage device under a single identifying name and a path.

## Text files

Text files (also called **files with format**) are those that contain only printable characters. In ASCII code, they are the characters from code 32, which corresponds to the blank space: ' '. We can view the contents of text files using any text editor, unlike it happens with binary files.

Text files can be created with different character sets: ASCII (the default code for text files), EBCDIC, Unicode (in its most commonly used coding, UTF-8). A character set (also code character code) encodes each written symbol (letters, numbers, symbols, etc.) using a number. The file contents do not need to follow any format or special layout (you can not 'decorate' the text). In some cases they have special characters to indicate end of line (EOLN by _End Of line_) and to mark the end of file (EOF by *End of File*).

Examples of text files:

* A C++ source code file
* A webpage written in HTML
* A config file of an operating system (.ini in Windows, .conf in Unix)
* A README.txt file

### Reading example

This is an example code to read a file contents line by line. All the instructions needed (declaration, opening, reading and closing) are detailed in the next sections.

```cpp
#include <iostream>
#include <fstream>

using namespace std;

int main() 
{
      ifstream fi("myfile.txt"); // We declare a read-only file and open it from the disk
      if (fi.is_open()) // We check that the file could be opened
      {
            string s;
            while (getline(fi,s))  // Read line by line until the end of file is found
            {
                  cout << "Line read from file: " << s << endl;
            }
            fi.close(); // Close the file
      }
      else cout << "Error opening file" << endl; // An error should always be given if the file could not be opened
}
```

### Declaration

We must include the _fstream_ library to work with files:

```cpp
#include <fstream>
```

Then, we have several options to declare a variable for a file.

```cpp
ifstream readFile;  // If we only want to access the file for reading its contents
ofstream writeFile; // If we only want to access the file for writing on it
fstream readWriteFile; // If we want to perform both operations (reading and writing)
```

Take into account that is very unusual to need a text file that uses both modes simultaneously (reading and writing).

### Opening and closing operations

Once a file variable is declared it is necessary to open it (tell the filename on disk). The function ```open``` can be used for this task:

```cpp
bool open(const char[] filename, ios_base::openmode mode);  // Before C++11
bool open(const string &filename, ios_base::openmode mode); // Since C++11
```

For example:
```cpp
ifstream fi; // We declare a read-only file variable
fi.open("myfile.txt"); // We open the file called "myfile.txt" from the current path
```

Declaration and initialization can also be done in a single line:

```cpp
ifstream fi("myfilename.txt"); // We declare the variable and open it in read-only mode
```

Opening a file may yield some errors in runtime, for example when the file does not exist but it was opened for reading, or when the file is in a restricted path (we do not have permissions there) and is opened for writing. Therefore, after opening a file it is important to always check that this operation succeeded. This can be done with the function ```is_open()```:

```cpp
if (fi.is_open()) 
{
   // We can read from the file...
} 
else 
{
   // Opening error
}
```

Finally, once we are done with the file, we have to close it to free resources and allow other programs to work with that file. For this, we use the function ```close()```:

```cpp
fichero.close();
```

### Opening modes

There are different modes that can be specified when opening the file, for example:

```cpp
// Declaration and initialization 
ifstream fi;
fi.open("myfilename.txt", ios::binary);
// Equivalent to:
ifstream fi("myfilename.txt, ios::binary);
```

The opening modes can be as follows:

| Mode | Description | Constant in C++ |
| -- | -- | -- |
| Reading | Only reading, starting from the beginning of the file (not necessary when using `ifstream`) | ```ios::in``` |
| Writing | Only writing, starting from the beginning of the file and overriting the file contents (not necessary when using `ofstream`) | ```ios::out``` |
| Reading and writing | Allows us to read and write contents to the file (not necessary when using `fstream`) | <code>ios::in &#124; ios::out</code> |
| Appending at the end of the file | Start writing from the end of the file without erasing its previous contents | <code>ios::app</code> |
| Move to the end of file | This option allows us to start reading or writing from the end of the file after opening it | ```ios::ate``` |
| Binary file | This mode allows us to use binary files. We will see this in detail in the binary files section | ```ios::binary``` |
| Truncate/erase contents | This mode allows us to erase all the contents of the file after opening it in write mode | ```ios::trunc``` |

### Reading text files

#### Detecting the end of file

When reading a file we need when we have reached the end of it to stop reading.

To know if we are at the end of a file after a reading operations, one possibility is to use the ```eof()``` method. For example, we could read the contents of a file through a loop that after each reading operation checks if the end of the file was reached:

```cpp
ifstream fi("myfile.txt");
if (fi.is_open())
{
      while(!fi.eof())
      {
            // Read file contents
      }
      fi.close();
}
else cout << "Error opening file" << endl;
```

This method returns a _Boolean_ value (*true* or *false*) depending on whether or not we are at the end. When we perform a reading operation (for example, with _cin_ or _getline_) of any data (character, integer, etc.) that cannot be done, ```eof``` returns *true*. But, **careful**, after having read **the last valid data** the method will still return *false*. Therefore, it is necessary to perform an additional reading (which result must be discarded, since it contains  invalid data) to cause the detection of the end of the file and make the ```eof``` function return *true*.

#### Reading line by line

There are alternative ways of reading a text file. For example, if wanted to read line by line:

```cpp
if (file.is_open()) 
{
   string s;
   getline(file,s);
   while (!file.eof()) 
   {
      // do something with s
      getline(file,s);
   }
   flie.close();
}
```

Note that we perform a reading operation before the while, and then we repeat the reading at the end of the loop. The following alternative is completely wrong:

```cpp
if (file.is_open()) 
{
   string s;
   while (!file.eof()) 
   {
      getline(file,s); // WRONG!
      // do something with s 
   }
   flie.close();
}
```

Why is it wrong? Consider for example that the file to be read is empty, or that we are in the last line of the file. With the second code, the end of file will be read first and we would process the string _s_, even if it is empty because ```getline``` could not read anything. In contrast, the first code deals correctly with this situation.

However, there is an alternative for reading a file which is easier and **recommended** when possible (in some situations it cannot be used). Here we do not need the ```eof()``` method because the function ```getline()``` already returns *false* when it cannot read anything else:

```cpp
if (file.is_open()) 
{
   string s;
   while (getline(file,s)) 
   {
      // do something with s
   }
   file.close();
}
```


#### Reading char, int, double, etc.

The following example shows how to read character by character. C++ files are objects of the class *stream*, therefore they can be accessed as input/output buffers using the ```>>``` and ```<<``` operators, in the same way what we do with *cin* and *cout* standard input / output buffers.

```cpp
if (file.is_open())
{
   char c; // This could also be int, float, etc.
   while (file >> c)
   {
      // do something with c
   }
   file.close();
}
```

In the following example we also read character by character, but considering that the file may contain blank spaces and we want to read them too (remember than blank spaces are ignored by the operator ```>>```):

```cpp
if (file.is_open())   
{
   char c;
   while (file.get(c)) 
   {
      // do something with c
   }
   file.close();
}
```

#### Reading with known mixed data types

Consider that we want to read the contents of a file that has several lines containing a string followed by two integers, for example:

```
hola 123 1024
mundo 43 23
```

To read a file like this we could use the following code:

```cpp
ifstream file("example.txt");

if (file.is_open())
{
   string s;
   int i,j;
   while (file >> s) // Read string
   {
      file >> i; // Read first number
      file >> j; // Read second number
      cout << "Read: " << s << "," << i << "," << j << endl;
   }
   file.close();
}
```

Alternative code (more compact):

```cpp
ifstream file("example.txt");

if (file.is_open())
{
   string s;
   int i,j;
   while (file >> s >> i >> j) // Read string, first number and second number
   {
      cout << "Read: " << s << "," << i << "," << j << endl;
   }
   file.close();
}
```

This is another example for a mixed-type contents (only a line with a number indicating the number of words, and then the words):

```
3 pedro antonio jordi
```

The code to read this file could be:

```cpp
if (file.is_open())
{
   string s;
   int n;
   file >> n;
   for(int i=0; i<n; i++) {
      file >> s;
      cout << "Name(" << i+1 << ")=" << s << endl;
   }
   file.close();
}
```

### Writing

To write in a text file we can use the output operator ```<<```, because as previously explained, files are *stream* objects and can be used as data input/output buffers.

```cpp
ofstream fo("results.txt"); // ofstream for writing

if (fo.is_open()) 
{
   const unsigned n = 10;
   for (unsigned i = 0; i < n; i++)
   {
         fo << "Printed: " << n << endl;
   }
   fo.close();
}
```

In this example, we simply open a file and write there "Printed: " and a variable called *n*. It must be taken into account that **the first writing operation erases all the previous contents of the file**. If we do not want to do so, we should use ```ios::app```.

Another writing example:

```cpp
ofstream fo("results.txt", ios::app); // ios::app means that the new contents will be appended at the end of the existing contents of the file
if (fo.is_open())
{
   fo << "Write something."
   fo << "Something else" << endl;

   fo.close();
}
```

### Exercises

#### Exercise 1

Make a program that reads a file called _myfile.txt_ and prints on screen only those lines containing the substring "Hello".

#### Exercise 2

Make a program for reading a file called _myfile.txt_, writing in another file _myfileUpper.txt_ the same content of the input file but with all the letters in uppercase. Example:

| myfile.txt | myfileUpper.txt |
| --- | --- |
| Hello, world. | HELLO, WORLD. |
| How are you? | HOW ARE YOU? |
| Bye and 1000 times bye | BYE AND 1000 TIMES BYE |

#### Exercise 3

Implement a program for reading two text files, _f1.txt_ and _f2.txt_, writing on the screen those lines that differ in both files. The program should print _<_ when the line corresponds to _f1.txt_, and _>_ when it corresponds to _f2.txt_. Example:

| f1.txt | f2.txt |
| --- | --- |
| Hello, world. | Hello, world. |
| My name is andrew | My name is Andrew |
| Bye, bye | See you soon |

El resultado debe ser:

```
Line 2:
< My name is andreu
> My name is Andreu
Line 3:
< Bye, bye
> See you soon
```

#### Exercise 4

Implement a function called _printLastLines_ which receives two arguments: the first one must be a positive integer _n_, and the second the name of a text file. The function must print on the screen the last _n_ lines of the given file. Example:

```cpp
printLastLines(3, "myfile.txt");
```

Assuming that the last three lines of the file are as follows, the output of the program should be:

```
with several worlds
one word
many, many, many words
```

This exercise can be solved in two possible ways: 
* Using "brute force": all the file could be read to count the number of lines, and then read it again to write the last _n_ lines. However, this is not efficient: what happens if the file had millions of lines?
* Using a vector of strings of size _n_ which stores at every time the last _n_ lines. At the beginning, it will contain less than _n_ lines.

#### Exercise 5

We have two text files _f1.txt_ and _f2.txt_ in which each line is a series of numbers separated by ':'. Assuming that the lines are in ascending order by the first number, make a function to read both files line by line, writing the common lines into the file _f3.txt_ like in the following example:

| f1.txt | f2.txt | f3.txt |
| -------- | -------- | -------- |
| 10:4543:23 | 10:334:110 | 10:4543:23:334:110 |
| 15:1:234:67 | 12:222:222 | 15:1:234:67:881:44 |
| 17:188:22 | 15:881:44 | 20:111:22:454:313 |
| 20:111:22 | 20:454:313 | |

<!---
## Binary files

A binary file stores a sequence of bytes. In these files, the data is stored as it is in the computer's memory. Unlike in text files, bytes are not converted into characters when they are saved in the file. For this reason, binary files are also called **files without format**. When we try to open these files using a standard text editor, we can see weird characters.

Examples of binary files:

* An image file
* A compiled program (executable file)
* A pdf file
* An mp3 file

Usually, when a program works with structs, binary files are used to store their information. One advantage of this is that whether in text files we need to start reading from the beginning of the file (what is called _secuential access_), in binary files we can access directly to the _n_-th element (struct) without reading the previous data. This is why binary files are said to have _direct (random)_ access.


### Reading example

### File reading example

This is an example code to read a file contents of a binary file. All the instructions needed (declaration, opening, reading and closing) are detailed in the next sections.

```cpp
#include <iostream>
#include <fstream>

struct Student 
{
      char id[10];
      int group;
      float mark;
};

using namespace std;

int main()
{
      ifstream fi("myfile.txt", ios::binary); // We declare a read-only binary file and open it from the disk
      if (fi.is_open()) // We check that the file could be opened
      {
           Student student;
           // Loop for reading all students from a binary file
           while (fi.read((char *)&student, sizeof(student))) 
           {
                  // process ’student’
           }
           fi.close(); // Close the file
      }
      else cout << "Error opening file" << endl; // An error should always be given if the file could not be opened
}
```


### Declaration

Declaration is similar than with text files. 

### Opening and closing

Here there is a difference, we need to add _ios::binary_ when opening the file:

```cpp
ifstream fi; // We declare a read-only file variable
fi.open("myfile.dat", ios::binary); // We open the binary file called "myfile.dat" from the current path
```

Declaration and initialization can also be done in a single line:

```cpp
ifstream fi("myfilename.dat", ios::binary); // We declare the variable and open it in read-only mode
```

A binary file can also be opened for writing:

```cpp
ofstream fo("myfilename.dat", ios::binary);
```

Or for reading and writing (this is more common than in text files):

```cpp
fstream fo("myfilename.dat", ios::binary);
```

Finally, to close a binary file, just as with text files, the function```close()``` is used.

```cpp
fi.close();
```

### Reading

For reading binary files we can **only** use the function  ```read```, which receives two parameters:

* The variable (struct or other data type) where the read data will be stored.
* The amount of bytes to be read from the file. This size can be known using the function ```sizeof()```. 

In the next example we read the contents of a binary file that contains many structs of type *CITY*.

```cpp
struct City { 
      char name[20];
      int population;
      int extension;
};

City city;
ifstream fb;

fb.open("myfile.dat", ios::binary);

if (fb.is_open())
{
   while (fb.read((char *)&city, sizeof(city)))
   {
      // process variable ’city’
   }
   fbl.close();
}
```

As we can see, with the while loop we read structs until the end of file is found. The first parameter of the function **read** is the variable where we will store the data, and the second is the number of bytes that the program should read. 

Note that:
* The first parameter also contains **(char *)&**. This is required to tell the compiler to read byte by byte, as a _char_ type always occupies one byte. 
* The second parameter is the size of the struct (in bytes) to be read, which can be calculated with **sizeof**. In this second parameter, instead of _city_ we could also have used _City_, as _sizeof_ returns the same size in both cases.

`IMPORTANT!: To read the contents of a binary file we need to know in advance how was it written (the data type or *struct* that it contains).`

`VERY IMPORTANT: As we need to know the size in bytes of the data to be read, it is mandatory that this data will have a constant size, or alternatively use a variable which tells us how many bytes should the program read.`

For this reason, we cannot store strings into a binary file (ok, actually we could with a trick that we will see later, but in Programming 2 we will only use char arrays in binary files).

### Direct access

If the file contains a series of elements of constant size, we can access directly to an element by calculating its position in function of the data size.

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
if (fbl.is_open()) {
	// nos posicionamos justo ante el tercer elemento:
	fbl.seekg( (3-1)*sizeof(ciudad), ios::beg); // contamos el tercer registro de tamaño ciudad desde el principio
	fbl.read( (char *)&ciudad, sizeof(ciudad) );
}

``` 
En este otro ejemplo, queremos acceder al **último** elemento del fichero:
```cpp

if (fbl.is_open()) {
	// nos posicionamos justo ante el tercer elemento:
	fbl.seekg( (-1)*sizeof(ciudad), ios::end); 
	fbl.read( (char *)&ciudad, sizeof(ciudad) );
}

``` 
A la función *seekg* le pasamos una posición negativa, relativa al final del fichero.

###Escritura de ficheros binarios

Para guardar datos en un fichero binario se usa la función ```write(registro, tamaño)```, y de forma parecida a la función de lectura, se le pasan dos parámetros:

* El registro que contiene los datos que van a enviarse al fichero.
* La cantidad de bytes que vamos a escribir. Con la función ```sizeof()``` calcularemos la cantidad de bytes a escribir. 

En el siguiente ejemplo de código escribimos en un fichero binario llamado "mifichero.dat" un registro de tipo *TIPOCIUDAD*.

```cpp
typedef struct {  } TIPOCIUDAD;

TIPOCIUDAD ciudad;
ofstream fbe("mifichero.dat", ios::binary);

if (fbe.is_open())
{
	// introducimos datos en el registro ’ciudad’
	ciudad = ...;
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

if (fbe.is_open()) {
	// nos posicionamos para escribir en el quinto elemento:
	fbl.seekp( (5-1)*sizeof(ciudad), ios::beg); 
	fbl.write( (const char *)&ciudad, sizeof(ciudad) );
}

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

---->
