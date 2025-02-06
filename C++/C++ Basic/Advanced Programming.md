# Advanced Programm

## 1. Introduction

The von Neumann architecture:

- Control unit
- Arithmetic logic unit with processor registers
- Memory for both data and instructions
- IO devices
- Bus: connects everything

## 2. Data-types

### 1. Initialization

```c++
int x{10};
auto x = 10;
```

### 2. Built-in data types and numerical accuracy

Binary representation error:  

- Fixed-point notation
- Floating-point notation

Overflow and underflow

Overview of built-in datatypes:

- unsigned
- bool, char, short int, int, long int, float, double
- enum

    ```c++
    enum Color {
        RED,    // 0
        GREEN,  // 1
        BLUE    // 2
    };

    enum class NewColor {
        RED,    // NewColor::RED
        GREEN,  // NewColor::GREEN
        BLUE    // NewColor::BLUE
    };
    ```

Bitwise operators: &, |, <<, >>

Fixed-width integer types:

- int8_t, int16_t, int32_t, int64_t, uint8_t, and many more
- std::size_t: Type that can store the maximum size of any object of any type.
- ! Appropriate type for loop iterations and array sizes.

### 3. Conversion

```c++
float a{1.2};
static_cast<int>(a)
```

Implicit conversions:

```c++
float a{1.2};
int b;
b = a;
```

Type conversion pitfall:

```c++
double d0 = 4 / 3 ;
double d1 = 4.0 / 3.0;
double d2 = 4.0 / 3 ;
```

Automatic type inference with auto:

```c++
int a1 = 3;
int b1 = 4;
auto c1 = a1 + b1; // yields int
double a2 = 3.0;
int b2 = 4;
auto c2 = a2 + b2; // yields double
```

### 4. Memory orgranization

### 5. Scope and lifetime

- Block scope
- Class scope
- Global scope

    ```c++
    // example.h
    #ifndef EXAMPLE_H
    #define EXAMPLE_H
    extern int globalVar;  // Declaration
    #endif // EXAMPLE_H

    // example.cpp
    #include "example.h"
    int globalVar = 42;  // Definition

    // main.cpp
    #include "example.h"
    int main() {
        int value = globalVar;
        return 0;
    }
    ```

- `namespace`: A namespace is used to create a scope where all names declared within it are only visible within that namespace.
- `static`: Using the static keyword before a global function or global variable makes it visible only within the file in which it is defined. This is useful for encapsulating implementation details and preventing name collisions.

### 6. C++ containers(string, array, vector, map)

- Arrays:
  - std::array: size is known at compile time, i.e., no insertion or deletion.

    ```c++
    #include <array>
    /* construction */
    std::array<int, 3> x_unit_vector = {1,0,0}; // before C++17
    std::array x_unit_vector = {1,0,0} ; // C++17
    /* accessing elements */
    x_unit_vector.at(5); // at() checks bounds - error
    x_unit_vector[5]; // no bounds checking - may run silently
    ```

- Vectors:
  - std::vector: size is not known at compile time, i.e., we can add or remove elements.

    ```c++
    #include <vector>
    /* construction */
    // 1. vector with 10 elements initialized to 0.0
    std::vector<double> grades_list( 10, 0.0 );
    // 2. Vector with three elements
    std::vector<double> grades_list = {55.5, 95.0, 70.0};
    // push_back: add elements to the end
    grades_list.push_back(85.0);
    ```

> Their elements are contiguous in memory.

- Ranged-for loops:

    ```c++
    std::vector<AdvProgStudent> advprog_students;
    std::vector<double> grades;
    // compute grades for all AdvProg students
    for ( const auto& student: advprog_students ){
        grades.push_back( student.compute_grade() );
    }
    ```

- STL algorithms:

    ```c++
    #include <algorithm>
    #include <vector>
    #include <numeric>
    // accumulate values in grades list, e.g., for average grade
    std::accumulate(grades_list.begin(), grades_list.end(), 0.0 );
    // sort the grades list
    std::sort( grades_list.begin(), grades_list.end() );
    // apply a function to each element in vector
    std::for_each(grades_list.begin(), grades_list.end(), bonus );
    ```

## 3. Functions

### 1. The call-stack

### 2. Pass by value -vs- pass by reference

```c++
f(Type& x); f(const Type& x); f(Type x);
```  

Definition of Matrix:

```c++
using Matrix = std::vector<std::vector<double>>;
Matrix pressure(1000, std::vector<double>(1000, 0));
```

### 3. Ruturn multiple values

```c++
std::pair<Type, Type> f(Type x)
auto [b, s] = offset(a);

std::tuple<int, double, std::string>
```

### 4. Function overloading

```c++
void offset_ref(double& x)
void offset_ref(const double& x)
double a = 1.23; offset_ref(a);
```

1. Number and types of arguments  
2. const, when applied to the entire function (member functions of classes). Note that ref and const-ref are treated differently.  

### 5. Default arguments

Default arguments must come after all the non-default arguments.  
They must not interfere with overloaded functions.

### 6. Template

```c++
template <typename T>  
T offset(T x){  
    return x + static_cast<T>(40);  
}
int main(){
    int b = offset<int>(2);
    return 0;
}
```

### 7. Anonymous functions (lambda expressions)

\[ captured_state \]( arguments ) -> return_type { body }  

```c++
// [&][=]
auto sum = [](int a, int b) -> int {
    return a + b;
};
std::cout << "Sum: " << sum(3, 4) << std::endl; 

int counter{0};
std::vector<double> grades {1.3, 2.7, 4.3, 2.0};
std::for_each( grades.begin(), grades.end(), [&counter](double& grade){ 
    if (grade <= 4.0 && grade > 1.0) {
        grade = offset(grade, -0.3);
        counter++;} });
```

Passing functions to... functions  

```c++
void apply_bonus(double& grade,
                    std::function<double(double, double)> operation){
    grade = operation(grade, -0.3);
    // void function don't need return.
}

auto super_bonus = [](double x, double step){return x + 3*step;};
for (auto& grade : grades) {
    apply_bonus(grade, offset);
    apply_bonus(grade, super_bonus);
}
```

### 8. Error handling (throwing & catching exceptions)

```c++
//throw
throw(std::invalid_argument("Invalid grade"));
//catch
try{
    apply_bonus(grade);
}
catch(const std::exceptiont& error){
    std::cerr << "Warning: " << error.what() << "\n";
}
```

### 9. Recursive functions

```c++
unsigned int factorial(unsigned int n){
    if (n <= 1) {
        return 1;
    } else {
        return factorial(n-1) * n; // function call + multiplication
    }
}
```

## 4. Resources Management

### 1. references & pointers

References: We cannot initialize a reference at runtime  
Pointers: Choosing an address at runtime  

In memory:  

```c++
int a = 5;
int & b = a;
int * c = &a;
std::cout << "a = " << a << ", &a = " << &a << "\n";
std::cout << "b = " << b << ", &b = " << &b << "\n";
std::cout << "c = " << c << ", &c = " << &c << ", *c = " << *c << "\n";
```

### 2. Managing dynamically allocated memory

delete and new:  

```c++
User * create_user(std::string name){
    User * new_user; // On the stack of create_user
    new_user = new User(name); // On the heap
    return new_user;
}
void register_student(std::string name){
    User * current_user = create_user(name);
    // ... copy user and data to a database
    delete current_user;
}
register_student("Lebowski");

new std::vector<int>*[10] {
    new std::vector<int>(5, 0),
    new std::vector<int>(5, 0),
    new std::vector<int>(5, 0),
};
delete[] vectorArray;
```

>On stack: mapping of (new) variables onto memory locations: deterministic and consecutive.  
>On heap: no such mapping possible: size of reserved data unknown at compile time.  

Memory leaks: Do we delete all the new temporary objects correctly?  No!  
Avoiding memory leaks: Smart pointers

```c++
// Only one pointer to the underlying memory allowed. Can't be copied.
std::unique_ptr<>
std::make_unique<>()

// Multiple pointers to the object allowed. Can be copied.
// The object is only deleted if there are no more pointers to it.
std::shared_ptr<>
std::make_shared<>()

// std::weak_ptr does not own the object, provides a non-owning reference to an object managed by std::shared_ptr.
// Can be used to monitor the object's lifecycle without extending its lifetime.
std::weak_ptr<>
// Typically constructed from a std::shared_ptr or another std::weak_ptr.
// The object will be deleted when the last std::shared_ptr is destroyed, even if std::weak_ptr instances still exist.
auto shared = std::make_shared<int>(10);
std::weak_ptr<int> weak(shared);
// Use the lock() method to attempt to obtain a std::shared_ptr. If the object is still alive, it succeeds.
if (auto temp = weak.lock()) {
    // The object is still alive, and you can use temp.
} else {
    // The object has been destroyed.
}

```

### 3. Array-like data types

```c++
std::array my_array<int,3> = {10, 20, 30} // Elements on stack
std::vector my_vector<int> = {10, 20, 30} // Elements on heap
```

>Lagacy: C-style arrays (on the stack) and dynamic arrays (on the heap)  
>Pointer arithmetic: address + N ! address to the N-th element in an array.

Operations of iterator(`std::array`, `std::vector`, `std::list`, `std::map`):

```c++
std::vector myvec = {10, 20, 30};
for ( int i = 0; i < myvec.size(); ++i ){
    std::cout << myvec[i];
}
for ( auto iter = myvec.begin(); iter != myvec.end(); ++iter ){
    std::cout << *iter;
}
```

STL Algorithms work with iterators:

```c++
std::vector<int> myvec = {95, 98, 2000, 10, 11};
std::sort(myvec.begin(), myvec.end());
for (const auto & elem : myvec){
    std::cout << elem;
}
// Output: 10, 11, 95, 98, 2000
```

Passing vectors to legacy functions:

```c++
void transform(int * data, size_t size){
    for (size_t i = 0; i < size; i++){
        data[i] = 2*data[i]; // some transformation
    }
}
int main(){
    std::vector<int> my_data = {10, 20, 30};
    transform(my_data.data(), my_data.size());
}
```

When to actually use raw pointers?  

1. compatible with older c++/c
2. interact with memory/hardware at a low level

### 4. lvalue & rvalue references

- lvalue references:

    ```c++
    void f1(int & x){
        std::cout << x;
    }
    void f2(const int & y){
        std::cout << y;
    }
    int main() {
        int a = 4;
        int b = a+1; // lvalue = rvalue
        f1(b); // compiles: x is another name for b
        f1(a+1); // does not compile: a+1 is not stored in a variable
        f2(a+1); // compiles (const lvalue references to rvalues allowed)
    }
    ```

- rvalue references:

    ```c++
    void f3(int && z){
        std::cout << z;
    }
    int main() {
        int a = 4;
        int b = a+1; // lvalue = rvalue
        f3(a+1); // compiles: && is an rvalue reference
        f3(b); // does not compile: b is an lvalue,
        // while z is an rvalue reference
    }
    ```

- When do we use && (rvalue references)? -- copying and moving

    ```c++
    // for example vector.push_back(some_element) offers at least
    void push_back( const T& value );  // Copying
    void push_back( T&& value );  // Moving
    ```

    >A unique_ptr cannot be copied, only moved.  
    >A shared_ptr can be copied.

## 5. Build time

### 1. Compilation

1. Overview of building flow:

   - Processor(-E): Copy-pasting all the text together
   - Compiler(-S,-c): Compilation and assembly and optimization
   - Linker(-o): Binding together binaries
       1. Include *statically linked libraries* in the binary.
       2. Any *dynamically linked libraries* will be loaded at runtime.

2. What do these binaries contain?: `\$ readelf -h code.o`
3. Are the binaries portable? Architecture and platform.
4. Runtime linking: Load our binary + shared libraries in memory.
5. Do I need to recompile?

### 2. Organizing our code

- `#include <iostream>`: Look for an iostream header in an implementation-specific way (system paths).  
 `#include "mysquare.h"`: Look for mysquare.h in the same directory first, then in the system paths.
- Header & implementation files: **Declaration** (and documentation) `int square(int);` vs **Definition**
- Order of inclusion matters: Dependencies, Forward declaration.
- Include guards: Avoiding multiple declarations

   ```c++
   #ifndef MYSQUARE_H
   #define MYSQUARE_H
   int square(int x);
   #endif
   ```

- Problems with including headers(1-4) --> A glimpse into the future of C++: *Modules*

### 3. Compile_time computation

Constant expressions allow us to run code at compile time:

- constexpr variables: `constexpr int z = 0;`  
Values of constant expressions must be known at compile time.
- constexpr functions: `constexpr unsigned ctime_fact(int n)`  
The function may be evaluated at compile time if its arguments are known at compile time. This function can still be called at runtime, as usual.

### 4. Tools to pack

- Build systems: *Make with Makefile*

  - make -j 4
  - make clean

   ```Makefile
   PROJECT_NAME = my_project
   CXX = g++
   CXXFLAGS = -Wall -std=c++17
   DEP = ./src/functions/functions.h
   BUILD_DIR = build
   
   ${PROJECT_NAME}: ./${BUILD_DIR}/main.o ./${BUILD_DIR}/functions.o
   ${CXX} $^ ${CXXFLAGS} -o $@

   ./${BUILD_DIR}/main.o: ./src/main. cpp ${BUILD_DIR} ${DEP}
   ${CXX} -c $< ${CXXFLAGS} -I ./src/functions/ -o $@

   ./${BUILD_DIR}/functions.o: ./src/functions/functions.cpp ${BUILD_DIR}
   ${CXX} -c $< ${CXXFLAGS} -0 $@

   ${BUILD_DIR}:
       mkdir $@

   .PHONY: clean
   clean:
       rm -rf ${PROJECT_NAME} ${BUILD_DIR}
   ```

- Build configuration systems: *CMake(Cross-platform way) with CMakeList.txt*

   ```CMake
   cmake_minimum_required(VERSION 3.22.1)

   set(CMAKE_CXX_STANDARD 17)
   set(CMAKE_CXX_STANDARD_REQUIRED ON)
   set(SRC src)

   project(my_project
   VERSION 1.0
   DESCRIPTION "An example project"
   LANGUAGES CXX)

   find_package(fmt)
   add_executable(${PROJECT_NAME} ${SRC}/main.cpp)

   set(FUNC_LIB functions)
   target_link_libraries(${PROJECT_NAME} ${FUNC_LIB} fmt::fmt)
   add_subdirectory(${SRC})

   target_compile_options(my_project PRIVATE -Wall)

   enable_testing()
   add_executable(mytest mytest.cpp)
   add_test(NAME test COMMAND mytest)
   target_link_libraries(mytest fmt::fmt)

   ```

   ```shell
   touch project/src/CMakeLists.txt
   cmake -S ../src -G "Unix Makefiles"
   make -j 4
   make test
   ```

- Distributing software

  - Distributing **code** or **binaries**
  - Package managers: System, Language, HPC

### 5. Pre-processor definitions / Macros (Object or function macros)

- Text manipulation using the preprocessor

   ```c++
   #define PI 3.14  // PI is a macro
   std::sin(2*PI);  // the preprocessor replaces by 3.14

   #define SQUARE(x) ((x) * (x))  // macro function, remember to use ().
   SQUARE(5.0)
   ```

- Conditional compilation

   ```c++
   #ifdef __linux__
   platform = "linux";
   #elif defined __WIN32
   platform = "win32";
   #endif
   ```

- Where are macros used?
  - assert
  - REQUIRE(sum(2+2)==4)
  - \_\_LINE\_\_, \_\_FILE\_\_

## 6. OOP1

### 1. Class basics

- Struct and Class

    ```c++
    struct Color {
        int red;
        int green;
        int blue;
    };
    Color TUMBlue={0, 101, 189};
    std::cout<<TUMBlue.red<<'\n';
    std::cout<<TUMBlue->red<<'\n';

    struct moreColor : Color{
        int black;
    }
    ```

- Declaration and Definition

    ```c++
    double Point(); //In Point.h
    double Point::Point() {return _x;} //In Point.cpp
    ```

- Access Modifier(Visibility and accesibility)
  - private, protect, public
  - static
- Constructors and destructor

    ```c++
    class Point {
        /* (as before) */
        public:
            Point(){} // default constructor, usually =default

            Point(double x, double y){
            std::cout << "Creating a point\n";
            _x = x;
            _y = y;}
            
            ~Point() {std::cout << "Destructing a point\n";} 
            // If a C++ class doesn't have an explicitly defined 
            // destructor, the  compiler will provide a default 
            // destructor for that class.
        };
    ```

    Default copy constructors copy member-by-member.  
    -> copy a pointer, but not the underlying dynamically allocated memory, if any (“shallow copy”).  
    If a class does not contain such pointers, the defaults are enough!

- Setters and Getters

    ```c++
    Point(double x, double y){
        set_coordinates(x,y);
    }
    void set_coordinates(double x, double y) {
        // we only want points with positive coordinates
        // (for any reason)
        if (x > 0.0) _x = x;
        if (y > 0.0) _y = y;
    }
    ```

- Classes using other classes (composition)

    ```c++
    class Line {
    private:
        std::array<Point, 2> _vertices;
    public:
        Line(Point startp, Point endp){
            _vertices[0] = startp;
            _vertices[1] = endp;
        }
        double length() {
            return std::sqrt(
            std::pow(_vertices[1].x() - _vertices[0].x(), 2) +
            std::pow(_vertices[1].y() - _vertices[0].y(), 2) );
        }
    };
    ```

- Operator overloading

    ```c++
    Point operator-(const Point & other_point) {
        return Point( _x - other_point._x, _y - other_point._y );
    }
    ```

- Copy constructor

    ```c++
    class Line {
        /* (same as before) */
        public:
            Line(const Line & from_line){
                std::cout << "Copying a line\n";
                _vertices = from_line._vertices;
            }
    };

    Line line1(point1, point2);
    Line line2(line1); // make a copy of line1
    // equivalent:
    Line line3 = line1;
    ```

    1. Even if _vertices is private, both lines are of the same class, so we can access all the private members
    of from_line.  
    2. In the copy constructor, const matters, as it allows us to pass an rvalue: `Line line2( Line(point1, point2) );`  
    3. If we don’t explicitly define a constructor, destructor, and other special member functions, the compiler implicitly defines them.

- Asking for defaults (or avoiding them)

    ```c++
    class Point {
        /* (as before) */
        public:
            Point() = default; // notice the missing {}
            Point(double x, double y){ /* as before */ }
            Point(const Point & from_line) = delete;
            Point& operator=(const Point& from_line) = delete;
    };
    ```

    1. If we define our custom constructor, the default constructor will not be generated and we can ask the
    compiler to also generate it.  
    2. We can also forbid some operations, by deleting them.

### 2. Classes managing dynamic memory

- A path made of lines

   ```c++
   class Path {
       std::size_t _num_edges;
           Line * _edges; // raw pointer only to showcase an issue
       public:
           Path(std::size_t num_edges, Line * edges){
               _num_edges = num_edges;
               _edges = new Line[_num_edges]; // assume Line() = default
               /* assign edges with a for loop */
           }
           ~Path(){
               // RAII: The same object that called new, also calls delete
               delete[] _edges;
           }
   };
   ```

   ![A path made of lines](images/A%20path%20made%20of%20lines.png)
- Copy constructors in cases of dynamic memory

   ```c++
   Path(const Path& from_path){
       _num_edges = from_path._num_edges;
       _edges = new Line[_num_edges];
       /* copy edges with a for loop */
   }

   Path& operator=(const Path& other) {
       if (this != &other) { // Protect against self-assignment
           delete[] _edges; // Free existing resource
           _num_edges = other._num_edges;
           _edges = new Line[_num_edges];
           for (std::size_t i = 0; i < _num_edges; ++i) {
               _edges[i] = other._edges[i];
           }
       }
       return *this;
   }
   // Cell &operator=(const Cell &from);  in Cell.h
   // Cell &Cell::operator=(const Cell &from) {...}  in Cell.cpp
   ```

   “deep copy”: copy everything, including the dynamic memory.  
   “shallow copy”: only copy the pointer (_edges), point to the same memory location.

### 3. Classes with constant members & methods

- Initializing const members

   ```c++
   class PointND {
   private:
       const std::size_t _N;
       std::vector<double> _coordinates;
   public:
       PointND(std::size_t dimensions, std::vector<double> coordinates)
       : _N(dimensions) // We have to initialize _N here
       {
           std::cout << "Constructing a PointND\n";
           _coordinates = coordinates;
       }
   };
   ```

   **Note:** Same requirement for initializing member references: int & a;

- Initializing any members

   ```c++
   : _N(dimensions), _coordinates(coordinates)
   ```

   *Prefer initialization to assignment in constructors.*

- const methods to operate on const objects (or on any object)

   ```c++
   class PointND {
       /* as before */
       std::vector<double> get_coordinates() const {
           return _coordinates;
       }

       void set_coordinates(std::vector<double> new_coordinates) {
           _coordinates = new_coordinates;
       }
   };

   const PointND origin( 3, std::vector<double>({0.0, 0.0, 0.0}) );
   origin.get_coordinates(); // Works! :-)
   ```

   On a const object, we can only call const methods.  
   Useful in any object: making a method const specifies that calling it will not change the state of the object.

### 4. Friends of my class

```c++
class PointND {
    // I allow this function to access my private members
    friend ostream & operator<<(ostream &, const PointND &);
};

std::ostream & operator<<(std::ostream & to_stream, const PointND & from_point) {
    for (auto c : from_point._coordinates) { // No getter!
        to_stream << c << " ";
        }
return to_stream;
}

const PointND origin( 3, std::vector<double>({0.0, 0.0, 0.0}) );
std::cout << "Origin: " << origin << "\n";
```

We can also declare other classes as friends. `friend class B;`

Use cases:

1. Allow a unit testing class to access private members (check state).
2. Strict encapsulation: make everything private, only allow specific friends.

## 7. OOP2

### 1. Move semantics

- Recap Copy

    1. Deep copy: ...
    2. Shallow copy: ...

- Move(Pointers) -- Copy the pointer, invalidate the original.

    1. Move constructor

        ```c++
        class Path {
        private:
            std::size_t _num_edges = 0;
            Line *_edges = nullptr;
        public:
            /* as before */
            
            Path(Path&& from_path){
                _num_edges = from_path._num_edges;
                _edges = from_path._edges;

                from_path._num_edges = 0;
                from_path._edges = nullptr;
            }
        };
        ```

    2. Move-assignment operator

        ```c++
        class Path {
        /* as before */

        Path& operator=(Path&& from_path){
            if(this != &from_path){
                _num_edges = from_path._num_edges;

                delete[] _edges;
                _edges = from_path._edges;

                from_path._num_edges = 0;
                from_path._edges = nullptr;
                return *this;
            }
            else return *this;
        }
        ```

- Move operators

    To generate an && from an lvalue, we can use `std::move()`

    ```c++
    Path path_new(std::move(path1)); // Move constructor
    path_new = std::move(path2); // Move-assignment operator
    ```

    >**Note:** std::move does not move anything (despite the name).

- When & why to use move operators

    1. Passing dynamic allocated resources to functions if no longer needed. (save memory & runtime)
    2. Temporary objects: `Constructor(std::string s): _s(std::move(s))`
    3. Many STL methods move, if possible.
    4. A std::unique_ptr can only be moved, not copied (only one owner).

    >**Note:** We can only move resources if they are not needed anymore / in the meantime.

- Two important rules

    1. The rule of zero(Usual case): No need to define our own copy, move or destructor function.
    2. The rule of five(Special case): if you define (even `=default`) or `delete` any of the copy, move, or destructor function, define them all.

### 2. Inheritance

- A Stripe is a Line with thickness

    ```c++
    class Stripe : public Line {
    private:
        int _thickness;
    public:
        Stripe(Point startp, Point endp, int thickness)
        : Line(startp, endp) // First, make a Line object
        // Initialization list can also call function.
        {
            std::cout << "Constructing a stripe\n";
            _thickness = thickness;
        }
    };
    ```

- protected access (Only me and my children)

    ```c++
    class Line {
    protected:
        std::array<Point, 2> _vertices;
    /* more */
    };

    // Now Stripe can access _vertices.
    // Although Stripe isn't friend, it's only chid of Line.
    Stripe::Stripe(Point startp, Point endp, int thickness)
    : Line(startp, endp){ // access _vertices in line from stripe
        auto x = Line::_vertices;
    }
    ```

- Non-public inheritance

    ```c++
    class Stripe : public Line { /* as before */ };

    class Stripe : protected Line { /* as before */ };

    class Stripe : private Line { /* as before */ };
    ```

### 3. Virtual functions

- Redefining base class

    ```c++
    class Brush {
    public: void draw() {
        std::cout << "I am a generic brush, I don't know how to draw\n";
        }
    };

    class Pencil : public Brush {
        public: void draw() {
        std::cout << "Drawing with a Pencil\n";
        }
    };
    ```

- Virtual functions

    ```c++
    class Brush {
    private:
        float _size;
    public:
        virtual void draw() = 0; // =0: Pure virtual function
                                 // Not implemented: Brush is abstract.
    };

    class Pencil : public Brush {
    public:
        void draw() override {/* ... */} // override: Intending to redefine 
                                         // it (must exist in base). Optional, 
                                         // but nice.
    };
    ```

    Abstract methods (*pure virtual functions*): must be defined in a derived class to make it concrete – otherwise, we cannot create an object.

- Runtime polymorphism

    1. Select the class of object at runtime

        ```c++
        unique_ptr<Brush> mybrush;
        mybrush = make_unique<Pencil>();
        mybrush->draw();
        ```

    2. A common interface

        ```c++
        void paint(unique_ptr<Brush> & b) {
            b->draw();
        }

        unique_ptr<Brush> mybrush;
        mybrush = make_unique<Pencil>();
        paint(mybrush);
        ```

        >This only works using a pointer to a common base class with a **virtual** (but not necessaril implemented) `draw()`.

        ![Virtual Function Tables1](images/Virtual%20Function%20Tables1.png)

    3. virtual destructors

        Here only the *Brush* destructor will be called, potentially leaking the memory that Pencil manages.

        ```C++
        std::unique_ptr<Brush> mybrush;
        mybrush = std::make_unique<Pencil>();
        ```

        So we need to declare the *Brush* destructor as virtual.

        ```C++
        virtual ~Brush() {}
        ```

        ![Virtual Function Tables2](images/Virtual%20Function%20Tables2.png)

    4. Slicing: a common mistake  

        Assigning or copying a derived class object to a base class object.

        ```c++
        virtual void Brush::draw() { cout << "Painting with a Brush"; }
        void Pencil::draw() override { cout << "Painting with a Pencil"; }
        Brush mybrush = Pencil();
        mybrush.draw();
        // Output: Painting with a Brush
        // Same problem without virtual
        ```

        >Do not copy, prefer pass-by-reference.  
        >Use a (smart) pointer when implementing runtime polymorphism.

### 4. Visitor design pattern

>To add a new operation, just add a new Visitor class.

- The classical Visitor design pattern

    ![The classical Visitor design pattern](images/The%20classical%20Visitor%20design%20pattern.png)

    ```c++
    class Pencil : public Brush {
    public: void accept(const BrushVisitor & v) override { v.visit(*this); }
    };

    class Draw : public BrushVisitor {
    public: void visit(const Pencil &) const override { std::cout << "Draw with a pencil\n"; }
    public: void visit(const FountainPen &) const override { std::cout << "Draw with a pen\n"; }
    };

    int main(){
    std::unique_ptr<Brush> mybrush;
    mybrush = std::make_unique<Pencil>();
    mybrush->accept( Draw{} );
    mybrush->accept( Erase{} ); // Just another BrushVisitor
    }
    ```

- A modern C++ implementation of Visitor

    ```c++
    class Pencil{}; // Does not derive from any base class, nothing special

    class Draw { // Only definining operator() for different types
    public: void operator()(const Pencil&) const { std::cout << "Draw with a pencil\n"; }
    public: void operator()(const FountainPen&) const { std::cout << "Draw with a pen\n"; }
    };
    
    int main(){
    using Brush = std::variant<Pencil, FountainPen>; // Store either a Pencil or a FountainPen
    Brush pen{}; // pen is default-initialized to Pencil
    pen = FountainPen{}; // pen is now a FountainPen
    std::visit(Draw{}, pen);
    std::visit(Erase{}, pen);
    }
    ```

## 8. Templates

### 1. Function and class templates

- Example problem: sorting

    1. Copy&paste
    2. Inheritance
    3. Templates

1. Function Basics

    ```c++
    template<typename T>
    T add(T first, T second) {
        return first + second;
    }

    int main() {
        add(1,1);
        add<int>(1,1);
    }
    ```

2. Requirements

    Sometimes will the Type T not addable, we need to define the operator+.

    ```c++
    template<typename T>
    T add(T first, T second) {
        return first + second;
    }

    class Point{ /* as before */
        Point operator+(const Point& other_point){
            return Point(_x + other_point._x, _y + other_point._y);
        }
    };

    int main() {
        std::cout << add(3, 5) << "\n" << add(3.0, 5.0) << "\n" << add(std::string("3"), std::string("5")) << "\n";

        Point point1(1.0, 1.0), point2(2.0, 2.0);
        std::cout << add(point1, point2) << "\n";
    }
    ```

3. Class Basics

    ```c++
    template<typename T>
    class Pair {
        T _first, _second;
    public:
        Pair(T first, T second);
    };

    int main() {
        Pair<int> pair_int(1, 2);
        Pair pair_double(1.0, 2.0);
        Pair<std::string> pair_string("hello", "world");
        // The <> part in *class* templates is optional in C++17 or newer
    }
    ```

    Every instance of a template is a different type:

    ```c++
    int main() {
        Pair pair_double(1.0, 2.0);
        Pair<std::string> pair_string("hello", "world");
        pair_double = pair_string; // This does not work: different types
    }
    ```

4. Class templates with multiple types

    ```c++
    template<typename T1, typename T2>
    class Pair {
        T1 _first;
        T2 _second;
    public:
        Pair(T1 first, T2 second);
    };
    ```

5. Default arguments

    For example, std::vector has a second, default argument:

    ```c++
    template<class T, class Allocator = std::allocator<T>> class vector;
    ```

6. Non-type template parameters

    Remember std::array<type, size>. Similarly

    ```c++
    template<typename T, size_t dimensions>
    class PointND {
        std::array<T, dimensions> _coordinates;
    public:
        PointND(std::array<T, dimensions> coordinates);
    };

    int main() {
        PointND point3D(std::array<int, 3>({0,0,0}));
    }
    ```

    >PointND<int,2> is a different type than PointND<int,3>  
    >Size known at compile-time

7. Class templates in class templates

    ```c++
    int main() {
        PointND<double, 3> xyz(std::array<double, 3>({5.0, 3.0, 2.0}));
        PointND<double, 2> xy(std::array<double, 2>({5.0, 3.0}));
        Pair<PointND<double, 3>, PointND<double, 2>> projection(xyz, xy);
        // or, implicitly:
        Pair projection(xyz, xy);
    }
    ```

### 2. Evaluation at compile time

>Note:  
>We cannot (nicely) split templates into a declaration and a definition part. We usually write the complete template of a function/class in a header file.  
>A function/class template is not a function/class yet.  
>Generation happens at compile time -> no performance overhead.
>Heavily-templatized codes tend to have a very long compilation phase and often very large error output.

- What does the compiler generate?

    ```c++
    template<typename T1, typename T2>
    class Pair {
        T1 _first;
        T2 _second;
    public:
        Pair(T1 first, T2 second);
        T1 getFirst();
        T2 getSecond();
    };

    int main() {
        Pair<int, std::string> pair_int_string(42, "world");
        std::cout << pair_int_string.getFirst() << "\n";
    }
    ```

    >Here, `getSecond()` is not mentioned ! Not generated in the binary.

- Two-phase lookup

    1. When the template is initially parsed. Lookup of non-dependent names and check of all non-template specific syntax
    2. When an instance of the template for a specific type is created (all information is present). Lookup of dependent names and check of dependent syntax

    ```c++
    template<typename T>
    T add(T first, T second){
        return first + second[]; //first-phase: wrong syntax
    }

    class Point{
        double _x = 0.0, _y = 0.0;
    public:
        Point(double x, double y): _x(x), _y(y) {}
    };

    int main(){
        Point point1(3.0, 5.0), point2(2.0, 6.0);
        /* second-phase error: no operator+ for Point */
        auto result = add(point1, point2);
    }
    ```

### 3. Template specialization

Special-case implementation for specific types:

```c++
template<typename T, size_t dimensions>
class PointND {
    std::array<T, dimensions> _coordinates;
public:
    PointND(std::array<T, dimensions> coordinates);
};

template<typename T>
class PointND<T, 2> { // Special case for 2D points:
    T _x, _y; // just x,y instead of a coordinates array
public:
    PointND(std::array<T, 2> coordinates):
    _x(coordinates[0]), _y(coordinates[1]);
};
```

### 4. Variadic templates

```c++
template<typename... Types> // Yes, the ... is part of the syntax
auto sum( Types... args){
    return (... + args);
}

int main(){
    float x = 1.1;
    double y = 2.5;
    int z = 4;
    double p = 3.14;
    std::cout << sum( x, y, z, p ) << "\n";
}
// ------------------------------------------------------

// Example function: Perform an operation on the argument
void process(int value) {
    std::cout << "Processing: " << value << std::endl;
}

// Using a fold expression to call the process function on each argument
template<typename... Types>
void callProcess(Types... args) {
    (process(args), ...);
}

int main() {
    callProcess(1, 2, 3, 4); // Call process function on each argument
    return 0;
}

```

We can also pass these variadic arguments to other functions. For example: `make_unique<T>`, `make_shared<T>` or `std::tuple`.

### 5. C++20 concepts

```c++
template<typename T>
concept Addable = requires(T x) { x + x; };

template<Addable T>
T add( T first, T second){
    return first + second;
}
// or
template<typename T>
    requires Addable<T> // && other constraints
T add(T first, T second){
    return first + seconde;
}

int main(){
    auto result = add(point1, point2);
}
```

### 6. Template meta-programming / Expression templates

## 9. STL

### 1. Containers

- C++ provides different kinds of containers:

    1. Sequence containers: array, vector, list, forward_list, deque...  
    2. Associative containers: set, map, multimap...  
    3. Unordered associative containers: unordered_set, unordered_map, unordered_multiset...

- A container is a class template (std::vector)

    1. `double *data = myVector.data()`
    2. `.size()`
    3. `operator[]`
    4. `.begin()`; `.end()`

- Some different containers

    ```c++
    std::array<int, 5> myArray = {1, 2, 3, 4, 5};

    std::vector<int> myVector{1, 5, 2, 13, 7};

    std::list<std::string> myList{"bananas", "oat", "cheese", "coffee", "oat", "apples"};

    std::set<std::string> mySet{"C++11", "C++98", "C++20", "C++17"};

    std::map<std::string, int> myMap{{"Alice", 165}, {"Bob", 163}, {"Chris", 185}, {"Dora", 170}};

    // Not a container, but remember also std::pair to associate two objects (first and second).
    ```

- Vector, deque, and list:

    1. Vector:
       1. Contiguous: elements next to each other in the memory.
       2. We can access elements randomly: We know that `myVector[i]` is at the address of `myVector.data()+i`.
    2. Deque
       1. Same as Vector
       2. `push_front` and `push_back`
    3. List:
       1. Non-contiguous: elements are not necessarily next to each other (cache-inefficient)
       2. Every element knows which element is before and after it. (or only after, in `std::forward_list`)
       3. We can traverse a list front-to-back or back-to-front, but not randomly.

- Map and unordered map

    1. Map:
       1. Key-value pairs
       2. Non-contiguous, implemented as a balanced binary tree
       3. Keys are sorted (O(log(N)) lookup), requires `operator<`
       4. Accessed as `my_map[my_key]`
       5. **Careful**: If a key is not found, it is automatically inserted, with default value!
    2. Unordered map:
       1. Same as map, but implemented as hash table
       2. Keys are not sorted, but hashed (requires `operator==`)

- Set und unordered set

### 2. Iterators and Ranges

- Iterators
  - An iterator is usually implemented as a class and provides a general way to traverse over any C++ container.
  - Every iterator supports ++ (next element) and * (dereference)
  - Different iterator types support different operations:
      1. Forward: * and ++ (e.g., std::forward_list)
      2. Bidirectional: same as Forward, with -- (e.g., std::list)
      3. RandomAccess: as Bidirectional, with +, -, +=, -=, [] (e.g., std::vector)
      4. Also: Input (read-only), Output (write-only), and more iterators.

- Ranges(C++20)
  - A range is something we can iterate over, from .begin() to .end():

    ```c++
    // C++20 ranged sorting:
    std::ranges::sort(myVector);
    ```

  - Composing algorithms:

    ```c++
    std::vector<int> myVector{1, 5, 2, 13, 7, 4};

    // A composition of operation:
    // 1. Create a view v of myVector
    // 2. Filter out odd elements
    // 3. Apply f(x)=x^2 to the remaining elements
    auto v = myVector
    | std::views::filter( [](auto i){ return i % 2 == 0; } )
    | std::views::transform( [](auto i){ return i*i; } );

    // Result: {4, 16}
    // (lazy evaluated, only when we access v --> cheap!)
    ```

### 3. Algorithmus

- std::generate

    ```c++
    std::vector<int> myVector(1000000);
    std::generate(myVector.begin(), myVector.end(), std::rand);
    ```

- std::iota

    ```c++
    std::vector<int> v(10);
    std::iota(v.begin(), v.end(), 100);
    ```

- std::max_element (min_element)

    ```c++
    std::vector<int> myVector{1, 5, 2, 13, 7};  // std::list, std::set
    max_element_value = *std::max_element(myVector.begin(), myVector.end()); // dereference!
    
    std::map<std::string, int> myMap{{"Alice", 165}, {"Bob", 163}, {"Chris", 185}, {"Dora", 170}};
    auto max_map_elem_value =
        *std::max_element(
            myMap.begin(), myMap.end(),
            [](auto & p1, auto & p2){return p1.second < p2.second;}
        );
    ```

- std::sort

  - It needs to be able to access the elements in any order. So we can't sort list with std::sort. But we can use list.sort().
  - `std::sort()` and `std::stable_sort()`

    ```c++
    std::vector<int> myVector{1, 5, 2, 13, 7};
    std::sort(myVector.begin(), myVector.end());
    std::sort(myVector.begin(), myVector.end(), std::greater<int>());
    ```

- std::accumulate

    ```c++
    auto sum = std::accumulate(myVector.begin(), myVector.end(), 0);
    auto product = std::accumulate(myVector.begin(), myVector.end(), 1, std::multiplies<int>());
    auto result = std::accumulate(myVector.begin(), myVector.end(), 0, [](int x, int y) { return x + 2 * y; });
    ```

- std::transform

    ```c++
    std::vector<int> myVector{1, 5, 2, 13, 7};
    std::vector<int> transformed(myVector.size());

    std::transform(myVector.begin(), myVector.end(), // Input
                   transformed.begin(), // Output
                   [](int& x){return x*x;}); // Operation
    ```

- std::for_each

    ```c++
    // A Caesar encryption (of lowercase English characters)
    void encrypt(std::string & message) {
        const int offset = 7;
        for (auto i = 0; i < message.size(); i++){
            message[i] = (((message[i] - 'a') + offset) % 26) + 'a';
        }
    }
    
    // Our shopping list
    std::list<std::string> myList{"bananas", "oat", "cheese", "coffee", "oat", "apples"};

    // Calling encrypt (of one argument) on each element
    std::for_each(myList.begin(), myList.end(), encrypt);
    ```

- std::find

    ```c++
    std::list<std::string> myList{"bananas", "oat", "cheese", "coffee", "oat", "apples"};

    // Do we already have milk in the list?
    // Returns an iterator to the first "milk", if found.
    auto milk_in_basket = std::find(myList.begin(), myList.end(), "milk");

    // If we already went out of the list while searching,
    // insert "milk" at the end (yes, push_back also works).
    if (milk_in_basket == myList.end()){
        myList.insert(myList.end(), "milk");
    }
    ```

- std::find_if

    ```c++
    bool is_fruit(std::string & x){
        std::set<std::string> fruits{"apples", "pears", "bananas"};
        return std::find(fruits.begin(), fruits.end(), x) != fruits.end();
    }

    std::list<std::string> myList{"chocolate", "chips", "coffee"};

    // We definitely need to buy fruit!
    auto fruit_in_basket = std::find_if(myList.begin(), myList.end(), is_fruit);

    if (fruit_in_basket == myList.end()){
    std::cout << "An apple a day keeps the doctor away!\n";
    }
    ```

- Predicates

    |Acting based on the values themselves | Acting based on predicates|
    |---|---|
    |std::find(b, e, value)|std::find_if(b, e, predicate)|  
    |std::count(b, e, value)|std::count_if(b, e, predicate)|
    |std::replace(b, e, old_value, new_value)|std::replace_if(b, e, predicate, new_value)|
    |std::copy(b, e, target)|std::copy_if(b, e, target, predicate)|
    |std::sort(b, e)|std::sort(b, e, predicate)|
    |...|...|

- Other algorithms

    1. count/count_if
    2. replace/replace_if
    3. copy/copy_if
    4. shuffle
    5. remove/remove_if
    6. any_of/all_of/none_of
    7. reduce
    8. inner_product
    9. ...

### 4. Parallel algorithms / Execution policy

- Most STL algorithms can be executed in parallel (since C++17)

    ```c++
    // A very large vector of random values
    std::vector<int> myVector(1000000);
    std::generate(myVector.begin(), myVector.end(), std::rand);
    std::sort(std::execution::par, myVector.begin(), myVector.end());
    ```

- Execution policy (hints for the compiler):

  1. seq: sequential (default)
  2. par: parallel ! multiple threads on multiple cores
  3. par_unseq: parallel and/or vectorized

>Include `<execution>`, link to Intel TBB library with `-ltbb`.

## 10. Performance

### 1. Motivation

Performance is more than not waiting a bit longer:  

  1. Simulations: Better approximations in given time and memory.
  2. Mobile / embedded / HPC: More efficient energy usage.
  3. Real-time applications (games, audio): Better experience (more FPS).
  4. Commercial applications (banking, databases): More transactions per second with the same resources, i.e., lower cost.

### 2. The Neumann bottleneck

In our programs we want:

1. Fast computations [FLOP/s]
2. High memory throughput [GB/s]

Memery gap -> Idea of a cache memory -> Cach levels  
Measuring the memory bandwidth: STREAM

### 3. Pipelining and Out-of-order execution

1. Pipelining

    Different units for each sub-operation, Pipelining concept to speed up computations.  
    Aim: Write code in a way that keeps the pipeline full
    (e.g., avoid branching inside loops).

2. Out-of-order excution

### 3. The Roofline model

Two major upper bounds for performance:  

$$P = min(P_{max}, I \cdot b_s)$$

1. Applicable peak performance $P_{max}$[operations / s] (assuming data in L1 cache)
2. Memory throughput
    1. Code characterized by computational intensity $I$ [operations / byte]
    1. System-specific applicable peak bandwidth $b_s$ [byte / s]

![The Roofline model](images/The%20Roofline%20model.png)

## 11. Optimization

### 1. A few guidelines

[C++ Perfermance Guidlines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#S-performance)

Per.1 Don’t optimize without reason  
Per.2 Don’t optimize prematurely  
Per.3 Don’t optimize something that’s not performance critical  
Per.4 Don’t assume that complicated code is necessarily faster than simple code  
Per.5 Don’t assume that low-level code is necessarily faster than high-level   code  
Per.6 Don’t make claims about performance without measurements  
Per.10 Rely on the static type system  
Per.12 Eliminate redundant aliases  
Per.13 Eliminate redundant indirections  
Per.14 Minimize the number of allocations and deallocations  
Per.15 Do not allocate on a critical branch  
Per.16 Use compact data structures  

### 2. Understanding our code

- Profiling--Example: gprof (GNU profiler)  

    ```sh
    g++ -pg main.cpp -o main
    ./main
    gprof ./main
    ```

- More performance analysis tools

### 3. What we can do

1. Moving computation to the compile time  

    Declare variables/functions as `const` or `constexpr`, if possible.

2. Reducing work
3. Avoiding expensive computations
4. Avoiding branches
5. Use the right data structures
    1. Contiguous data structures faster for serial/random access(e.g.,  `std::vector` faster than `std::list`).
    2. Adding/deleting internal nodes on a list is much faster than on a vector.
    3. Use a `std::string_view` instead of `std::string` for read-only arguments.
    4. For small sizes, arrays get precomputed, leading to significant difference in performance compared to vectors.
6. Memory alignment of structures
    1. 64bit processors accesses memory in 64bit (8 byte) words in each cycle.
    2. Data types must be aligned with words or half words.(1 Byte types are allowed anywhere)
    3. Cache lines contain a few words.  
    4. Padding: Empty space is introduced to not split data between words.
    5. The order of members in a struct/class matters.
    6. hole detection
7. Reducing cache misses
8. Memory access: Blocking
9. Inline functions

    ```c++
    inline int max(int a, int b){ /* ... */ }
    ```

    1. At compile time: function body is embedded at every place where function is called.
    2. Inlined functions need to be implemented in a header file.
    3. It is up to the compiler whether to inline or not, as it may find that it is not worth it

### 3. What the compiler can do

1. Subexpression elimination
2. Loop invariants
3. Strength reduction
4. Evaluation of constant expressions
5. Attributes that can help the compiler optimize

    ```c++
    if (x > 0) {
    std::sqrt(x);
    } else [[unlikely]] {
    std::throw(std::invalid_argument("x<=0"));
    }

    void foo(int x){
    [[assume(x > 0)]]
    std::cout << 1 / x;
    }
    ```

6. Loop unrolling
7. Loop fusion

## 12. Closing

### 1. History of C++ and compatibility with C

1. Using C libraries in C++

    ```c++
    extern "C"{                 // some C networking library providing
        #include "network.h"    // struct Socket; int initSocket(Socket*);
    }                           // int send(Socket* s, int* data, int count);
    // C++ code
    std::vector<int> elements = {4, 7, 9, 20};
    Socket socket;
    initSocket(&socket);
    send(&socket, elements.data(), elements.size());
    ```

2. Use `vector.data()` and `vector.size()` for pointer + size
3. Use `std::string.c_str()` for zero-terminated C-string access.

### 2. Some of the missing content

- Concurrency: starting threads and distributing work to them
- Bitwise operations
- Streams and input/output, text manipulation
- User-defined literals
- Namespaces
- Error handling
- ...

### 3. Upcoming C++ features

1. C++20
   1. std::span
   2. Coroutines
2. C++23
   1. std::mdspan
   2. std::expected
   3. std::print
3. C++26
   1. Pack indexing
   2. A linear algebra library

### 4. Contributing to a free software project

1. What is a contribution?  
Issues, Documentation, Code, Code reviews, Testing, Operations, User support, Onboarding...
2. How to contribute code?  
   1. “Fork” the repository -> your own copy of repository
   2. Create a separate branch -> you may want to pull again later
   3. Code and run any tests/formatter/...
   4. Commit & push to your fork

### 5. My first long-term project

1. **LICENSE**: What do you want others to be able to do with the code?
   - MIT: Do almost anything you want
   - GPL: Do almost anything, as long as it stays free
   - More licenses on <https://choosealicense.com/>
2. **README.md**: What is this project? How to use it?
3. **CONTRIBUTING.md**: How and where can someone contribute?
4. **Code of conduct**: How should contributors behave?  
Popular option: <https://www.contributor-covenant.org/>
5. **Build system**: Setup CMake or similar early enough
6. **Tests**: Setup tests and continuous integration early enough
7. **More**: Documentation, Issue templates, ...

## 13. Vectorization

### 1. Auto-vectorization

1. Use simple for loops
    - Countable
    - Single entry and single exit
    - Contain straight line code
    - Innermost loop of a nest
    - Without function calls
    - Avoid dependencies between loop iterations
2. Use array notations instead of pointers
3. Use the loop index directly in array subscripts
4. Use memory efficiently
    - Favor inner loops with unit stride
    - Minimize indirect addressing
    - Align your data to 16-byte boundaries
5. Choose a suitable data layout with care
6. Use aligned data structures
7. Use structure of arrays (SoA) instead of array of structures (AoS)

    ```c++
    typedef struct {
        // positions in x-, y- and z-direction (64 byte aligned)
        double x[NUMBEROFMOLECULES] __attribute__((aligned(64))),
               y[NUMBEROFMOLECULES] __attribute__((aligned(64))),
               z[NUMBEROFMOLECULES] __attribute__((aligned(64)));
        // velocities in x-, y- and z-direction (64 byte aligned)
        double velocityX[NUMBEROFMOLECULES] __attribute__((aligned(64))),
               velocityY[NUMBEROFMOLECULES] __attribute__((aligned(64))),
               velocityZ[NUMBEROFMOLECULES] __attribute__((aligned(64)));
    } __attribute__((aligned(64))) Molecules;
    ```

### 2. Vector intrinsics

1. Nomenclature: `_mm[result length]_<instruction>_<input length>()`

2. AVX-512 example:

    ```c++
    for (int i = 0; i < NUMBER_OF_ELEMENTS; i += 8) {
        a = _mm512_load_pd(&as[i]);
        b = _mm512_load_pd(&bs[i]);
        c = _mm512_add_pd(a, b);
        _mm512_store_pd(&cs[i], c);
    }
    ```

3. What else?
    - Rotate
    - Leading zero count
    - Encrypt
    - Reduce

## 14. Other Concepts

1. volatile

    It's used to tell the compiler that the variable may be modified by external factors, such as multithreading or hardware,   hence preventing the compiler from optimizing it.  
    This means that every reference to a variable marked with the volatile  keyword will directly fetch the value from memory, rather than using a potentially optimized value.

    ```c++
    volatile int *ptr = (int*)0x4000;
    ```

2. Union

    A union is a special data type that can store different data types, but only one at a time. The size of a union is determined by its largest member.  

    ```c++
    union Data {
        int i;
        float f;
        char str[20];
    };
    ```

3. Bit Field

    A bit field is a feature that allows the members of a data structure to be packed into one or more bits.  
    However, note that there are some limitations and quirks to bit fields that you should be aware of when using them.

    ```c++
    struct BitField {
    unsigned int is_keyword : 1;
    unsigned int is_extern  : 1;
    unsigned int is_static  : 1;
    };
    ```

4. nonexcept

    If a function is declared noexcept, it should not throw any exceptions. If it does throw an exception, the program will call std::terminate to immediately terminate execution.  
    It can help the compiler optimize code, because the compiler knows that the function does not throw exceptions and can generate more efficient code as a result.

    ```c++
    void foo() noexcept {
        // ...
    }
    ```

5. Function pointer

    A function pointer is a special type of pointer that stores the address of a function, rather than the address of data. This allows us to call a function using the pointer.  
    Function pointers are incredibly useful in many situations, for instance, you can use them to implement callback functions or create an array of function pointers to implement a jump table, and so on.

    ```c++
    void someFunction(int x) {
        // ...
    }

    int main() {
        void (*pFunc)(int) = someFunction;  // 将函数的地址赋值给函数指针
        pFunc(10);  // 使用函数指针调用函数
        return 0;
    }
    ```

6. Assert

    This line of code is an assertion. It serves to check if the expression inside the parentheses is true. If it is true , the program proceeds normally. If it is false, the program stops here and prints an error message.

    ```c++
    assert(expression);
    ```

7. Functor

    Functors are objects that can be used like functions. They differ from regular functions in that they are a class or structure that overrides the function call operator (operator()). This allows you to use an object of this class as if it were a function.

    ```c++
    #include <iostream>

    class Functor {
    private:
        int count;
    public:
        Functor() : count(0) {}

        // Überladung des Funktionsaufrufsoperators
        void operator()(int x) {
            count += x;
            std::cout << "Summe: " << count << std::endl;
        }
    };

    int main() {
        Functor functor;
        functor(10); // Aufruf des Funktors wie eine Funktion
        return 0;
    }
    ```
