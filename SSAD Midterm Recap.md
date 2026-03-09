## Week 1: C++ type system; references & constants types

### Study Points for Week 1 Material

1.  **Memorize the Reference Rules:** Know exactly what is illegal (e.g., null references, arrays of references, pointer to reference).
2.  **Stack vs. Heap:** Be able to identify where an object lives based on how it is declared (`static`, `new`, local variable).
3.  **Const Casting:** Understand the difference between `const T*`, `T* const`, and `const T* const`.
4.  **Array Decay:** Remember that an array name acts as a constant pointer to the first element.
5.  **Vector Syntax:** Know how to declare, initialize, and iterate over a `std::vector`.
6.  **Namespace Clashes:** Understand why namespaces exist (to allow same variable names in different scopes).

### C++ Memory Model
*   **Three kinds of memory:**
	1.  **Program Memory:** Contains machine code instructions. Cannot be modified by the program.
	2.  **Stack:** Discipline defined by static program structure. Used for local variables, function arguments, and static objects.
	3.  **Heap (Dynamic Memory):** Discipline defined by runtime semantics. Used for dynamic objects (`new`/`delete`).
- **Comment:**
	- Talk about System stack (AR, local var's)
	- Differents betwen local and dynamic:
		- Scope

### Type System
*   **Definition of Type:**
	* A set of values, a set of operators, and a set of relationships with other types.
*   **Classification:**
	*   **Fundamental Types:** Integers, Floating-point, Characters, `bool`, `void`.
	*   **User-defined Types:** Classes, Structures, Unions, Enumerations.
	*   **Compound/Modified Types:** Arrays, Pointers, References, Functions!!!, Constants.

### References vs. Pointers

| Feature | Pointers (`T*`) | References (`T&`) |
| :--- | :--- | :--- |
| **Status** | Objects (occupy memory) | Synonyms (do not occupy memory) |
| **Initialization** | Can be uninitialized (null) | **Must be initialized** upon declaration |
| **Reassignment** | Can point to different objects | Cannot be reseated (always refers to initial object) |
| **Syntax** | Explicit dereference (`*p`) | Implicit access (`r = 5`) |
| **Nullability** | Can be `nullptr` | **No null references** |
| **Arrays** | Arrays of pointers allowed | **No arrays of references** |
| **Pointer to** | Pointer to reference is **illegal** | Reference to pointer is allowed (`int*&`) |

### Constant Types (`const`)
*   **Meaning:** `const T` denotes objects of type `T` that cannot change their values.
*   **Variations:**
	*   `const int x`: Constant integer.
	*   `const int* p`: Pointer to a constant integer (cannot modify value via `p`).
	*   `int* const p`: Constant pointer to an integer (cannot change address stored in `p`).
	*   `const int* const p`: Constant pointer to a constant integer.
*   **Constant Expressions:** Only compile-time constants can be used for array sizes or template arguments.

### `auto` Specifier
*   **Function:** Type deduction. The compiler determines the type from the initializer.
*   **Rules:**
	*   `auto x = 7;` → `int`
	*   `auto& x = ref;` → Reference type preserved.
	*   `const auto* p` → Pointer to const.

### Namespaces
*   **Purpose:** Group related declarations, prevent name clashes, structure large programs.
*   **Standard Library:** All standard entities are in the `std` namespace.
*   **Access:**
	*   Qualified name: `std::cout`
	*   Using-declaration: `using std::cout;`
	*   Using-directive: `using namespace std;` (Note: Often discouraged in header files).

### Arrays vs. Vectors
*   **Arrays (`int A[10]`):**
	*   Fixed size (compile-time).
	*   Part of core language.
	*   Name decays to pointer to first element.
	*   Low-level, prone to bugs.
*   **Vectors (`std::vector<int>`):**
	*   Dynamic size (runtime).
	*   Part of Standard Library.
	*   Rich functionality (`push_back`, `size`, etc.).
	*   **Recommendation:** Use vectors instead of arrays.

### Modern C++ Features
*   **Range-based For Loop:** `for (auto& elem : container)`
	*   Use `auto&` to modify elements.
	*   Use `auto` or `const auto&` for read-only access.
*   **Structured Binding:** `auto [x, y] = expression;`
	*   Decomposes arrays, structs, or tuples into individual variables.

## Week 2: Classes without OOP

### Study Points for Week 2 Material

1.  **Constructor Call Chain:** Know exactly when the Default, Copy, or Conversion constructor is triggered.
    *   `C c;` → Default
    *   `C c = 5;` → Conversion + (possibly) Copy (optimized away)
    *   `C c = d;` → Copy
    *   `c = 5;` → Conversion + Assignment
2.  **Initialization Braces `{}`:** Remember they prevent narrowing conversions and avoid the "Most Vexing Parse".
3.  **`new` vs `malloc`:** `new` allocates memory **AND** calls the constructor.
4.  **Destructor Timing:** Static objects are destroyed automatically at scope end; Dynamic objects require `delete`.
5.  **`constexpr` Rules:** Functions must be simple enough for compile-time evaluation. Cannot be used with non-constant variables.
6.  **Access Control:** Default inheritance is `private` for `class`, `public` for `struct` (though struct wasn't explicitly detailed in W2, it's standard C++ knowledge often tested). Default member access in `class` is `private`.
7.  **Conversion Operators:** `operator bool()` allows objects to be used in `if` conditions.

### Classes as User-Defined Types
*   **Definition:** A class is a user-defined compound type that behaves similarly to fundamental types.
*   **Members:**
    *   **Data Members:** Represent the state (object variables).
    *   **Member Functions:** Represent behavior (methods).
    *   **Special Member Functions:** Constructors, Destructor, Operator functions, Conversion functions.
*   **Access Control:**
    *   `private`: Accessible only within the class (default for `class`).
    *   `public`: Accessible from outside the class.
    *   `protected`: (Covered in later weeks, related to inheritance).

### Object Lifecycle & Memory
*   **Creation:**
    *   **Static:** Declared directly (e.g., `Point p;`). Created on **Stack**. Lifetime determined by scope.
    *   **Dynamic:** Created with `new` (e.g., `Point* p = new Point();`). Created on **Heap**. Lifetime until `delete`.
*   **Removal:**
    *   **Static:** Automatically destroyed when scope ends (Destructor called).
    *   **Dynamic:** Must be explicitly destroyed with `delete` (Destructor called).
*   **`new` Operator Actions:**
    1.  Allocates memory on the heap.
    2.  Invokes the constructor for the allocated memory.

### Initialization Forms
*   **Four Forms:**
    1.  `int x = 0;` (Copy initialization)
    2.  `int x(0);` (Direct initialization)
    3.  `int x{0};` (Uniform/Braced initialization)
    4.  `int x = {0};` (Copy list initialization)
*   **Uniform Initialization (`{}`):**
    *   Prevents **narrowing conversions** (e.g., `int x{3.14};` is an **Error**).
    *   Avoids the **"Most Vexing Parse"**: `C c();` is interpreted as a **function declaration**, not an object. Use `C c{};` instead.

### Constructors & Special Member Functions
*   **Constructor:** Called upon object creation.
    *   **Default Constructor:** Takes no arguments (or all arguments have defaults).
    *   **Conversion Constructor:** Takes one argument (allows implicit conversion, e.g., `C c = 5;`).
    *   **Copy Constructor:** `C(const C& other)`. Called when initializing a new object from an existing one.
    *   **Move Constructor:** (Mentioned briefly) Transfers resources.
*   **Destructor (`~C()`):** Called upon object destruction. Releases resources.
*   **Initialization vs. Assignment:**
    *   **Initialization:** Object is being **created** (Constructor called).
    *   **Assignment:** Object **already exists** (Assignment operator `operator=` called).

### Operator Overloading
*   **Purpose:** Allow user-defined types to work with standard operators (`+`, `-`, `[]`, `()`, etc.).
*   **Syntax:** `ReturnType operator OpSign (Parameters) { ... }`
*   **Rules:**
    *   Cannot create new operators.
    *   Arity and precedence cannot change.
    *   Can overload `new`/`delete`, `[]`, `()`, conversion operators.
*   **Conversion Functions:** `operator Type()` allows implicit conversion to another type (e.g., `operator bool()`).

### `constexpr`
*   **Definition:** Indicates that a value or function can be evaluated at **compile-time**.
*   **Requirements:**
    *   Must be non-virtual.
    *   Body should typically contain a single return statement (in older standards).
    *   Arguments/Return types must be literal types.
*   **`const` vs `constexpr`:**
    *   `const`: Runtime constant (usually), cannot change after initialization.
    *   `constexpr`: Compile-time constant. Implies `const` for objects.

### Type Specification Simplification
*   **`typedef`:** Creates a synonym for a type (C-style).
*   **`using`:** Creates a type alias (C++11 style).
    *   Example: `using PtrFun = int(*)(int);`

## Week 3: C++ classes & the basics of OOP

### Study Points for Week 3 Material
1.  **Virtual Dispatch Rule:** Memorize: **Virtual = Dynamic Type**, **Non-Virtual = Static Type**. This solves 50% of output questions.
2.  **Abstract Class Constraint:** You **cannot** create an instance of a class with a pure virtual function (`= 0`). You can only create pointers/references.
3.  **Protected Access:** Understand that `protected` allows access in derived classes but blocks access from outside (unlike `public`).
4.  **Hiding vs. Overriding:** If signatures match but `virtual` is missing in base, it's **hiding**, not polymorphism.
5.  **Virtual Inheritance:** Know that it prevents multiple copies of the base class in diamond inheritance structures.
6.  **`override` Keyword:** It is optional but recommended. It causes a compile error if the base function isn't virtual or signatures don't match.
7.  **Constructor Order:** Base class constructors are called **before** Derived class constructors. Destructors are called in **reverse** order.

### Special Member Functions (Compiler-Generated)
*   **Rule:** If not provided by the programmer, the compiler generates default versions for:
    *   Default Constructor
    *   Copy Constructor
    *   Move Constructor (C++11)
    *   Copy Assignment Operator
    *   Move Assignment Operator (C++11)
    *   Destructor
*   **Member-wise Principle:** These default functions operate on base class subobjects and class members individually.

### Instance vs. Class Members (`static`)
*   **Instance Members:** Unique to each object (e.g., `int x;`). Memory allocated per object.
*   **Class Members (`static`):** Shared across all instances of the class (e.g., `static int count;`).
    *   **Memory:** Allocated once (in data segment), independent of objects.
    *   **Access:** Via class name (`Class::member`) or object instance.
    *   **Static Functions:** Cannot access non-static members directly (no `this` pointer).

### Inheritance Basics
*   **"Is-a" Relationship:** Derived class is a specialized version of the Base class.
*   **Subobject:** A derived class object contains a base class subobject.
*   **Access Specifiers in Inheritance:**
    *   `public`: Base public → Derived public; Base protected → Derived protected.
    *   `protected`: Base public/protected → Derived protected.
    *   `private`: Base public/protected → Derived private.
    *   **Note:** Base `private` members are **never accessible** in Derived class.
*   **`protected` Keyword:** Members accessible within the class and **derived classes**, but not from outside.

### Method Overriding vs. Hiding
*   **Hiding (Non-Virtual):** If a derived class defines a function with the same name as a base class (non-virtual) function, it **hides** the base version. Call depends on **Static Type**.
*   **Overriding (Virtual):** If the base function is `virtual`, the derived function **overrides** it. Call depends on **Dynamic Type**.
*   **`override` Keyword:** Ensures at compile-time that the function actually overrides a virtual base function.

### Static vs. Dynamic Types
*   **Static Type:** The type declared in the code (e.g., `Base* ptr`). Determined at compile-time.
*   **Dynamic Type:** The type of the actual object in memory (e.g., `Derived` object). Determined at runtime.
*   **Polymorphism Rule:**
    *   **Virtual functions:** Dispatched based on **Dynamic Type**.
    *   **Non-virtual functions:** Dispatched based on **Static Type**.

### Multiple & Virtual Inheritance
*   **Multiple Inheritance:** A class inherits from more than one base class (`class D : public B1, public B2`).
*   **Diamond Problem:** Ambiguity when two base classes inherit from the same grandparent. Results in duplicate subobjects.
*   **Virtual Inheritance:** Solves the Diamond Problem. Ensures only **one copy** of the base class subobject exists.
    *   Syntax: `class B : virtual public A`

### Abstract Classes & Pure Virtual Functions
*   **Pure Virtual Function:** Declared with `= 0` (e.g., `virtual void f() = 0;`).
*   **Abstract Class:** A class containing **at least one** pure virtual function.
*   **Restriction:** **Cannot instantiate** objects of an abstract class (e.g., `new Abstract()` is an error).
*   **Usage:** Pointers/references to abstract classes can point to concrete derived objects (Polymorphism).

## Week 4: C++ classes (cont)

### Study Points for Week 4 Material
1.  **Const Correctness**: Memorize: **`const` object → can only call `const` methods**. This is a common compilation error trap.
2.  **Initializer List Requirements**: `const` members, reference members, and base classes without default constructors **MUST** use initializer lists.
3.  **`= delete` vs. `private`**: `delete` gives **compile-time error** (clearer intent), `private` gives **linker error** (if called from outside).
4.  **`this` in Static Functions**: Static functions **do not have** `this` pointer (they belong to the class, not an instance).
5.  **Delegating Constructor Cycles**: Watch for circular delegation (A→B→A), which causes compilation errors.
6.  **Default Constructor Generation**: If you define **any** constructor, the compiler **does not** generate a default one (unless you use `= default`).
7.  **Const Overloading**: You can have `void f()` and `void f() const` as two different functions (overloaded by `this` pointer type).

### Deleted and Defaulted Functions (C++11)
*   **`= default`**: Explicitly request compiler-generated implementation.
    *   Useful when you define other constructors but still want a default constructor.
    *   Example: `ClassName() = default;`
*   **`= delete`**: Explicitly prohibit a function.
    *   Causes **compile-time error** if called.
    *   Used to make classes **non-copyable** or prevent certain conversions.
    *   Example: `ClassName(const ClassName&) = delete;`

### Initializing Bases & Members
*   **Member Initializer List**: Syntax `Constructor() : Base(arg), member(val) { }`
    *   **Required** for:
        *   `const` members (cannot be assigned, must be initialized)
        *   Reference members (cannot be reassigned)
        *   Base classes without default constructors
        *   Classes without default constructors
*   **Initialization vs. Assignment**:
    *   **Initialization** (in initializer list): Object is created with value.
    *   **Assignment** (in constructor body): Object is created (default), then value is changed.

### Delegating Constructors
*   **Purpose**: One constructor calls another constructor of the **same class** to avoid code duplication.
*   **Syntax**: `Constructor() : Constructor(args) { specific-actions }`
*   **Rules**:
    *   Must be in the **initializer list**.
    *   Cannot create **cycles/recursion** (e.g., A calls B, B calls A).
    *   The delegating constructor's body executes **after** the target constructor completes.

### `this` Pointer
*   **Definition**: A hidden pointer passed to every **non-static** member function.
*   **Type**: `ClassName* const` (constant pointer to the object).
*   **For `const` member functions**: Type is `const ClassName* const` (cannot modify object).
*   **Usage**:
    *   Access members: `this->member` ≡ `member`
    *   Return current object: `return *this;` (for chaining)
    *   **Not available** in static member functions (no object instance).

### Constant Member Functions
*   **Syntax**: `ReturnType FunctionName() const { }`
*   **Meaning**: Promises not to modify the object's state.
*   **Rules**:
    *   Can be called on **both** `const` and non-`const` objects.
    *   Non-`const` member functions **cannot** be called on `const` objects.
    *   Can overload based on `const`-ness (two versions of same function).

### Function Declaration vs. Definition
*   **Declaration**: Signature only (in header file `.h`). Tells compiler the function exists.
    *   Example: `int f(int x);`
*   **Definition**: Signature + Body (in source file `.cpp`). Actual implementation.
    *   Example: `int f(int x) { return x; }`
*   **Member Functions**: Can be defined **inline** (inside class) or **outside** (using `ClassName::FunctionName`).

## Week 5: C++ templates

### Study Points for Week 5 Material
1.  **Smart Pointer Selection:**
    *   **`unique_ptr`** → Single ownership, move-only
    *   **`shared_ptr`** → Multiple ownership, reference counting
    *   **`weak_ptr`** → Break circular references, observe without owning
2.  **Template Instantiation:** Compiler deduces types from function arguments (not return type). Class templates require explicit type specification.
3.  **Specialization Priority:** Specialized version is called when types match exactly; generic template is fallback.
4.  **Type Requirements:** If template uses `operator>`, the actual type must implement it (or compilation fails).
5.  **`std::move`:** Used to transfer ownership of `unique_ptr`.
6.  **RAII Guarantee:** Destructor automatically releases resources when smart pointer goes out of scope.
7.  **Circular Reference:** `shared_ptr` ↔ `shared_ptr` = memory leak. Solution: `shared_ptr` ↔ `weak_ptr`.
8.  **Non-Type Parameters:** Must be compile-time constants (e.g., `int N`, not runtime variables).

### Function Templates
*   **Purpose:** Define a family of functions that work with multiple types without code duplication.
*   **Syntax:**
    ```cpp
    template<typename T>
    T Max(T a, T b) { return a > b ? a : b; }
    ```
*   **Template Instantiation:** Compiler generates specific function versions based on actual argument types.
    *   `Max(1, 2)` → `Max<int>`
    *   `Max(1.0, 2.0)` → `Max<double>`
*   **Requirements on Types:** Actual types must support all operations used in the template (e.g., `operator>` for `Max`).
*   **Explicit Instantiation:** Required when compiler cannot deduce types (e.g., `spaceOf<int>()`).

### Class Templates
*   **Purpose:** Define a family of classes that work with multiple types (e.g., `Stack<T>`, `vector<T>`).
*   **Syntax:**
    ```cpp
    template<typename T>
    class Stack { T S[100]; void push(T v); T pop(); };
    ```
*   **Instantiation:** Must explicitly specify type parameters: `Stack<int> s;`
*   **Non-Type Parameters:** Can parameterize with values (e.g., array size):
    ```cpp
    template<typename T, int N>
    class Stack { T S[N]; };
    Stack<int, 10> s;
    ```
*   **Requirements on Actual Types:** Must have copy constructor and assignment operator (for pass-by-value).

### Template Specialization
*   **Purpose:** Provide different implementation for specific types.
*   **Syntax:**
    ```cpp
    template<typename T, typename N>
	int fun(T a, N b) { return a + b; }

    template<>
    int fun<int*, int*>(int* a, int* b) { return *a + *b; }
    ```
*   **When to Use:** When generic implementation is inefficient or incorrect for specific types.

### Smart Pointers (RAII Pattern)
*   **Purpose:** Automatic memory management, prevent memory leaks and dangling pointers.
*   **RAII:** Resource Acquisition Is Initialization – resources released when object goes out of scope.

| Pointer Type          | Ownership                    | Key Features                                                      |
| :-------------------- | :--------------------------- | :---------------------------------------------------------------- |
| **`unique_ptr`**      | **Exclusive** (one owner)    | Cannot copy, can move (`std::move`), deleted copy ctor            |
| **`shared_ptr`**      | **Shared** (multiple owners) | Reference counting (ARC), destroyed when count = 0                |
| **`weak_ptr`**        | **Non-owning**               | Observes `shared_ptr`, doesn't affect count, solves circular refs |
| **Raw Pointer (`*`)** | **Manual**                   | No automatic management, prone to leaks                           |

| Operation       | `unique_ptr`    | `shared_ptr`        | `weak_ptr`                 |
| :-------------- | :-------------- | :------------------ | :------------------------- |
| **Copy**        | ❌ Deleted       | ✅ (increases count) | ✅ (doesn't increase count) |
| **Move**        | ✅ (`std::move`) | ✅                   | ✅                          |
| **Dereference** | ✅ (`*`, `->`)   | ✅ (`*`, `->`)       | ❌ (must use `lock()`)      |
| **Get Raw**     | ✅ (`.get()`)    | ✅ (`.get()`)        | ✅ (`.lock()`)              |
| **Ownership**   | Exclusive       | Shared              | None (observer)            |

### Circular Reference Problem
*   **Problem:** Two `shared_ptr` objects pointing to each other prevent destruction (reference count never reaches 0).
*   **Solution:** Use `weak_ptr` for one direction of the relationship.

### Template Code Bloat
*   **Problem:** Each template instantiation generates separate code, increasing executable size.
*   **Cause:** Separate compilation with templates in header files can create multiple copies.

## Week 6: C++ templates & generic programming; adapters Lambdas & functional programming

### Study Points for Week 6 Material
1.  **Specialization Priority:** Explicit specialization is chosen over primary template when types match exactly.
2.  **Lambda Capture Semantics:** 
    *   `[=]` creates **immutable copy** (need `mutable` to modify)
    *   `[&]` creates **reference** (modifications affect original)
3.  **Function Template Deduction:** Return type is **NOT** used for deduction – only argument types.
4.  **Partial Specialization:** Only for **class templates**, not function templates.
5.  **Smart Pointer Ownership:** 
    *   `unique_ptr` = one owner (copy deleted)
    *   `shared_ptr` = reference counting
    *   `weak_ptr` = no ownership (breaks cycles)
6.  **Functor Advantages:** State + Inlining + Type safety (vs function pointers)
7.  **Non-Type Parameters:** Must be **compile-time constants** (e.g., `template<int N>`)
8.  **Template Template Parameters:** Syntax requires `template<template<typename> class T>`

### Function Template Instantiation Types
| Type | Description | Example |
| :--- | :--- | :--- |
| **Complete Explicit** | All template arguments specified | `Max<int, float>(a, b)` |
| **Incomplete Explicit** | Some arguments specified, others deduced | `Max<int>(a, b)` |
| **Implicit** | All arguments deduced from call | `Max(a, b)` |
| **Important Rule** | Return type is **NOT** considered for deduction | `T Max(T a, T b)` – T deduced from arguments only |

### Template Specialization
*   **Explicit Specialization:** Different implementation for **specific type(s)**.
    *   Syntax: `template<> ReturnType Function<SpecificType>(...)`
    *   Empty angle brackets `<>` required
    *   Example: `template<> int fun<int, double>(int, double)`
*   **Partial Specialization:** Different implementation for **subset of types** (e.g., all pointers).
    *   Syntax: `template<typename T> class C<T*>`
    *   **Only for class templates** (not function templates directly)
*   **Priority:** Specialization > Primary Template

### Template Parameters
| Parameter Type | Description | Example |
| :--- | :--- | :--- |
| **Type Parameter** | Actual argument is a type | `template<typename T>` |
| **Non-Type Parameter** | Actual argument is a constant value | `template<int N>` |
| **Template Template** | Actual argument is a template | `template<template<typename> class C>` |

*   **Default Arguments:** Can be provided for template parameters (must be from right to left)

### Functional Objects (Functors)
*   **Definition:** Class/object with **`operator()`** defined
*   **Usage:** Can be called like a function: `obj(args)` ≡ `obj.operator()(args)`
*   **Advantages over Function Pointers:**
    *   Can maintain **state** (member variables)
    *   Can be **inlined** (better performance)
    *   Type-safe

### Lambda Expressions
*   **Syntax:** `[capture](parameters) -> returnType { body }`
*   **Capture List:**

| Capture Syntax | Variable Access | Can Modify? | Needs `mutable`? |
| :--- | :--- | :--- | :--- |
| `[]` | Not accessible | N/A | N/A |
| `[x]` | By value (copy) | ❌ No | ✅ Yes |
| `[&x]` | By reference | ✅ Yes | ❌ No |
| `[=]` | All by value | ❌ No | ✅ Yes |
| `[&]` | All by reference | ✅ Yes | ❌ No |
| `[=, &x]` | All by value, x by ref | x: ✅ Yes | ❌ No (for x) |

*   **`mutable` Keyword:** Allows modification of captured-by-value variables inside lambda
*   **Return Type:** Deduced automatically if single return statement; otherwise specify explicitly

### Template Adapters
*   **Purpose:** Build complex functionality from simpler components
*   **Example:** Comparators (`Greater<T, N>`, `Less<T, N>`) passed to algorithms
*   **STL Connection:** `std::sort`, `std::find_if` accept functional objects/lambdas

## Week 7: C++ stand. library; the notion of iterator, iterator examples

### Study Points for Week 7 Material
1.  **Iterator Hierarchy:** Memorize the capabilities of each category (Input < Forward < Bidirectional < Random Access).
2.  **Container-Iterator Mapping:**
    *   `vector` → Random Access
    *   `list` → Bidirectional
    *   `forward_list` → Forward
3.  **Algorithm Constraints:** Know which algorithms require which iterators (e.g., `sort` needs Random Access, `reverse` needs Bidirectional).
4.  **SOLID Acronym:** Know what each letter stands for and the basic definition.
5.  **Liskov Substitution:** Understand that Derived classes must not strengthen preconditions or weaken postconditions of the Base class.
6.  **Dependency Inversion:** Prefer pointers/references to **base classes/interfaces** over concrete derived classes in high-level logic.
7.  **GP vs OOP:** GP focuses on algorithms/types decoupling (Iterators); OOP focuses on encapsulation/inheritance. STL uses GP principles within C++ (OOP language).

### Generic Programming (GP) & STL
*   **Generic Programming:** Approach focusing on **generality** and **efficiency**. Algorithms are decoupled from data structures.
*   **STL Components:**
    1.  **Containers:** Data structures (`vector`, `list`, `map`, etc.).
    2.  **Algorithms:** Functions operating on containers (`sort`, `find`, `reverse`).
    3.  **Iterators:** The **interface** between containers and algorithms.
    4.  **Functors/Adaptors/Allocators:** Supporting components.
*   **Key Idea:** Algorithms do **not** know about containers; they only know about **iterators**.

### Iterators
*   **Definition:** Objects that provide access to elements of a container (generalization of pointers).
*   **Requirements:** Must support specific operators depending on category (e.g., `*`, `++`, `!=`, `--`, `+n`).
*   **Iterator Categories:**

| Category          | Capabilities                                 | Typical Container        |
| :---------------- | :------------------------------------------- | :----------------------- |
| **Input**         | Read, single pass (`*`, `++`, `!=`)          | `istream`                |
| **Output**        | Write, single pass (`*`, `++`)               | `ostream`                |
| **Forward**       | Read/Write, multi-pass (`*`, `++`, `!=`)     | `forward_list`           |
| **Bidirectional** | Forward + Backward (`--`)                    | `list`, `set`, `map`     |
| **Random Access** | Bidirectional + Arithmetic (`+n`, `[]`, `<`) | `vector`, `deque`, array |

### SOLID Principles
*   **S - Single Responsibility:** A class should have only one reason to change (one task).
*   **O - Open/Closed:** Classes should be **open for extension** (inheritance/polymorphism) but **closed for modification**.
*   **L - Liskov Substitution:** Objects of a derived class must be substitutable for objects of the base class without breaking the program.
*   **I - Interface Segregation:** Clients shouldn't depend on methods they don't use (split large interfaces).
*   **D - Dependency Inversion:** High-level modules shouldn't depend on low-level modules; both should depend on **abstractions** (interfaces/abstract classes).

### Algorithm Genericity
*   **Evolution of `find`:**
    1.  Array-specific (`int*`, size).
    2.  Template type (`T*`).
    3.  Iterator-based (`Iterator first`, `Iterator beyond`).
*   **Benefit:** Same algorithm works for arrays, vectors, lists, etc., as long as iterators meet requirements.

## Final Midterm Prep Summary (Weeks 1-7 Combined)

| Rank | Topic                                 | Weeks | Why                                                                   |
| :--- | :------------------------------------ | :---- | :-------------------------------------------------------------------- |
| 1    | **Virtual vs. Non-Virtual Functions** | 3, 4  | Polymorphism rules (Static vs Dynamic type) are tested every year.    |
| 2    | **Smart Pointers**                    | 5, 6  | `unique_ptr`, `shared_ptr`, `weak_ptr` semantics (ownership, cycles). |
| 3    | **Template Specialization**           | 5, 6  | Which version is called (Primary vs Explicit vs Partial).             |
| 4    | **Constructors & Initialization**     | 2, 4  | Copy vs Move vs Conversion, Initializer Lists, `explicit`.            |
| 5    | **References vs. Pointers**           | 1, 2  | Rules, nullability, syntax, pass-by-ref vs pass-by-val.               |
| 6    | **Const Correctness**                 | 1, 4  | `const` objects, `const` methods, `constexpr`.                        |
| 7    | **Inheritance Access Rules**          | 3, 4  | `public`, `protected`, `private` inheritance visibility.              |
| 8    | **Iterator Categories**               | 7     | Matching iterators to containers/algorithms (`list` vs `vector`).     |
| 9    | **Abstract Classes**                  | 3     | Pure virtual functions (`= 0`), instantiation rules.                  |
| 10   | **Memory Model**                      | 1, 2  | Stack vs Heap, `new`/`delete`, object lifetime.                       |
| 11   | **Lambda Capture**                    | 6     | `[=]`, `[&]`, `mutable`, variable scope.                              |
| 12   | **Static Members**                    | 3, 4  | Memory allocation, access via class vs object.                        |
| 13   | **Operator Overloading**              | 2, 4  | Syntax, conversion operators, `operator()`.                           |
| 14   | **SOLID/OOP Design**                  | 3, 7  | Principles behind inheritance and interface design.                   |
| 15   | **Namespace & Arrays**                | 1     | `std::`, array decay, `vector` vs raw arrays.                         |
