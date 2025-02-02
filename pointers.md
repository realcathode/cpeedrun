### **1. Pointer Basics**
-  A pointer is a variable that stores the memory address of another variable.  

**Syntax**:
```c
int x = 10;
int *ptr = &x; // ptr holds the address of x
```

**Usage**:
```c
printf("%p", (void*)ptr); // Prints the address of x
printf("%d", *ptr);       // Dereference: prints 10
```

---
### **2. Pointer Arithmetic**
- **Rules**:
    - `ptr + n` advances by `n * sizeof(type)` bytes.
    - Subtracting pointers gives the number of elements between them.
    - `void*` pointers cannot be used in arithmetic (cast first).

**Example**:
```c
int arr[] = {10, 20, 30, 40};
int *p = arr; // Points to arr[0]
p++;          // Now points to arr[1]
printf("%d", *p); // Output: 20
```

---
### **3. Pointers and Arrays**
- **Array Decay**: Arrays decay to pointers when passed to functions.
- **Accessing Elements**:
```c
int arr[3] = {1, 2, 3};
printf("%d", *(arr + 1)); // Output: 2 (same as arr[1])
```

**Example**:
```c
void printArray(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]); // arr[i] ≡ *(arr + i)
    }
}
```

---
### **4. Pointer to Pointer**
```c
int x = 5;
int *p = &x;
int **pp = &p; // Pointer to a pointer
printf("%d", **pp); // Output: 5
```
---
### **5. Function Pointers**
```c
int (*funcPtr)(int, int); // Declare a function pointer
funcPtr = &add;           // Assign to a function
int result = funcPtr(3, 5); // Call via pointer
```

**Example**:
```c
#include <stdio.h>
int add(int a, int b) { return a + b; }
int main() {
    int (*func)(int, int) = add;
    printf("%d", func(2, 3)); // Output: 5
}
```

---
### **6. Memory Allocation with Pointers**
#### **Dynamic Allocation**:
```c
int *arr = malloc(5 * sizeof(int)); // Heap allocation
arr[0] = 10;
free(arr); // Always free!
```
#### **Common Functions**:
- `malloc`: Allocates uninitialized memory.
- `calloc`: Allocates zero-initialized memory.
- `realloc`: Resizes existing memory block.

**Example**:
```c
int *arr = calloc(5, sizeof(int)); // Allocates and initializes to 0
arr = realloc(arr, 10 * sizeof(int)); // Resize to 10 elements
```

---
### **7. Pointer Arithmetic with Structs**
```c
typedef struct {
    int x;
    char c;
} MyStruct;

MyStruct s[2] = {{1, 'A'}, {2, 'B'}};
MyStruct *ptr = s;
ptr++;
printf("%c", ptr->c); // Output: 'B'
```

---
### **8. Void Pointers (Generic Pointers)**
- Can hold any address but cannot be dereferenced directly.
- Must be cast to a specific type first.

**Example**:
```c
int x = 10;
void *vp = &x;
int *ip = (int*)vp;
printf("%d", *ip); // Output: 10
```

---
### **9. Common Pitfalls**
#### **Pitfall 1: Dereferencing NULL/Invalid Pointers**
```c
int *p = NULL;
printf("%d", *p); // Crash: dereferencing NULL
```
#### **Pitfall 2: Memory Leaks**
```c
int *arr = malloc(10 * sizeof(int));
// Forgot to free(arr); → Memory leak!
```
#### **Pitfall 3: Pointer Arithmetic Out of Bounds**
```c
int arr[3] = {1, 2, 3};
int *p = arr + 5; // Undefined behavior!
```

---

### **10. Tricky Code Snippets**

> [!question]- **Q1: What does this print?**
> - **Output**: `5`
>     
> - **Explanation**: `&a` is a pointer to the entire array. `&a + 1` points past the array. `p - 1` points to the last element.

```c
int a[] = {1, 2, 3, 4, 5};
int *p = (int*)(&a + 1);
printf("%d", *(p - 1)); 
```

---

> [!question]- **Q2: Identify the bug**
> - **Bug**: Modifying a string literal (stored in read-only memory).

```c
char *str = "Hello";
str[0] = 'h'; // Crash!
```

---

> [!question]- **Q3: Predict the output**
> - **Output**: `8` (assuming 64-bit system; size of a pointer, not the data it points to).


```c
int x = 10;
int *p = &x;
printf("%d", sizeof(p));
```

---

### **11. Advanced: Pointers and Multidimensional Arrays**

```c
int arr[2][3] = {{1, 2, 3}, {4, 5, 6}};
int (*ptr)[3] = arr; // Pointer to an array of 3 integers
printf("%d", ptr[1][2]); // Output: 6
```

---

### **12. Key Takeaways**
1. **Pointer Arithmetic**: Always scale by `sizeof(type)`.
2. **Memory Management**: Always pair `malloc` with `free`.
3. **Arrays vs. Pointers**: Arrays decay to pointers, but `sizeof` behaves differently.
4. **Undefined Behavior**: Dangling pointers, out-of-bounds access, and invalid casts.

---

**Final Exam-Style Questions**:

**Q1**: What is the output?
```c
int x = 5;
int *p = &x;
int **pp = &p;
**pp = 10;
printf("%d", x);
```

**Q2**: Fix this code:
```c
char *str = malloc(10);
strcpy(str, "Hello");
free(str);
printf("%s", str); // dangling
```

**Q3**: Predict the output (assume little-endian):
```c
int num = 0x12345678;
char *c = (char*)#
printf("%02x", *c);
```

> [!question]- Answers
> - **Q1**: `10` (modifying `x` via double pointer).
> - **Q2**: Remove `printf` after `free` (dangling pointer).
> - **Q3**: `78` (little-endian stores least significant byte first).
