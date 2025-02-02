### **1. String Basics**
- A string in C is a null-terminated (`'\0'`) array of `char`.  
**Key Properties**:
- Stored in contiguous memory.
- Terminated by `'\0'` (ASCII 0).
- String literals (e.g., `"Hello"`) are stored in read-only memory.

**Declaration**:
```c
char s1[] = "Hello";    // Stack-allocated, mutable
char *s2 = "World";     // Pointer to read-only literal
char s3[10];            // Uninitialized buffer
```

---

### **2. Memory Allocation**

#### **Stack Allocation**:
```c
char name[20] = "Alice"; // Fixed-size buffer
```

#### **Heap Allocation**:
```c
char *str = malloc(50 * sizeof(char));
strcpy(str, "Dynamic string");
free(str); // Always free!
```

#### **Dynamic Strings**:
```c
char *create_str(const char *src) {
    char *dest = malloc(strlen(src) + 1); // +1 for '\0'
    strcpy(dest, src);
    return dest;
}
```

---
### **3. Common Pitfalls**
1. **Buffer Overflow**:
```c
char s[5];
strcpy(s, "HelloWorld"); // Overflow! Undefined behavior.
```

2. **Missing Null Terminator**:
```c
char s[5] = {'H', 'e', 'l', 'l', 'o'}; // No '\0' → Not a valid string!
```

3. **Modifying Literals**:
```c
char *s = "Hello";
s[0] = 'h'; // Crash! Attempt to write to read-only memory.
```
---
### **4. String Functions**
#### **Core Functions**:

| Function  | Syntax                                                   | Behavior                                           |
| --------- | -------------------------------------------------------- | -------------------------------------------------- |
| `strlen`  | `size_t strlen(const char *s)`                           | Returns length (excluding `'\0'`).                 |
| `strcpy`  | `char* strcpy(char *dest, const char *src)`              | Copies `src` to `dest` (unsafe).                   |
| `strncpy` | `char* strncpy(char *dest, const char *src, size_t n)`   | Copies up to `n` chars (safer).                    |
| `strcat`  | `char* strcat(char *dest, const char *src)`              | Appends `src` to `dest`.                           |
| `strncat` | `char* strncat(char *dest, const char *src, size_t n)`   | Appends up to `n` chars.                           |
| `strcmp`  | `int strcmp(const char *s1, const char *s2)`             | Returns 0 if equal, <0 if `s1 < s2`, >0 otherwise. |
| `strchr`  | `char* strchr(const char *s, int c)`                     | Returns pointer to first occurrence of `c`.        |
| `strstr`  | `char* strstr(const char *haystack, const char *needle)` | Finds substring `needle` in `haystack`.            |
| `strtok`  | `char* strtok(char *str, const char *delim)`             | Tokenizes string (stateful, modifies input).       |

#### **Examples**:
```c
// strlen
printf("%zu", strlen("Hello")); // Output: 5

// strcpy vs strncpy
char dest[10];
strncpy(dest, "Hello World", sizeof(dest)); // Truncates to fit buffer

// strtok
char str[] = "A,B,C";
char *token = strtok(str, ",");
while (token) {
    printf("%s\n", token); // Output: A\nB\nC
    token = strtok(NULL, ",");
}
```

---

### **5. Safer Alternatives**
- **`snprintf`**: Bounded string formatting.
```c
char buf[10];
snprintf(buf, sizeof(buf), "%s", "Hello World"); // Truncates to fit
```
    
- **`strdup`**: Duplicates a string (non-standard but common).
```c
char *s = strdup("Hello"); // Allocates and copies
free(s);
```

---

### **6. Tricky Code Snippets**

> [!question]- **Q1: What does this print?**
> 
> - **Output**: `5 12`
>     
> - `strlen` stops at `'\0'`, `sizeof` includes all bytes (11 chars + null terminator).

```c
char s[] = "Hello\0World";
printf("%zu %zu", strlen(s), sizeof(s));
```

---

> [!question]- **Q2: Identify the bug**
> 
> - **Buffer overflow**: `"Hello"` requires 6 bytes (5 chars + `'\0'`).

```c
char *s = malloc(5);
strcpy(s, "Hello");
printf("%s", s);
```

---

> [!question]- **Q3: Predict the output**
> - **Output**: Negative value (ASCII 'A' (65) < 'B' (66)).

```c
printf("%d", strcmp("Apple", "Banana"));
```

---

### **7. Key Takeaways**
1. **Null Terminator**: Always reserve space for `'\0'`.
2. **Buffer Safety**: Prefer `strn` functions over `str` variants.
3. **Literal Immutability**: Never modify string literals.
4. **Dynamic Strings**: Always track ownership to prevent leaks.
---

### **8. Common String Functions Cheat Sheet**

|Function|Description|
|---|---|
|`strlen`|Get string length (excludes `'\0'`).|
|`strcpy`/`strncpy`|Copy strings (use `strncpy` for bounds).|
|`strcat`/`strncat`|Concatenate strings.|
|`strcmp`/`strncmp`|Compare strings lexicographically.|
|`strchr`/`strrchr`|Find first/last occurrence of a character.|
|`strstr`|Find substring.|
|`strtok`|Split string into tokens.|
|`sprintf`/`snprintf`|Format string into buffer.|
|`strdup`|Duplicate string (allocates memory).|
|`memcpy`/`memmove`|Copy memory blocks (use `memmove` for overlap).|

---

**Final Exam-Style Questions**:

**Q1**: Fix this code:
```c
char s[5];
strcpy(s, "Hello");
```

**Q2**: What does this print?
```c
printf("%s", strchr("Hello", 'l') + 1);
```

**Q3**: Why is this dangerous?
```c
char *concat(const char *a, const char *b) {
    char result[100];
    strcpy(result, a);
    strcat(result, b);
    return result;
}
```


> [!question]- Answers
> - **A1**: Use `strncpy(s, "Hello", sizeof(s)-1); s[sizeof(s)-1] = '\0'; 
> - // only four characters are copied and then null terminator is added
> - **A2**: `"lo"` (pointer to first 'l' + 1).
> - **A3**: Returns pointer to stack-allocated `result` (dangling pointer).
