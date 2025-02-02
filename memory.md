### **1. Memory Management Basics**
- C requires manual memory management using stack and heap allocation.  
**Key Concepts**:
- **Stack**: Automatic allocation (function-local variables). Fast but limited size.
- **Heap**: Manual allocation (`malloc`, `calloc`, `realloc`, `free`). Flexible but error-prone.
- **Static/Global**: Allocated at compile-time (persists for program lifetime).

**Example**:
```c
int stack_var;           // Stack
static int static_var;   // Static/global
int *heap_var = malloc(sizeof(int)); // Heap
```

---
### **2. Memory Allocation Functions**
#### **`malloc`**: Allocates uninitialized memory.
```c
int *arr = malloc(5 * sizeof(int)); // 5 integers (uninitialized)
```

#### **`calloc`**: Allocates zero-initialized memory.
```c
int *arr = calloc(5, sizeof(int)); // 5 integers (all 0s)
```

#### **`realloc`**: Resizes existing memory block.
```c
arr = realloc(arr, 10 * sizeof(int)); // Expand to 10 elements
```

#### **`free`**: Releases allocated memory.
```c
free(arr); // Always pair with malloc/calloc/realloc
```

---

### **3. Common Pitfalls**
#### **Pitfall 1: Memory Leaks**
```c
void leak() {
    int *arr = malloc(100 * sizeof(int));
    // Forgot to free(arr)!
}
```

#### **Pitfall 2: Dangling Pointers**
```c
int *p = malloc(sizeof(int));
free(p);
*p = 10; // UB: Writing to freed memory
```

#### **Pitfall 3: Double Free**
```c
int *p = malloc(sizeof(int));
free(p);
free(p); // UB: Freeing already freed memory
```

#### **Pitfall 4: Buffer Overflow**
```c
int arr[5];
arr[5] = 10; // Out-of-bounds access (UB)
```

---
### **4. Best Practices**
- **Check Allocation Success**:
```c
int *arr = malloc(100 * sizeof(int));
if (!arr) { /* Handle error */ }
```
- **Zeroing Freed Pointers**:
```c
free(arr);
arr = NULL; // Prevents dangling pointers
```
- **Use `sizeof` Correctly**:
```c
int *arr = malloc(10 * sizeof *arr); // Safe even if type changes
```

---
### **5. Memory Operations**
#### **`memcpy` vs `memmove`**:
- `memcpy`: Fast copy (no overlap allowed).
- `memmove`: Safe for overlapping regions.
```c
char src[] = "ABCDEF";
memmove(src + 2, src, 5); // OK: "ABABCF"
// memcpy(src + 2, src, 5); // UB!
```

#### **`memset`**: Initialize memory block.
```c
char buf[10];
memset(buf, 0, sizeof(buf)); // Zero-initialize
```

---
### **6. Advanced Topics**
#### **Memory Alignment**:
Use `alignas` (C11) or compiler-specific attributes:
```c
#include <stdalign.h>
alignas(16) int arr[4]; // 16-byte aligned
```

#### **Custom Allocators**:
```c
typedef struct {
    char *buffer;
    size_t offset;
} Arena;

void *arena_alloc(Arena *a, size_t size) {
    void *ptr = a->buffer + a->offset;
    a->offset += size;
    return ptr;
}
```

---

### **7. Tricky Code Snippets**
> [!question]- **Q1: What's wrong here?**
> - **Out-of-bounds access**: Valid indices are 0–4. UB!

```c
int *p = malloc(5 * sizeof(int));
p[5] = 10; // Valid?
```
---

> [!question]- **Q2: Predict the output**
> - **Undefined behavior**: Accessing freed memory.

```c
int *p = malloc(sizeof(int));
*p = 5;
free(p);
printf("%d", *p);
```
---

> [!question]- **Q3: Fix this code**
> - **Buffer overflow**: Allocate 6 bytes for `"Hello\0"`.

```c
char *s = malloc(5);
strcpy(s, "Hello");
```
---

### **8. Key Takeaways**
1. **Ownership**: Track who allocates/frees memory.
2. **Safety**: Use `calloc`/`realloc` properly and check for `NULL`.
3. **Tools**: Use `valgrind` to detect leaks (not exam-relevant but real-world critical).
4. **Undefined Behavior**: Avoid dangling pointers, double frees, and out-of-bounds access.

---

**Final Exam-Style Questions**:

**Q1**: What happens here?  
```c
int *p = malloc(sizeof(int));
int *q = p;
free(p);
*q = 10;
```

**Q2**: Why is this dangerous?  
```c
void process(int size) {
    int arr[size]; // Variable-Length Array on stack
}
```

**Q3**: Optimize this code:  
```c
int *arr = malloc(100 * sizeof(int));
memset(arr, 0, 100 * sizeof(int));
```

> [!question]- Answer
> - **A1**: Dangling pointer (`q` points to freed memory). UB! 
> - **A2**: Stack overflow for large `size`. Use heap allocation instead.  
> - **A3**: Replace with `calloc(100, sizeof(int))` for zero-initialization.




