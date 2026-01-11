# C-_CCEE
## Varible Initialization
```
#include <iostream>
#include <vector>
using namespace std;

int main() {
    // Primitive types
    int a = 10;          // Copy
    int b(20);           // Direct
    int c{30};           // Uniform ✅
    
    // Strings
    string s1 = "Hello";
    string s2("World");
    string s3{"C++"};    // ✅
    
    // Arrays
    int arr1[] = {1, 2, 3};
    int arr2[3] = {1, 2, 3};
    int arr3[5] = {1, 2};  // {1, 2, 0, 0, 0}
    int arr4[3]{1, 2, 3};  // Uniform ✅
    
    // Vectors
    vector<int> v1 = {1, 2, 3};
    vector<int> v2{1, 2, 3};     // ✅
    vector<int> v3(5, 10);       // {10, 10, 10, 10, 10}
    
    // Pointers
    int* ptr1 = nullptr;  // C++11 ✅
    int* ptr2 = NULL;     // Old way
    int* ptr3 = 0;        // Old way
    
    return 0;
}
```
## Static
1. ClassName::member -- assign a value
2. alse class k bahar assing hoga static varible

## Constant
```
const int MAX = 100;   // Cannot change
// MAX = 200;          ❌ Error

const double PI = 3.14159;

// Must initialize
const int x;           ❌ Error - must initialize
const int y = 10;      ✅ OK
**Memory Trick:**

Read from right to left:
const int* ptr       → ptr is pointer to const int
int* const ptr       → ptr is const pointer to int
const int* const ptr → ptr is const pointer to const int
```
## Revison
```
int result = 5 + 3 * 2;
// * has higher precedence than +
// result = 5 + 6 = 11 (not 16)

int x = 10 > 5 && 3 < 7;
// Relational first, then logical
// x = true && true = 1 (true)
```

---

# 🎯 Quick Revision Points

## Program Structure:
```
#include → namespace → prototypes → main() → definitions
```

## C++17 Features:
```
Structured bindings, if initializer, optional, string_view
```

## Tokens:
```
Keywords, Identifiers, Literals, Operators, Punctuators
```

## Initialization:
```
int a = 10;   // Copy
int b(10);    // Direct
int c{10};    // Uniform ✅ (prevents narrowing)
```

## Static:
```
static int count;              // Declaration
int ClassName::count = 0;      // Definition (outside)
ClassName::staticMethod();     // Call without object
```

## const:
```
const int x = 10;             // Constant variable
void func() const { }         // Const method (read-only)
const int* ptr;               // Pointer to const
int* const ptr;               // Const pointer
```

## Operators:
```
++x  → Pre-increment (increment then use)
x++  → Post-increment (use then increment)
a?b:c → Ternary (if a then b else c)
```
## Function
```
// Rules:
// - Default parameters must be at the end
void func(int a, int b = 10) { }  // ✅ OK
void func(int a = 5, int b) { }   // ❌ Error
```
```
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 10, y = 20;
    swap(&x, &y);
    cout << x << " " << y;  // 20 10 ✅
    return 0;
}
```
## new / delete
```
// ✅ Correct usage
int *ptr = new int;
delete ptr;

int *arr = new int[10];
delete[] arr;  // Note: [] for arrays

// ❌ Memory leak
int *ptr2 = new int;
// Forgot delete! Memory leak ⚠️

// ❌ Undefined behavior
int *ptr3 = new int[5];
delete ptr3;  // Wrong! Should be delete[]
```
## Class Pointer & this pointer
```
class Student {
public:
    string name;
    int marks;
    
    void display() {
        cout << name << ": " << marks << endl;
    }
};

int main() {
    // Stack object
    Student s1;
    s1.name = "Rahul";
    s1.display();  // Use . (dot)
    
    // Heap object (pointer)
    Student *s2 = new Student;
    s2->name = "Priya";  // Use -> (arrow)  // or = //(*s2).name
    s2->display();
    
    delete s2;  // Free memory
    
    return 0;
}
```
## malloc/ free
```
malloc → C function, no constructor ❌
#include <cstdlib>

// Single object
int *p1 = (int*)malloc(sizeof(int));  // Cast needed
*p1 = 10;
free(p1);

// Array
int *arr = (int*)malloc(5 * sizeof(int));
free(arr);

// Class object
Student *s = (Student*)malloc(sizeof(Student));
// ❌ Constructor NOT called!
free(s);
// ❌ Destructor NOT called
=======================================================
// Allocates and initializes to 0
int *arr = (int*)calloc(5, sizeof(int));
// arr = {0, 0, 0, 0, 0}
free(arr);
=====================================================
int *arr = (int*)malloc(5 * sizeof(int));

// Resize to 10 elements
arr = (int*)realloc(arr, 10 * sizeof(int));

free(arr);
```
```
class Student {
    string name;
public:
    Student() {
        cout << "Constructor called\n";
    }
    ~Student() {
        cout << "Destructor called\n";
    }
};

// Using new (Correct ✅)
Student *s1 = new Student();  
// Output: Constructor called
delete s1;
// Output: Destructor called

// Using malloc (Wrong ❌)
Student *s2 = (Student*)malloc(sizeof(Student));
// ❌ No constructor called!
free(s2);
// ❌ No destructor called!
```
## Run time polymorphism
C++: virtual keyword zaroori for runtime polymorphism
```
class Parent {
public:
    virtual void show() { // 'virtual' keyword zaroori
        cout << "Parent";
    }
};

class Child : public Parent {
public:
    void show() override { // override (optional C++11+)
        cout << "Child";
    }
};

Parent* p = new Child();
p->show(); // Output: "Child" (runtime polymorphism)
```
## virtual function
```
class Base {
public:
    virtual void display() { } // Virtual function
    void show() { }            // Non-virtual
};

Base* b = new Derived();
b->display(); // Runtime polymorphism ✅
b->show();    // Compile time (Base version) ❌ // to show() likho to chal jaye ga
```
## pure virtual function
```
class Shape {
public:
    virtual void draw() = 0; // Pure virtual (= 0)
    // Class ab abstract ban gaya
};

class Circle : public Shape {
public:
    void draw() { // Implementation zaroori
        cout << "Circle";
    }
};

// Shape s; // ❌ ERROR - cannot instantiate
Circle c;   // ✅ OK
```
## Interface
```
// Pure abstract class = interface
class Printable {
public:
    virtual void print() = 0; // All pure virtual
    virtual void show() = 0;
};

class Document : public Printable {
public:
    void print() { }
    void show() { }
};
```
```
ios (base)
  ├── istream (input)
  │     ├── cin
  │     └── ifstream (file input)
  └── ostream (output)
        ├── cout, cerr, clog
        └── ofstream (file output)
```
```
#include <iostream>
#include <iomanip>
using namespace std;

int x = 123;
double pi = 3.14159;

// Width
cout << setw(10) << x;        // "       123" (right-aligned)

// Precision
cout << setprecision(2) << pi; // 3.1

// Fill character
cout << setfill('*') << setw(10) << x; // "*******123"

// Alignment
cout << left << setw(10) << x;  // "123       "
cout << right << setw(10) << x; // "       123"

// Number base
cout << hex << 255; // ff (hexadecimal)
cout << oct << 8;   // 10 (octal)
cout << dec << 10;  // 10 (decimal - default)
```
## File
```
#include <fstream>
ifstream  // Input file stream (read)
ofstream  // Output file stream (write)
fstream   // Input + Output (read/write)
ios::in | ios::out - in(input)/
close(); // Close file
```
```
#include <iostream>
using namespace std;
class Parent {
public:
    void display() { cout << "P "; }
};
class Child : public Parent {
public:
    void display() { cout << "C "; }
};
int main() {
    Parent *obj = new Child();
    obj->display();
    return 0;
}
// p -
| Case                 | Output | Reason               |
| -------------------- | ------ | -------------------- |
| Non-virtual function | `P`    | Compile-time binding |
| Virtual function     | `C`    | Runtime polymorphism |

```
## MCQ
```
1. Which of the following type of class allows only one object of it to be created?

A. Virtual class
B. Abstract class
C. Singleton class ✅
D. Friend class

2. Which of the following is not a type of constructor?

A. Copy constructor
B. Friend constructor ✅
C. Default constructor
D. Parameterized constructor

3. Which of the following statements is correct?

A. Base class pointer cannot point to derived class
B. Derived class pointer cannot point to base class ✅
C. Pointer to derived class cannot be created
D. Pointer to base class cannot be created

4. Which of the following is not the member of class?

A. Static function
B. Friend function ✅
C. Const function
D. Virtual function

5. Determining at runtime which method to invoke is called?

A. Data hiding
B. Dynamic typing
C. Dynamic binding ✅
D. Dynamic loading

6. Function defined inside a class is called?

A. Member variable
B. Member function ✅
C. Class function
D. Classic function

7. Compiler inserts arguments if not specified →

A. Call by value
B. Call by reference
C. Default arguments ✅
D. Call by pointer

8. How many instances of abstract class can be created?

A. 1
B. 5
C. 13
D. 0 ✅

9. Which of the following cannot be friend?

A. Function
B. Class
C. Object ✅
D. Operator function

10. Exposing only necessary information to client →

A. Encapsulation
B. Abstraction
C. Data hiding ✅
D. Data binding

11. Why reference is not same as pointer?

A. Reference can never be null
B. Reference once assigned cannot change
C. No dereferencing needed
D. All of the above ✅

12. cout is a/an ___

A. Operator
B. Function
C. Object ✅
D. Macro

13. Object of one class inside another class →

A. Encapsulation
B. Abstraction
C. Composition ✅
D. Inheritance

14. Types of polymorphism in C++

A. 1
B. 2 ✅
C. 3
D. 4

15. Abstract data type →

A. int
B. double
C. string
D. Class ✅

16. Adding components at runtime →

A. Data hiding
B. Dynamic typing
C. Dynamic binding
D. Dynamic loading ✅

17. Constructor is called when →

A. Object is declared ✅
B. Object is used
C. Class is declared
D. Class is used

18. Function overloading is →

A. Virtual polymorphism
B. Transient polymorphism
C. Ad-hoc polymorphism ✅
D. Pseudo polymorphism

19. C++ follows which approach?

A. Top-down
B. Bottom-up ✅
C. Right-left
D. Left-right

20. Correct about function overloading →

A. Types of arguments differ
B. Order of arguments differ
C. Number same
D. Both A and B ✅

21. Correct about class & structure →

A. Structure can’t have functions
B. Class public, structure private
C. Pointer cannot be declared
D. Class private by default, structure public by default ✅

22. Wrapping data & functions together →

A. Abstraction
B. Encapsulation ✅
C. Inheritance
D. Polymorphism

23. Waiting till runtime to decide function call →

A. Data hiding
B. Dynamic casting
C. Dynamic binding ✅
D. Dynamic loading

24. Late binding implemented using →

A. C++ tables
B. Virtual tables ✅
C. Indexed virtual tables
D. Polymorphic tables

25. Operator overloaded for cout →

A. >>
B. << ✅
C. +
D. =

26. Correct class of cout →

A. iostream
B. istream
C. ostream ✅
D. ifstream

27. Cannot be used with virtual →

A. class
B. member functions
C. constructor ✅
D. destructor

28. Constructor performs →

A. Construct class
B. Construct object
C. Construct function
D. Initialize objects ✅

29. Which causes exception?

A. Missing semicolon
B. Function call issue
C. Syntax error
D. Run-time error ✅

30. Compiler checks type of reference →

A. Inheritance
B. Polymorphism ✅
C. Abstraction
D. Encapsulation

31. Correct const member function syntax →

A. const int ShowData()
B. int const ShowData()
C. int ShowData() const ✅
D. Both A and B

32. Late binding uses →

A. Virtual function ✅
B. Operator function
C. Const function
D. Static function

33. Correct statement →

A. Static type checking
B. Dynamic type checking
C. Static member const
D. Both A and B ✅

34. Reusability desirable because →

A. Less testing
B. Less maintenance
C. Less compilation
D. Both A and B ✅

35. Correct this usage →

A. this->x ✅
B. this.x
C. *this.x
D. *this-x

36. Static polymorphism mechanism →

A. Operator overloading
B. Function overloading
C. Templates
D. All of the above ✅

37. Operator overloading statements →

All operators overloadable ❌

Meaning can be changed ❌
Answer → B) Both false ✅

38. Same function in base & derived →

A. Compile error
B. Base always
C. Derived always
D. Object decides ✅

39. Available only in class hierarchy →

A. Public
B. Private
C. Protected ✅
D. Member functions

40. Not a type of inheritance →

A. Multiple
B. Multilevel
C. Distributive ✅
D. Hierarchical

41. Cannot be overloaded →

A. []
B. ->
C. ?: ✅
D. *

42. Virtual call resolved at compile time →

A. Destructor
B. Constructor
C. main
D. Both A and B ✅

43. Inline function →

A. Speeds execution
B. Slows execution
C. Increases code size
D. Both A and C ✅

44. Pure virtual function syntax →

A. virtual void Display(){0};
B. virtual void Display = 0;
C. virtual void Display() = 0; ✅
D. void Display() = 0;

45. Header for cin & cout →

A. istream.h
B. ostream.h
C. iomanip.h
D. iostream.h ✅

46. Keyword to overload operator →

A. overload
B. operator ✅
C. friend
D. override

47. Class without name →

A. No destructor
B. No constructor
C. Not allowed
D. Both A and B ✅

48. class A : public X, public Y {} →

A. Multilevel
B. Multiple inheritance ✅
C. Hybrid
D. Hierarchical

49. Function call resolution →

A. Only II
B. Both
C. Only I ✅
D. Neither

50. Invalid visibility while inheriting →

A. public
B. private
C. protected
D. friend ✅

51. Friend function can access →

A. Public
B. Protected
C. Private
D. All of the above ✅

52. Correct statement →

A. Protected not allowed
B. Structure can have functions ✅
C. Class public default
D. Structure private default

53. Abstract class made by →

A. static keyword
B. virtual keyword
C. virtual function
D. Pure virtual function ✅

54. Default access specifier in class →

A. protected
B. public
C. private ✅
D. friend

55. Static data member →

A. Static function accesses static only
B. Shared by all objects
C. Accessed directly in main
D. Both A and B ✅

56. Reuse mechanism →

A. Abstraction
B. Inheritance ✅
C. Dynamic binding
D. Encapsulation

57. Correct statement →

A. Class is instance of object
B. Object is instance of class ✅
C. Class is instance of data type
D. Object is instance of data type
```
