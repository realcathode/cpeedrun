### **1. Struct Basics**
- A `struct` is a user-defined data type that groups variables under a single name.  
**Syntax**:
```c
struct Person {
    char name[50];
    int age;
    float height;
};
```

**Usage**:
```c
struct Person p1; // Declare a struct variable
strcpy(p1.name, "Alice");
p1.age = 30;
p1.height = 5.8;
```

---

### **2. The Arrow Operator (`->`)**
- Used to access struct members **via a pointer** to the struct.
- Equivalent to `(*ptr).member`.
**Example**:
```c
struct Person *ptr = &p1;
printf("%s", ptr->name); // Same as (*ptr).name
```
---
### **3. Memory Allocation for Structs**
#### **Stack vs. Heap**:
- **Stack**: Automatic allocation (fast, limited size).
```c
struct Person p2; // Stack-allocated
```

- **Heap**: Manual allocation using `malloc`/`free` (flexible, larger size).
```c
struct Person *p3 = malloc(sizeof(struct Person));
free(p3); // Don’t forget this!
```

#### **Dynamic Allocation for Nested Structs**:
```c
struct Address {
    char city[50];
    int zip;
};

struct Employee {
    char name[50];
    struct Address addr; // Struct inside struct
    struct Address *addr_ptr; // Pointer to nested struct
};

// Allocate the Employee and its nested Address pointer
struct Employee *e = malloc(sizeof(struct Employee));
e->addr_ptr = malloc(sizeof(struct Address)); // Allocate nested struct

// Free nested memory first!
free(e->addr_ptr);
free(e);
```

---
### **4. Struct Padding and Alignment**
Structs are padded to align members with memory boundaries for efficiency.  
**Example**:

```c
struct Example {
    char c;   // 1 byte
    // 3 bytes padding (assuming 4-byte alignment for int)
    int i;    // 4 bytes
};
printf("%zu", sizeof(struct Example)); // Output: 8 (1 + 3 + 4)
```

---
### **5. Structs in Structs (Nested Structs)**
**Direct Nesting**
```c
struct Date {
    int day, month, year;
};

struct Student {
    char name[50];
    struct Date birthday; // Nested struct
};
```

**Pointer Nesting**:
```c
struct Node {
    int data;
    struct Node *next; // Pointer to another struct (linked list)
};
```

---
### **6. Typedef with Structs**
Simplify struct declarations with `typedef`:
```c
typedef struct Car {
    char model[50];
    int year;
} Car;

Car c1; // No need to write "struct Car"
```

---
### **7. Advanced Example: Struct with Flexible Array Member (C99)**
A struct can end with a flexible array (size not specified):
```c
struct DynamicArray {
    int length;
    int data[]; // Flexible array member (must be last)
};

// Allocation:
int size = 10;
struct DynamicArray *arr = malloc(sizeof(struct DynamicArray) + size * sizeof(int));
arr->length = size;
arr->data[0] = 100; // Access like a normal array
```

---

### **8. Common Pitfalls**

#### **Pitfall 1: Forgetting to Allocate Nested Pointers**
```c
struct Employee *e = malloc(sizeof(struct Employee));
e->addr_ptr->city = "Paris"; // CRASH! addr_ptr is uninitialized.
```
**Fix**: Allocate the nested struct first.

#### **Pitfall 2: Shallow Copying Structs**
```c
struct Person p1 = {"Alice", 30};
struct Person p2 = p1; // Copies all members (including arrays)
strcpy(p2.name, "Bob"); // p1.name remains "Alice"
```

**But for pointers**:
```c
struct Data {
    int *array;
};

struct Data d1;
d1.array = malloc(5 * sizeof(int));
struct Data d2 = d1; // Shallow copy: d2.array points to the same memory!
free(d1.array);      // d2.array is now a dangling pointer.
```

#### **Pitfall 3: Returning Structs from Functions**
Returning large structs by value is inefficient. Use pointers instead:
```c
struct Person* createPerson() {
    struct Person *p = malloc(sizeof(struct Person));
    return p; // Caller must free this!
}
```

---

### **9. Tricky Code Snippets**

> [!question]- **Q1: What does this print?**
> - **Output**: `2 2`
> - **Explanation**: `ptr->x++` increments `p1.x` from 1 to 2. `ptr->y` accesses `p1.y`.

```c
typedef struct {
    int x;
    int y;
} Point;

Point p1 = {1, 2};
Point *ptr = &p1;
ptr->x++;
printf("%d %d", p1.x, ptr->y);
```

---

> [!question]- **Q2: Is this code valid?**
> - **Yes**: `B` is defined inside `A`, and `inner` is a member of `A`.`

```c
struct A {
    int a;
    struct B {
        int b;
    } inner;
};

struct A a;
a.inner.b = 5; // Valid?
```

---
> [!question]- **Q3: Memory Leak?**
> - **Yes**: `d->arr` is not freed. Fix:
>  free(d->arr);
>  free(d);

```c
struct Data {
    int *arr;
};

struct Data *d = malloc(sizeof(struct Data));
d->arr = malloc(10 * sizeof(int));
free(d);
```

---

### **10. Advanced: Function Pointers in Structs**
Structs can contain function pointers for OOP-like behavior:
```c
typedef struct {
    int (*add)(int, int); // Function pointer member
} Calculator;

int sum(int a, int b) { return a + b; }

int main() {
    Calculator calc;
    calc.add = sum; // Assign function to pointer
    printf("%d", calc.add(3, 5)); // Output: 8
}
```

---

### **Key Takeaways**
1. **Arrow vs. Dot**: Use `->` for pointers, `.` for direct struct access.
2. **Allocation**: Always free nested pointers before the parent struct.
3. **Padding**: `sizeof(struct)` includes padding; use `#pragma pack` to control alignment (advanced).
4. **Flexible Arrays**: Use for dynamically sized structs (C99+).
---

**Final Exam-Style Questions:**

**Q1**: What is wrong here?
```c
struct Book {
    char *title;
};

struct Book b;
strcpy(b.title, "C Programming"); // Crash!
```

**Q2**: Predict the output:
```c
struct Test {
    short a;
    char b;
    int c;
};
printf("%zu", sizeof(struct Test)); // Assume 4-byte alignment.
```

**Q3**: How to fix this code?
```c
struct Node {
    int data;
    struct Node next; // Error?
};
```
