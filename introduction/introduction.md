# Unit 1 - Introduction

In this unit we will review the topics given in Programming 1, adding concepts on algorithm design, methodology and C++ syntax.

## Design of algorithms and programs

As you know, in order to develop a program it is necessary to create one or several source code files written in a programming language. Once we have the code, through a compiler we can transform it into an executable program that the computer can run.

Therefore, it is necessary to write source code, compile it and as a result we can execute our program. However, before starting to write code we need to analyze first the **requirements** and think about the **design** of our algorithm.

The development phases of a program are the following:

1. Study of the requirements of the problem
1. Algorithm design (can be done on paper)
1. Writing the source code on the computer
1. Compilation of the program and correction of errors
1. Execution of the program
1. Test of all (or almost) possible cases

The process of writing, compiling, executing and testing should be iterative, evaluating independent functions or modules.

### 1. Requirements

To start a program we ned to know in advance the **requirements**, that is, **what** our program should do. In some cases this is simple. For example, if we wanted to make a program to show the first _n_ numbers. However in other cases it is much more complicated, for example, if we are planning to develop an accounting software for a company.

Before starting to design the program, it is necessary to have a list of requirements with all the options that it will contain. This is a sample list for a game:

* There are several levels sorted by difficulty
* In each level there will be pigs, birds and other objects with which they interact
* The user can throw a bird with a slingshot to kill the pigs
* A pig is destroyed if a bird or an object hits it.
* We can have several types of birds.
* The user can touch a bird in flight to make special actions depending on the bird's type.
* etc...

As you can see, the list of requirements can be very long. In the assignments of Programming 2 (P2) we will clearly provide the list of requirements to avoid questions about what the program should do and how the user could interact with it.

### 2. Algorithm design

Once we know the requirements, we have to think about **how** are we going to implement our program. This is done in the design phase, and includes detecting the necessary data types, deciding the functions that we need and developing a program flow.

The design stage is very important, and it can be done on paper. For this, first it is advisable to think about what **data** do we have (in the previous example, birds, pigs, objects, levels, background would be structs and/or classes if the program is object-oriented). Also, we need to think about what are we going to do with them (launchBird, impactWithPig, touchBird, etc., which will be our  **functions** or methods in object-oriented programming).

In the case of programs with a graphical interface (for example, an smartphone app), we can perform the design using a programming environment (also known as IDE) for adding screens, buttons, views, etc. In the case of a program without graphics (as in this subject), the design is limited to creating the types of data needed and the necessary functions to work with them.

In theory, with a good design we can already implement the code, and it is not necessary to redesign the program. In reality, for complicated programs it is likely that, although the initial design is good, we need to redo some parts. For example, we could make a smartphone app and realize that a button is not as adequate in a certain location as we thought, and this may force us to change the design. Anyway, it is clear that if we make a good initial design it will be easier to continue with the following phases, so it is advisable to dedicate time to think this stage properly.

### 3. Writing the code

Once we are clear about how we are going to make the program, we can proceed to **write its source code** using a text editor, or alternatively an integrated development environment (IDE, _Integrated Development Environment_).

It is very important not to write too much code (for example more than 50 lines) at once. Instead, you should  write a few lines of code (or a function), and then compile it. After fixing the compilation errors and testing that the code fragment does what is expected, we can continue writing.

If we make a lot of code without checking it, when we try to compile it it will likely get a lot of compilation errors and we will not be able to solve them properly.

### 4. Compilation and error correction

A compiler interprets or converts source code into a program that the computer can run. In most cases, after writing code we can get some compilation errors. These errors must be corrected and the code should be rewritten and compiled until no more errors occur.

There are two types of errors: compilation errors, which prevent generating an executable, and compilation _warnings_, which allow us to generate the executable but warn us that something may be wrong. It is convenient to fix all _warnings_, since many times they indicate that something is not right and ends up producing errors during the next stage, the execution of the program.

### 5. Execution

Once we have compiled a new fragment of our program, we must run it to check that the code does what is expected. Sometimes this is not the case and we have to go back to stage 3, rewriting the code, compiling and executing it again.

### 6. Test

After running the program we can verify that everything works as expected but, is that the case in all possible situations? What happens if, for example, the user types in an incorrect value? Is everything still working well?

In principle, it is difficult to control all possible cases that may occur during the execution of the program, but there are some tricks consider them.

For example, we have the following function:

```cpp
float division(int a, int b) {
	float result = a/b;
	return result;
}
```

There are two execution errors in the code, can you see them?

If we run it, we could see the first error right away.


```cpp
cout << division(3,4) << endl;
```

The result will be 0, since we have made an integer division. We can fix it:

```cpp
float result = (float)a/b;
```

As soon as one of the two operands is of type _float_, the result will be _float_. However, the second error is more complicated. How could we find it?

The answer is that **for each function we must consider all the possible values ​​of its parameters** and see if the output will be correct with them. For example, this function receives two integers. Their value can be positive, negative, or zero.

If any parameter is negative, would it work? Yes.

If any parameter is zero, would it work? No, since divisions can not be made by zero and _b_ could have that value.

Therefore, to obtain a correct function we would need to make changes:

```cpp
float division(int a, int b) {
	float result=0;
	if (b!=0) {
		result = (float)a/b;
	}
	else {
		cout << "Error, division by zero is not allowed!" << endl;
	}
	return result;
}
```

## C++

In this section we will focus on the C ++ language, which is the one used in Programming 2. There are many C++ references, but if you want to complement this book we recommend <a href="http://www.cplusplus.com">cplusplus</a> and <a href="http://www.minidosis.org">minidosis</a>.

### Basic elements

In a source code we can find different basic elements:

* Identifiers: Variable names, functions, constants, etc.
* Constants: 123, 12.3, 'a', etc.
* Reserved words: `if`, `while`, etc.
* Symbols: {}, (), [], ;, etc.
* Operators: ++, --, +, \*, /, etc.
* Data types: `int`, `char`, `float`, `double`, `bool`, `void`, etc.

For example, in this code snippet:

```cpp
int main() {
  for (int i=0; i<10; i++)
    cout << "Hello world" << endl;
}
```

* Identifiers: `i`
* Constants: 0, 10, `"Hello world"`
* Reserved words: `for`, `main`, `std::cout`, `std::endl`
* Symbols: `(`, `)`, `;``
* Operators: =, <, ++, <<
* Data types: `int`

Let's see in detail each of these elements.

#### Identifiers

The identifiers are the names that we give to the constants, variables or functions. The programmer chooses them, but you must follow a series of recommendations:

* Identifier names must be **relevant**, that is, their name must indicate what they are used for. The following are correct examples:

```cpp
int numberStudents = 0;
void viewStudents(...)
```

In C++, _lowerCamelCase_ notation is usually followed. This means that  the name of the variables and functions must begin with a lowercase letter, and if there is more than one word, each first letter must be uppercase as in the previous example.

These are examples of wrong names:

```cpp
const int kEIGHT=8; // A constant should not be named with its value
int p,q,r,a,b; // A unique letter is not relevant
int counter1,counter2; // better int i,j;
```

Also by agreement, counters in C++ begin with the letter _i_. Therefore, if we have several counters we should call them _i,j,k_, etc.

Obviously, there are reserved words that can not be used as user-defined names. For example, in C++ we can not name a variable as `int`, `long`, `friend`, `for`, etc.

Constant names are usually capitalized, to distinguish them from variables.

#### Constants

We can have constants of different types:

| Tipo   | Ejemplos                   |
|--------|----------------------------|
| int    | 123, 007, -4               |
| float  | 123.0, -0.4, .3, 1.23e-12  |
| char   | 'a', '1', ';', '\''        |
| char[] | "hello","","message: \""      |
| bool   | true, false                |

When the constants appear directly in the code with their value, as in the following example, they are said to be **implicit**.

```cpp
if (i<255) {
  cout << "Correct value" << endl;
}
```

However, if we declare them with a name that identifies them, they are said to be **explicit**:

```cpp
const int kMAXVALUE = 255;
const char kMESSAGE[] = "Correct value";

if (i<kMAXVALUE) {
  cout << kMESSAGE << endl;
}
```

We can declare explicit constants of any type:

```cpp
const int MAXSTUDENTS=600;
const double PI=3.141592;
const char MESSAGE[] = "BYE";
```

The question is, when should we do it? And the answer is: **almost always**, since we should declare as constants those values that we might want to change in future versions of the program. For example, we may want to change the text of a message, so we must declare it as constant.

The main advantage of using constants is that usually in the code we need to use the same value many times (for example, an error message). If we declare it as an explicit constant and we want to change its value in the future, we would only have to make this change in the declaration. If we do not declare it as explicit, we would have to look in the code for all the instructions in which this message appears to change its value, which is more uncomfortable and prone to errors.

#### Variables

Unlike constants, which can only have a fixed value assigned, variables can be used to store values that change during program execution.

It is highly recommended that whenever a variable (of simple type) is declared, an initialization value will be assigned, either in the same instruction or in the next one. For example:

```cpp
 int numProfessors=0; // Initialization in the same instruction

 int numStudents;
 numStudents = 10; // Initialization in the next instruction
```

If you do not initialize a variable of simple type, its value will be the one in memory at that moment, that is, anything (we can not control it).

> The most common simple types in C/C++ are `int`, `char`, `float`, `double`, `unsigned` and `bool`.

This code would be wrong:

```cpp
int i;
cout << i << endl; // The value of i is "random" 
```

##### Scope

All the variables (and constants) that declared in our code have a scope. The scope of a variable or constant begins when it is declared, and ends when the braces (block) that contains it ends. Therefore, we can only use the variables or constants during their scope.

```cpp
if (i<10) {
	int j = 20;
}
j = 10; // We cannot use the variable j here because it was removed when the if instruction ended
```

Loop example:

```cpp
for (int i=0; i<10; i++) {
	cout << "Hello world" << endl;
}
i = 2; // We cannot use the variable i here because it was removed
```

We can declare (although it is not convenient) two variables that have the same name within the same function, as long as they have a different scope. For example, this would compile:

```cpp
int nboxes=0;
if (i<10) {
  // we can use nboxes
  int nboxes=100; // and another variable can be declared using the same name (not recommended!)
  cout << nboxes << endl; // prints 100
}
cout << nboxes << endl; // prints 0, as this instruction is printing the first variable
```

##### Global variables

Variables are usually declared within a function, but when they are declared outside they are called global variables. In general, it is recommended not to use global variables (they are dangerous) and in Programming 2 they are strictly forbidden.

Why do we forbid these variables? Let's see an example of code in which you can see how complicated could be to manage them.

```cpp
#include <iostream>
using namespace std;
int counter=10;

void countDown() {
  while (counter > 0) {
    cout << counter << " ";
    counter--;
  }
  cout << endl;
}

int main() {
  countDown();   // Prints 10, 9, 8, ... 0
  countDown();   // Here nothing is printed
}
```

In this example, we have a global variable called `counter` that is initially set to 10. We can see that if we call the `countDown` function, the first time will do one thing and the second will do something different.

This code is very complicated to control, since **the same function with the same parameters behaves differently** depending on where we make the call in the code.

#### Data types

These are the main **data types** in C++:

| Type   | Description                   |
|--------------|----------------------------|
|`short` | Short integer |
| `int`  | Integer, twice the bytes of `short` |
| `char` | Character |
| `float`| Real |
|`double`| Real, twice the bytes of `float` |
|`bool` | Boolean|

In addition, for integers we have the unsigned versions (`unsigned short` and `unsigned int`). If a short integer (`short`) is represented with 2 bytes, its value ranges from -32768 to 32767. On the other hand, if it is an `unsigned short`, we can represent from 0 to 65535. Therefore, if we know that we can not have negative values, we could use unsigned to represent higher numbers without needing more bytes.

> The number of bytes required for each data type depends on the platform (for example, if the operating system is 32 or 64 bits).

##### Conversion

In our code we can convert a variable from one type to another. This conversion can be implicit (the compiler does it for us directly), or explicit (we have to tell it to do it). Below we can see examples of **implicit** conversions:

| Conversion   | Examples                   |
|--------------|----------------------------|
| char -> int  | int le = 'A' + 2;          |
| int -> float | float pi = 1 + 2.141592;   |
| float -> double  | double pidouble = pi; |
| bool -> int | int c = true; // c == 1   |
| int -> bool   | bool c = 77212; // b == true     |

In these previous examples there are no conversion problems, because the target variable is more general than the original variable. For example, we can always store a value `char` (1 byte) in an `int` (usually 2 bytes).

However, there are other cases in which doing the conversion we can lose information. In these situations the compiler will show a `warning`, and we will have to force the conversion in the code, doing what we know as `casting`.

> It is important not to ignore the _warnings_.

Let's see some **explicit** conversion examples:

| Conversion   | Examples                   |
|--------------|----------------------------|
| int -> char  | char c = (char)('A' + 2); // c valdrá 'C'    |
| float -> int | int epi = (int)pi;  // epi will be 3  |
| double -> float | double d = (double)pi; |

In the first case, if we convert an integer to a character we could have problems (if it is greater than 255 it would not fit into a byte). Therefore, the compiler gives us a _warning_ (sometimes even an error) so that we take this into account. To indicate explicitly that we want to convert one type to another, even if there may be problems, we have to do a _casting_, converting the data type using parentheses before the name of the variable.

##### Type declaration

In C ++, as in most programming languages, you can define new types. To start, we can assign aliases to certain types of existing data, using `typedef`:

```cpp
typedef int integer;  
integer i,j; // i,j are int

typedef bool logico,booleano;  // logico and booleano are bool
logico b;  // b is bool
```

We can also declare an array as a type:

```cpp
typedef char chararray[MAXARRAY];  // chararray is an array of char
```

In Programming 2 we do not recommend to redefine existing types, basically because a C++ developer is not used to seeing data types that are called _logico_ or _chararray_ in the code. If you see a code full of data types that you do not know, you should go to the definition of the types to know what they mean in each case, and this is not comfortable when sharing and reusing code.

In addition to redefining existing types, C++ allows us to create new types of data. In Programming 2, the only new data types that we will see will be the structs (in the last topic we will learn how to make classes, but actually they are not data types). We can declare a struct in two ways:


<!--- CAMBIADO TRASPAS --->

```cpp
 struct Student {      // First way: 'Student' is a C++ type (recommended)
   int id;
   double mark;
 };
```

```cpp
 typedef struct {     // Second way: 'Student' is a type in C (and C++)
   int id;
   double mark;
 } Student;
```

In both cases we declare a type of data called `Student`. Then, we can create a variable of this type as follows:

```cpp
Student alu;
```

The main advantage of using structs is that they allow us to group variables. Let's consider the following function:


```cpp
int manageStudent(int id, double mark, char name[], int group, int absences);
```

If we used a `Student` struct to group all this data, the call would be simpler:

```cpp
int manageStudent(Student alu);
```

In addition, we could create arrays:

```cpp
Student alus[100];
```

To access the data of a record we can use a dot (`.`) after its name. For example:

```cpp
Student a,b;

a.id = 123133;  // field assignment
b = a;          // struct assignment
```

The direct assignment of structs (as in the last line of the previous example) is possible, but we should not do it if we have an array or a pointer inside the struct. The reason is that we must be very careful when directly assigning a struct or a pointer for the reasons that we will see in the dynamic memory unit.

#### Expressions

Expressions are used when we want to perform calculations and evaluate one or several operations returning a result. Inside an expression we have operators and operands, as can be seen in the following example:

```cpp
int i = 10 + 12; // Expression with operators 10 and 12, and operand +.
```

The expressions can be of several types.

##### Arithmetic expressions

In C ++ the main arithmetic operators are sum (`+`), subtraction (`-`),  multiplication (`*`), division (`/`), and module (`\%` ).

If we have an operand of type `char` or `bool` inside an arithmetic expression, it is implicitly converted to `int`. For example:

```cpp
int i = 'a' + 3; // i == 100
char c = 'A' + 3; // c == D
```

A division between two integers (`int`) returns an integer number. For example:

```cpp
float f1 = 7 / 2; // f1 == 3
float f2 =  (float)7 / 2; // f2 == 3.5
```

As soon as one of the two operands is `float` (or `double`), the result of the division is a real number, as can be seen in the previous example.

The module operator (`%`) returns the remainder of a division between two integers. For example:

```cpp
int remainder = 30 % 7; // remainder == 2
```

In arithmetic expressions there are operators that have preference over others. In particular, the `*` and `/` operators are always evaluated before the `+` and `-` operators.

```cpp
int n = 2+3*2;  // First, 2+6 is computed, as multiplication operation has preference
```

If there are several operations in an expression, it is recommended to use parentheses to avoid problems. For example:

```cpp
int n = 2+(3*(7/2.5));
```

##### Increment and decrement operators

Operators `++` and `--` are used to increase or decrease the value of an integer variable. We can use them before or after the variable: 

```cpp
++i;  // Preincrement
i++;  // Postincrement
```

The difference between setting them before or after is important, since it does not mean the same thing. If they appear alone in a line as in the previous example, the result is identical. However, when used with other instructions, their behaviour changes:

```cpp
int i = 3;
int k = ++i;  // k == 4, i == 4

i=3;
int j = i++;  // j == 3, i == 4
```

As you can see, with preincrement (or predecrement) the sum is performed **before** calculating the rest of the expression. On the other hand, postincrement (or postdecrement) are calculated **at the end** of the instruction. In the first case, the increment is calculated first (`++i`), and then this value is assigned to `k`. However, in the second case the assignment is made (`j = i`) and finally the value of `i` is increased.

Although they can be used at any point of an expression, it is advisable not to mix these operators in the same instruction, since it is very complicated to predict their result. For example, what would be the value of `j` in this case?:

```cpp
i = 3;
j = i++ + --i;  // value of j?
```

> Think of the answer, and then confirm the result by executing this code.

##### Relational expressions

Relational expressions evaluate an operation, returning just `true` or `false`. The main relational operators in C ++ are `==`, `!=`, `>=`, `>`, `<=`, `<`.

If the operands are of different types, they are implicitly converted to the more general type. For example:

```cpp
bool result = 2 < 3.4; // Internally the operation is 2.0 < 3.4
```

In C++, operands are grouped two by two. Therefore, to know if `a<b<c`, we have to indicate the following:

```cpp
if (a < b && b < c) {
	// relation is true
}
```

##### Logic expressions

Logic expressions evaluate an expression of logical type, returning either `true` or `false`. The main logical operators in C++ are `!`, `&&`, `||`. Denial (`!`) returns `true` if what follows afterwards is `false`, or `false` if what follows is `true`. For example:

```cpp
int i = 2;
bool end = false;

if (i == 2 && !end) {
	// Condition is true, this line is executed
}
```

There is a C++ feature that is important to know: The **short-circuit evaluation**. When we have a condition `||`, if the left operand is `true`, then the right operand will never be evaluated, as `true||x` will always be `true`. For example:

```cpp
int i = 2;
bool salir = false;

if (i == 2 || !end) {
	// Variable end is not checked, as i==2
}
```

Also, when we have a condition `&&`, if the left operand is `false` then the right operand will not be evaluated, since `false && x` will always be `false`. For example:

```cpp
char v[] = "Hello world";
const char search = 'k';

for (int i = 0; i<strlen(v) && v[i]!=search; i++) {
 	cout << v[i];   // The loop prints "Hello world"
}
```

This code snippet prints the contents of an array until the letter `'k'` is found.  Look at the condition `&&`. When `i==strlen(v)`, this expression will be `false` and therefore `v[i]!=search` will not be checked. If short-circuit evaluation did not exist and this second condition would be checked, a segmentation fault (_segmentation fault_) would occur when trying to look at a position in the array that is beyond its size. If we implemented the condition backwards, `v[i]!=search && i<strlen(v)`, we would get this error. Therefore, **the order of the operands in a logic expression is important**.

#### Input / output

In C ++ we have the input and output _streams_ called _cin_ and _cout_, respectively. We can show a variable on the screen as follows:

```cpp
int i = 3;
cout << i << endl;
```

As can be seen, the "arrows" point in the direction of the operation (the data is sent to `cout`). By default, everything that is sent to the standard output (`cout`) is printed on the screen. However, we can change this behavior. Write this program, call it `test.cc` and compile it:


```cpp
#include <iostream>

using namespace std;

int main() {
	cout << "Hello world" << endl;
}
```

Run it from the terminal: 

```bash
$ ./test
```

You will see that the message is printed on the screen. Now let's execute it in another way:

```bash
$ ./test > output.txt
```

Instead of being displayed on the screen, the output has been saved in the `output.txt` file, which you can open with any text editor. This is called **redirection** of the standard output.

Just as we can show something, we can also read the value of a variable from the keyboard:

```cpp
#include <iostream>

using namespace std;

int main() {
	int n;
	cin >> n; // An integer is read from the keyboard
	cout << "I read " << n << endl;
}
```

In this case, we read from the **standard input** with `cin`. The data to be read will depend on the type of the variable. In this example it is an integer, but it could be a character (only one letter would be read), or a string, for example. The input operator reads from the keyboard ignoring blanks and tabs until reading the data type of the variable that is indicated, and leaves the reading pointer just after.

As with `cout`, we can read several variables in a single instruction:

```cpp
#include <iostream>

using namespace std;

int main() {
	int n;
	char c;
	cin >> n >> c; // We read an integer value and a character
	cout << "I read " << n << " and " << c << endl;
}
```

When we run this program, it will wait for us to enter an integer number and then a character.

> Every time we read something with `cin>>`, it is convenient to write `cin.get();`, `cin.ignore();` or call a function `cleanBuffer()` so that the line break character will be removed from the _buffer_, since it can give problems if a string is read afterwards as we will see in the `strings` unit.

We will execute the previous program redirecting the standard input. With any text editor we create a new file called `input.txt`, and write the following:

```bash
124 k
```

Do not forget to put a line break at the end of the file. Then, we execute:

```bash
$ ./test < input.txt
```

You will see how the program no longer asks for the input data, but reads it from the file. We can also use the input and output redirection simultaneously:

```bash
$ ./test < input.txt > output.txt
```

The program will read the standard input from the file `input.txt`, writing the result in `output.txt`.

There is a third _stream_, the error output (`cerr`). Change the previous program, replacing `cout` with `cerr`. As you can see, if we run the program as we did before, the information will be displayed on the screen and `output.txt` will be empty. This occurs because what goes through the error output is redirected differently from what is shown by the standard output. To redirect the standard error, we have to write:


```bash
$ ./test < input.txt 2> errors.txt
```

Usually, the error output is used to display error messages from our programs. For example, add anything to the previous code so it does not compile. Execute the compiler:

```bash
g++ -o test test.cc 2> errors.txt
```

You will see that the compilation errors were saved in `errors.txt`, since the `g++` program shows all error messages using `cerr`.

#### Flow control

The code flow control allows us to add conditions to the code, so that our program executes only certain instructions based on a given condition.

##### `if`

The `if` statement executes what is next as long as the condition is `true`. We can also add an `else` so that alternative instructions are executed when the condition is not met:

```cpp
if (condition) {
	// Instructions
}
else {
	// Other instructions
}
```

##### `while`

The flow control instruction `while` creates a loop that ends when a condition is no longer met.

```cpp
while (condition) {
	// Instructions
}
```

It is not recommended to use `||` inside a `while` condition, since it is very complicated to control and unintuitive. For example, try to guess when would the loop end in the following code:

```cpp
bool found=false;
int i=0;
while (i<10 || !found) {
	cout << i << endl;
   i++;
}
```

Answer: Never, as `i<10` must be met, **and also** `found` should be `true`.

> Note: To stop a program from an infinite loop, press `Ctrl+C`.

##### `for`

The `for` conditions are loops with an initialization and an instruction that is executed after each iteration:

```cpp
for (initialization; condition; ending) {
	// Instructions
}
```

The `for` loops are usually necessary when we iterate through a series of elements, for example in an array:

```cpp
bool end = false;
int array[] = {1,3,5,2,5,6,1,2};

for (int i=0; i<8 && !end; i++) {
	cout << i << endl;
	if (array[i] == 6)
		end=true;
}
```

Actually, a `for` is equivalent to a `while` condition but saving some code:

```cpp
initialization;
while (condition) {
   // Instructions
   ending;
}
```

##### `do-while`

The `do-while` control instructions are similar to the `while` instructions, with the difference that the first time **they always** execute their content.

```cpp
do {
  // Instructions
} while (condition);
```

They are used, for example, when we want to show a menu on the screen.

##### `switch`

The `switch` instructions are used when we want to make an action for a variable that could have certain values. For example:

```cpp
switch (variable) {
  case value1:
           // Instructions 1
           break;
  case value2:
           // Instructions 2
           break;
  default:
           // Instructions 3
           break;
}
```

In C++ the variable (or constant) used in `switch` must be an integer or a char (which is implicitly converted into an int). For example:

```cpp
char c;
cin >> c;
switch(c) {
	case 'a':
		cout << "Selected a" << endl;
		break;
	case 'b':
	case 'c':
		cout << "Selected b or c" << endl;
		break;
	default:
		cout << "Unknown value" << endl;
		break;
}
```

If the variable is of another type such as `float` or `double`, the program will not compile.

> As you can see, we have to use `break` in the `switch`. You can also use `break` to exit a `while`, `do` or `for` loop.

#### Arrays and matrices

An array is a container that allows to store a sequence of variables or constants. In C/C++, an array must be declared using a **fixed size** that can not be changed in runtime.

We can use a constant to indicate the size:

```cpp
int studentsArray[10];
char row[kMAXBOARD];
```

We can also create a array by initializing it with a series of elements. In this case it is not necessary to specify the size:

```cpp
int numbers[] = {1,3,5,2,5,6,1,2};
```

In C ++ we can create an array with a size assigned by a variable:

```cpp
int n;
cin >> n;
int v[n]; // The array size is known in runtime. NOT RECOMMENDED!
```

However, the language standard does not recommend doing so. If the size of the array is not known at compile time, then it is better to use vectors, as we will see below.

We can use square brackets (`[]`) to access the values of an array:

```cpp
int numbers[] = {1,3,5,2,5,6,1,2};
cout << numbers[2] << endl; // Prints 5
```

The index of an array starts at position 0. Also, we can never exceed the number of elements of an array, since is most likely to throw a segmentation fault:

```cpp
int numbers[] = {1,3,5,2,5,6,1,2};

numbers[20] = 12; // Error, the array has less than 20 elements
numbers[8] = 3; // Error, the array has 8 elements which positions range from 0 to 7

for (int i=0; i<8; i++) {
	numbers[i] = 4; // Correct
}
```

It is important to always check the limits of an array to avoid accessing outside them.

A matrix is an array with more than one dimension:

```cpp
int matrix[10][10];

matrix[0][2]=31; // Value assignment
```

#### Arrays of characters in C

Character arrays in C are simply arrays that contain characters. For example:

```cpp
const char myarray[]="Hello";
```

To represent a character string we use double quotes (`"`), whereas to represent a single character we use single quotes (`'`).

The peculiarity is that C arrays add the null character at the end: `'\0'`.


| H | e | l | l | o |\0 |
|:---|:---|:---|:---|:---|:---|
| <sub>0</sub> | <sub>1</sub> | <sub>2</sub> | <sub>3</sub> | <sub>4</sub> | <sub>5</sub> |

**It is mandatory that all C character arrays end with the null character** so that the functions that work on them (such as `strlen`, `strcpy`, etc.) could be executed correctly.

Like any other array, if we declare it without initialization we must specify its size:

```cpp
const int tARRAY = 10;
char charArray[tARRAY];
```

#### Vectors

As we have seen previously, once we have declared an array we cannot change its size. If we wanted to use arrays of variable size, then we should use C++ **vectors** instead.

A <a href=http://www.cplusplus.com/reference/vector/vector/>vector</a> is an array with efficient access to elements and with the ability to  be resized automatically when elements are added or deleted. These vectors initially belonged to the _STL (Standard Template Library)_ library, which implements a series of dynamic containers, algorithms and iterators.

Let's see an example to use them:

```cpp
vector<int> v; // Declare an integer vector
vector<int> v2(3); // Declare an integer vector with 3 elements
v.resize(4); // Dinamically change its size

v.push_back(12); // Add a value at the end of the vector

// Element access
for (unsigned int i=0; i<v.size(); i++) {
      v[i]=23; // Assignment
}
```

As you can see in this code snippet, we can declare a vector only indicating its type, without specifying its size:

```cpp
vector<int> v;
```

Optionally, we can also indicate an initial size:

```cpp
vector<int> v2(3);
```

We can add an element to the end of a vector with `push_back()`, and get the effective size of a vector with `size()`.

These are just some of the functions of the vector class, whose reference can be found <a href=http://www.cplusplus.com/reference/vector/vector/> here</a>. In addition to these, vectors have functions to sort their elements following the criteria that we choose, to erase an element and be resized automatically, etc.

In Programming 2 we will frequencly use vectors.

#### Enumerated types

Enumerated types can be used when we have a certain range of possible values for a variable (or a constant). These possible values are called enumerators, and the variables of enumerated types can take any value from these enumerators, as can be seen in the following example:

```cpp
enum colors_e {black, blue, green, red}; // Type definition

colors_e mycolor; // Variable declaration

mycolor = blue; // Enumerator assignment

if (mycolor == green) { // Comparison with an enumerator
	mycolor = red;
}
if (mycolor == 0) {  // Comparison with an integer (internally, black)
	cout << "Black" << endl;
}
```

Internally, enumerators are implicitly converted to integers and vice versa, as can be seen in the previous example.

#### Functions

A function is a set of code lines that perform a task and, optionally, can return a value. They can also receive (optionally) a series of parameters, by value or by reference.


```cpp
// Function that receives two parameters by value and one by reference, and returns an integer number

int function(int a, int b, int &c) {
	int ret = 0; // We declare a variable of the type of return

	// Instruction 1
	// Instruction 2
	// ...

	return ret; // Return the result
}
```

As can be seen in the previous example, when a function returns a value the variable is usually declared at the beginning and a single return is made at the end.

A function should not have much code. Typically, the function code should "fit" on the screen when viewed with an editor (about 50 lines maximum). If the function is longer, it is advisable to divide it into several shorter functions.

Sometimes we do not know when to create a new function. There are several cases in which it is necessary, but there is a simple rule: **if you have to copy/paste code then you need a function**. When we copy a piece of code to paste it somewhere else, it's because we're using it twice. When this happens, it is better to create a function for this piece, so that the code is more compact and also easier to maintain (if we want to make a change in the code of the function, we will only have to do it in one place, instead of several times if we did not create it).

> Important: It is highly recommended to compile and test the functions separately, instead of waiting to finish the program to start compiling and testing. Every time we have a new function, we have to check that it works correctly with any number of parameters, and when it is tested, we can go on writing the next one.

The C/C++ compiler reads the code from the beginning to the end, so if we call a function that is declared later than the call we will get a compilation error. To avoid this, you can move the function before your call, or indicate your declaration (also called header or prototype) before. This is an example of the second type (header declaration):

```cpp
// Prototype / header / function declaration
int function(bool, char, double []);

char anotherFunction() {
   double vr[MAXMARKS];
   a = function(true,'a',vr);  // Function call
}

// Body / implementation of the function
int function(bool eat, char option, double vectorMarks[]) {
  // Instructions
}
```

In Programming 2, as most C++ developers do, when we have all the code in a single source file we recommend moving the function code before  instead of indicating its header:

```cpp
// Body / function implementation
int function(bool eat, char option, double vectorMarks[]) {
  // Instructions
}

char otherFunction() {
   double vr[MAXMARKS];
   a = function(true,'a',vr);  // function call
}
```

This second form speeds up the writing of the code, since if we wanted  to change the parameters of a function we should only have to do it in the body, instead of in two places (body and declaration).

In C++, functions accept parameters passed by value or by reference (with `&`). Internally, when a function receives a parameter by value, it is **copied** into a local variable that is destroyed when the function ends. This way, changes made to the values ​​of this variable will have no effect on the output of the function. When a parameter is passed by **reference** a copy is not made, so changes made during the function will affect this variable.

The problem arises when we want to pass by value a very large variable (for example, a string of 1 million characters). If the compiler makes a copy, the performance of the program will be seriously affected (in addition, we will need much more memory). To avoid this, in C++ you can pass a parameter by reference with `const`, as in the following example:

```cpp
void function(const string &s)  {
	// The compiler does not make a copy of s, but if we try to modify this variable a compilation error will be given
}
```

What we say to the compiler with this statement is: "Do not make a copy of the variable, but I promise not to modify it within the function". In fact, if we try to modify it, we will get a compilation error.

> For pedagogical reasons, in Programming 2 parameters must not be passed by reference when it is not necessary to do so, except if it is with `const`.

In C/C++ there is a particular situation: The arrays and matrices are **always** passed by reference. The explanation will be given in the dynamic memory unit, but basically this happens because internally they are pointers and in reality what we pass by value or reference is the pointer itself.


```cpp
int sumVM(int v[],int m[][MAXCOL]) {  // the size of the first dimension is not indicated
  // Instructions
}

sumVM(vector,matrix);  // call, without brackets  
```

As you can see, in C++ it is not necessary to indicate the size of the first dimension, but if there are more dimensions it is necessary to put them.

There is also a special function in C/C++, called `main`. The `main` function is the first one invoked when a program starts. You can receive parameters (we'll see how to do it in the parameter passing section), and it returns a value (although it can be ommitted, it usually returns 0). For example:


```cpp
int main() {
  // Instructions

  return 0; // Optional
}
```

How should a `main` function be? Ideally, by looking at this function you should understand the basic program flow. An example of a proper `main` function:


```cpp
int main() {
	int n;
	read(n);

	if (n<0)
		cout << "Error, the number cannot be negative";
	else process(n);
}
```

As you can see, there is little code and it is understood what the program does (approximately). Sometimes it is not so simple when the program is very long, but the idea is this. What **never** should be done is to **do all the code inside the main**, or leave a `main` which contains a single call to a function that implements everything:

```cpp
int main() {
	function();  // Wrong
}
```

### Program arguments

The `main` function can also receive parameters. Doing this, we are passing arguments to our program, and this serves for execution in batches (in a non-interactive way). An example of argument passing by the `ls` program:

```bash
ls -l -a
```

In this case, we passed two parameters: `-l` and `-a`. With this information, the `ls` program will display the data in a certain way. Let's consider that `ls` did not admit parameters. Every time we run it, it would ask us if we want to show the hidden files, if we want to show the information by lines, etc., which would be very tedious for the user.

Let's see an example of a program that allows parameters in the `main` function:

```cpp
int main(int argc, char *argv[]) {
	// At this point, argc contains the number of parameters, and argv their value.
	for (unsigned i=0; i<argc; i++) {
		cout << "Argv[" << i << "]=" << argv[i] << endl;
	}
}
```

If we execute the program in this way:

```bash
./program one two three
```

It will print:

```bash
Argv[0]=./program
Argv[1]=one
Argv[2]=two
Argv[3]=three
```

Therefore, the variable `argc` contains the number of parameters that the user has entered, and `argv` is an array of arrays of characters containing in each position the value entered by the user.

At first it seems simple, but it gets more complicated when we have many parameters and can be given in any order. For example, `g++` allows many parameters and the order can be important:

```bash
g++ -Wall -o program program.cc -g
```

It is somewhat complicated to manage all this variability. For example, if we wanted to make a program that accepts three parameters ("one", "two" and "three"), we could use a loop like this:

```cpp
int main(int argc, char *argv[]) {

	  if (argc>4) {
			cout << "Syntax: " << argv[0] << " [one | two | three]" << endl;
			return(-1);
		}

		for (unsigned i=1; i<argc; i++) {
			string arg = argv[i];

			if (arg=="one")  {
				// Do something with the argument one
			}
			else if (arg=="two") {
				// Do something with the argument two
			}
			else if (arg=="tres") {
				// Do something with the argument three
			}
			else {
				cout << "Syntax: " << argv[0] << " [one | two | three]" << endl;
				return(-1);
			}
		}
}
```

Sometimes it can be so complicated that it is convenient to use a separate function to manage the parameters.

#### Exercise (arguments)

We are going to make a program to print on the screen the first _n_ prime numbers. By default (if no arguments are given), the program should print the first 10 prime numbers separated by blank spaces. Example:

```bash
./primes
1 2 3 5 7 11 13 17 19 23
```

The user must also be able to indicate the `-L` and/or `-N n` options.

The `-L` option is to display each number on a different line, and the `-N` option followed by a number is to show the first `n` primes. For example:

```bash
./primes -N 3
1 2 3
./primes -L -N 2
1
2
```

If the parameters indicated by the user are not correct, a syntax error message must be displayed and the program should end. The complicated part of this exercise is checking that all parameters are correct.

### Typical structure of a C ++ program

For review, let's see the typical structure of a C++ program:

```cpp
#include <standard header files>
...
#include "own header files"
...
using namespace std; // this allows to use bool (and string)
...
const ... // Constant declaration (if they are not in the header file)
...
typedef ... // types not previously defined
...
// declaration of global variables FORBIDDEN in P2!!!
...
// functions
...
int main() {
...
}
```

### Compilation

The C ++ compiler that we will use in this subject is GNU GCC, which is installed by default in Linux. We can call the compiler from a terminal by typing `g++`. The C ++ compiler supports many parameters, let's see the most important ones:

* `-Wall` : Shows all warnings, instead of only those that are more relevant (**recommended in P2**)
* `-g` : Compiles in a mode which allows to easily find errors using a debugger (**recommended in P2**).
* `-o`: Sets the name of the executable file given after this option.
* `--version`: Shows the current version of the compiler.
* `-std=c++0x`: Uses the new C++ standard which allows using weakly typed variables with `auto`, `lambda` functions, `for_each` loops, and concurrence functions, among others. We are not using these functionalities in P2.

The recommended way in Programming 2 to compile a program is as follows:

```bash
g++ -Wall -g program.cc -o program
```

It is very important that just after `-o` there will be the name of the executable, **not** the source code file. If we write `-o program.cc`, we will erase all our source code (it is usual to lose it in the first courses, so be careful).

### Debugging

Sometimes the program compiles but it fails during execution. In some situations it is very complicated to find which line of the code has caused the execution error. Fortunately, there are **debuggers**, tools that make this task much easier.

> They are called _debuggers_ because they find _bugs_, which is how code errors are popularly known. This term comes because in 1946, a moth was accidentally introduced into a circuit causing errors in its program. When they found the problem, they hit it with adhesive tape in the corresponding error report and wrote: _First current case of bug being found_.

![Bug](bug.jpg "Bug")

Here are some widely used debuggers:

* <a href="http://valgrind.org">Valgrind</a>. It is the debugger that we will use in the "corrector" of the assignments. It detects memory errors (access to components outside a vector, variables used without initialization, pointers that do not point to a reserved area of memory, etc.).
* <a href="https://www.gnu.org/software/gdb/">GDB</a>. This debugger starts our program, stops it when we ask for it and looks at the content of the variables. If our executable gives a segmentation fault, it tells us the line of code where the problem can be found.
* More examples in Linux: <a href="https://www.gnu.org/software/ddd/">DDD</a>, <a href="https://wiki.gnome.org/Apps/Nemiver">Nevimer</a>, <a href="http://elinux.org/Electric_Fence">Electric Fence</a>, <a href="http://duma.sourceforge.net">DUMA</a>, etc.

Almost all these debuggers can be installed from the Linux package installer (better than installing them from their website).

### Remainder

For pedagogical reasons, in Program 2 it is **strictly forbidden to use global variables**.

### References

To learn more about C ++ we recommend checking the following references:

<a href="http://www.cplusplus.com">http://www.cplusplus.com</a>

<a href="http://www.minidosis.org">http://www.minidosis.org</a>
