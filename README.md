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
