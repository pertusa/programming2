# Unit 4 - Dynamic memory

In this unit, we describe what happens in the memory when we declare variables in C++. We will learn the differences between static memory and dynamic memory, and how to use dynamic memory to store data whose size is variable and/or unknown at compilation time.

## Memory management

Each variable that we use contains information that is stored in the memory that is assigned to our program. The amount of memory that each variable occupies is determined by its data type. For example, one ```char``` is one byte in memory. As you know already, we can use a simple variable when we want to store a single value \(for example, an integer or a character \), or an _array_ when we want to store a series of data of the same type under the same variable name. If we use an array, we must specify in its declaration the maximum number of values that it can store.

However, in some situations, when we write the code (at **compilation time**) we do not know the amount of information we need to store in memory. Sometimes, the amount of necessary memory is only known once the program is running (at **runtime**), for example if we ask the user to enter the size of an array and then we declare an array with that size.

The memory of a program is actually a large array of bytes where all declared variables are stored. During the program execution, each variable (such as a simple type or an array) is located somewhere in an area of memory. The location of a variable in the memory (measured in bytes) is called its **address**. Therefore, a memory address actually represents the index in the memory array where the variable was stored.

### Static memory

Each time we declare a variable which size is known at compilation time, we are allocating static memory. We can include in this category all the variables \(simple types or arrays\) and local parameters of the functions, as well as the global variables declared as we have been doing so far. For example, these variables are stored in static memory:

```c++
int i = 0;
char c;
float vf[3] = { 1.0, 2.0, 3.0 };
```

The following figure shows how these variables could be allocated in the memory. The cells represent the value that the variables contain, and the numbers under each cell are the **location (memory address)** that each variable occupies in memory. Actually, depending on the type of the variable, it could occupy more or less bytes (for example, a char is 1 byte long and a float could be 4). However, for simplicity, in the following examples each variable will always occupy two bytes, and we will assume that they usually start from the memory address 1000 (althought there could be any initial address).

![Disposición de las variables en memoria](memoria.png)

### Dynamic memory

Dynamic memory is normally used to store large volumes of data whose exact size is unknown when implementing the program. Unlike with static memory, in this case the amount of data to be stored is calculated during the execution of the program (in **runtime**), and it can also change while the program is running. Examples of dynamic memory classes that we have already used in Programming 2 are ```vector``` and ```string```.

In C++, **dynamic memory** is implemented using **pointers**. Internally, classes such as vector or string make use of this special data type. Next, we will see what a pointer is, how can we declare it, and how to use it.

## Pointers

### Definition and declaration

A **pointer** is a **number** that stores a memory address. In order to declare a pointer, we use the character ```*``` and we also need the data type of the memory address that it will store, so the compiler will be able to interpret its value. Therefore, pointers are declared using the character ```*``` before the variable name. For example:

```c++
int *pointerToInteger;  // This variable is a pointer to an integer 
char *pointerToChar;  // This variable is a pointer to a character
int *arrayOfPointersToInteger[ARRAYSIZE]; // This variable is an array of pointers to an integer
double **pointerToPointerToDouble; // This variable is a pointer to a pointer of double
```

As can be seen in the last example, a pointer may contain a memory address of another pointer (indicated in the example with  \*\*\).

### Address and content

In C++ we have two operators to deal with pointers:

| `&x` | Memory address of the variable `x` |
| :---: | :--- |
| `*x` | Contents of the memory address pointed by  `x` |

Let's see an example of how these operators work:

```c++
int i=3;
int *pi;  // The pointer is declared, but not initialized yet
pi = &i;  // Operator &. We initialize the pointer pi to contain the "memory address of the variable i"
*pi = 11; // Operator *. "Contents of the memory address pointed by pi" = 11.

cout << i << endl; // Prints 11
```

When a pointer contains a memory address of a given variable (in this example, this is set in the third line), we say that the pointer is **pointing** to that variable.

The easiest way to assign a memory address to a pointer is using the **operator &** to set the address of a static variable. This way, we can use a pointer to read or modify an existing variable value as if it were the same variable. Memory locations are determined by the operating system and it is possible that variables' addresses may be different during repeated runs of a program.  

> Now you should understand why we use ```&``` in C++ to pass a variable by reference: in this case, we are passing the memory address to the function, therefore those changes in the variable value inside the function will remain after the function ends.

With the **operator \*** we access to the **contents** of the memory address that a pointer has. For using this operator, we need to be sure that it points  to a valid memory address.

In the following example you can see how the memory contents would be after running the previous code example.

![Disposición de las variables en memoria](memoria2.png)

As can be seen, when the program executes `pi = &i`, the value of pi \(the pointer\) is set to 1000, which is the memory address of the variable i. When executing the instruction `*pi = 11` this memory location is accessed to write there the value 11, so actually it is as if we had modified the variable i, but we have done this indirectly.

### Declaration with initialization

As with all other variables, it is recommended to always initialize a pointer at its declaration instruction. For example:

```c++
int *pi = &i; // pi contains the memory address of i
```

When we declare a pointer but still do not know which address it will point to, it is convenient to use the value ```NULL```. A NULL value indicates that a pointer is not pointing yet to any variable:


```c++
int *pi = NULL;
```

> Whenever a pointer does not have assigned memory, it should be NULL. Before accessing a pointer, this will allow us to check if it points to a valid memory address. Example:

```c++
if (pi != NULL)
{
    *pi = 11;
}
```

### Exercises

#### Exercise 1

To solve this exercise, you should write in paper the memory array and update its values as it happens in the memory figures (for example, the figure where i is set to the memory address 11).

Indicate what would be the output of the following two code fragments:

```c++
// Fragment 1
int e1;
int *p1, *p2;
e1 = 7;
p1 = &e1;
p2 = p1;
e1++;
(*p2) += e1;

cout << *p1;
```

```c++
// Fragment 2
int a=7;
int *p=&a;
int **pp=&p;

cout << **pp;
```

## Using pointers

So far we have seen how to use pointers to access static variables. However, the main utility of pointers is to dynamically allocate memory every time we need to store new information without having to resort to static variables.

### Allocate and free memory

In order to assign a new dynamic memory address to a pointer, we have to **allocate memory** using the function **new**. When this memory is no longer necessary we must **free it** using the **delete** function. For example:

```c++
double *pd;
pd = new double;     // Allocate memory
if (pd!=NULL) 
{
    *pd = 4.75;
    cout << *pd << endl; // Prints the value pointed by pd (4.75)
    delete pd;           // Free the memory
}
else
{
    cout << "There is not enough memory to allocate the pointer pd" << endl;
}
```

As you can see in the second line, when allocating memory it is required to specify again the type of data that will contain this memory. This is necessary  for the program to reserve the amount of memory needed \(in bytes\) to store the new information. The function ```new``` may fail if the program ran out of memory (for example, when allocating 50Gb of memory). In this case, ```new``` will return ```NULL```. In the following figure you can see the representation of the variables from the previous example.

![Disposición de las variables en memoria](memoria3.png)

Those variables allocated with `new` are stored in different memory areas than static memory variables. When `new` is used, the program reserves some memory and returns the starting address of that reserved space, which in this example is stored in the pointer ```pd```.

Whenever memory is reserved with ```new``` we must release it with ```delete``` when it is no longer necessary to continue using it, so that space will be available for future allocations. If a program does not correctly release the reserved dynamic memory and it makes many reservations or runs continuously for a long time, it can exhaust all the available memory.

> After using `delete`, the memory is freed but the pointer is not set to NULL by default. If you need to reuse that pointer, it is convenient to assign it the NULL value manually to avoid accessing a memory area that is no longer available for the program. Remember that you should always check that the pointer is different from NULL before accessing its value, especially when the pointers are used in a place in the code that is far from its declaration. You can also reassign the pointer to contain another memory address using ```new```.

### Pointers and arrays

In the previous example we used a pointer to reserve memory that will store a single variable, which actually is not very useful. However, pointers can also be used to create arrays or matrices. In order to allocate memory for an array, you must specify the size of the array in brackets ```[]```. Once we have finished with the variable, it will also be necessary to use the brackets to free the memory of the vector when using ```delete```. For example:

```c++
int *pv;
int n = 10;
pv = new int[n]; // Allocate memory to store n integers
pv[0] = 585;     // We can use the pointer as if it was an array
delete [] pv;    // We must free the allocated memory. In this case, we do not need to indicate the array size between brackets, but brackets are mandatory to free all the allocated memory
```

Since pointers can store the memory address of any variable, they can also be used to directly access a position of an array. For example:

```c++
int v[ARRAYSIZE];
int *pv;
pv = &(v[7]); // pv contains the memory address of the eighth element of the array v
*pv = 117;    // v[7] = 117
```

### Pointers and structs

Pointers can also be used to contain memory addresses of structs. Once memory has been assigned to a struct, we can access the fields of that struct using the **dot operator \(.\)** as if it were a normal struct, although for this it is necessary to first access the pointer contents with **\***. For example:

```c++
struct MyStruct
{
    char c;
    int i;
};

MyStruct *pr;
pr = new MyStruct;
(*pr).c = 'a'; // Assigns a value to the field c of the struct
```

> In the previous example, the parentheses around the expression `*pr` are necessary, since otherwise the value of ```c``` would be interpreted as a pointer  as if we had written  ```*(pr.c)```.

In order to facilitate the clarity of the code, C++ also allows a special syntax to access the fields of a struct referenced by a pointer. For this, we can use the  **operator **```->```. For example:


```c++
struct MyStruct 
{
    char c;
    int i;
};

MyStruct *pr;
pr = new MyStruct;
pr->c = ’a’; // Equivalent to (*pr).c = ’a’;
```

### Pointers defined with ```typedef```

As we already mentioned in Unit 1, we can use `typedef` to define new data types:

```c++
typedef int integer; // integer is an alias for the type int
integer a,b;         // Equivalent to: int a,b;
```

In some situations, we can define pointers with ```typedef``` to make the code clearer:

```c++
typedef int *PointerToInt;
int i = 0;
PointerToInt pi = &i;  // Equivalent to: int *pi;
```

### Pointers as parameters of functions

Pointers can also be used as parameters of functions, allowing us to pass dynamically allocated variables to a function. For example:

```c++
void f (int *p)   // The pointer is passed by value
{      
    *p = 2;
}

int main()
{
    int *q = new int;
    f(q); // The function f modifies the value referenced by the pointer
    cout << *q << endl; // Prints "2"
}
```

> Even if a pointer is passed by value, you can modify the contents of the memory address to which it points, as we are passing by value the memory address where it points to, **not its contents**. This is the reason why C arrays are always passed by reference, since they are actually pointers.

If we pass a pointer by reference, then we are allowing the modification of the **memory address** it contains, being able to point it to a different memory area. For example:

```c++
void f2 (int *&p)   // The pointer is passed by reference
{
    int num = 10;
    p = &num;  // Now, p points to the memory address of num
}

int main()
{
    int i = 0;
    int *p = &i;
    f2(p);  // The function f modifies the memory address pointed by p
    cout << *p << endl; // ERROR!, since num does not longer exist we cannot access to its address
}
```

In the previous example, we have made ```p``` to point to a local variable of the function. When the execution of the function is finished, all its local variables are removed from the memory, so that the memory address contained in the pointer is no longer valid. If we try to access the contents of ```p``` we may have a **segmentation fault** and the program would end abruptly.

Finally, if we use `typedef` to define data types with pointers, we can use these new types to pass pointers as parameters to functions. For example:

```c++
typedef int* PInt;

void f (PInt p) // By value
{
    *p = 2;
}

void f2 (PInt &p)  // By reference
{
    int num = 10;
    p = &num;
}

int main() 
{
    int i = 0;
    PInt p = &i;
    f(p);
    f2(p);
}
```

## Common mistakes using pointers

Incorrect management of pointers is the main cause of segmentation faults in C/C++ programs. Below are the most common errors when using pointers:

* **Use a pointer without having assigned it a memory address.** As with the uninitialized variables of simple data types, a pointer can contain an arbitrary memory address if it is created but not initialized. Also, we should not access to the memory address of a pointer containing the NULL value.

```c++
int *p;
*p = 7; // Error !!!
```

* **Use a pointer after having freed it.** As in the previous case, once the memory of a pointer is released it is no longer safe to access its contents.

```c++
MyStruct *p,*q;
p = new MyStruct;
...
q = p;
delete p;
q->num = 7; // Error!!!
```

* **Release non-reserved memory** If we try to free the memory of a pointer that contains a reference to a static variable, the program will end with an runtime error.

```c++
int *p = &i;
delete p;
```

### Exercises

#### Exercise 2

Given the following struct, which is the same than in the binary files exercises:

```c++
struct Student
{
      char id[10];
      char surname[40];
      char name[20];
      int group;
};
```

Make a program to read a student \(only one\) from a binary file, store it in dynamic memory using a pointer, print its contents and finally release the reserved memory.
