### **1. Bitwise Operators**
- Bitwise operators perform operations on individual bits of integers. 

**Operators**:
- `&` (AND): Sets bit to 1 if both bits are 1.
- `|` (OR): Sets bit to 1 if either bit is 1.
- `^` (XOR): Sets bit to 1 if bits are different.
- `~` (NOT): Flips all bits.
- `<<` (Left Shift): Shifts bits left, padding with 0s.
- `>>` (Right Shift): Shifts bits right (signed: arithmetic, unsigned: logical).

**Example**:
```c
int a = 5;      // 0101
int b = 3;      // 0011
int c = a & b;  // 0001 (1)
int d = a << 2; // 10100 (20)
```

---

### **2. Common Bitwise Operations**

#### **Set a Bit**:
```c
x |= (1 << n); // Set nth bit (0-indexed)
```
#### **Clear a Bit**:
```c
x &= ~(1 << n); // Clear nth bit
```
#### **Toggle a Bit**:
```c
x ^= (1 << n); // Toggle nth bit
```
#### **Check a Bit**:
```c
if (x & (1 << n)) { /* nth bit is set */ }
```

---
### **3. Bit Masking**
Extract or modify specific bits using masks.  

**Example**:
```c
// Extract lower 4 bits:
int lower = x & 0xF;
// Set upper 4 bits:
x |= 0xF0;
```

---
### **4. Bit Shifting**

#### **Left Shift**:
- Equivalent to multiplying by 2<sup>n</sup> (for non-negative integers).
```c
int x = 5;      // 0101
x = x << 2;     // 10100 (20)
```

#### **Right Shift**:
- **Logical Shift** (unsigned): Pads with 0s.
- **Arithmetic Shift** (signed): Pads with sign bit.
```c
int y = -8;     // ...11111000 (2's complement)
y = y >> 1;     // ...11111100 (-4, implementation-defined)
```
---
### **5. Bit Fields in Structs**
- Define struct members with specific bit widths.  

**Example**:
```c
struct Flags {
    unsigned int isReady : 1; // 1-bit field
    unsigned int mode : 3;    // 3-bit field
};

struct Flags f;
f.isReady = 1;
f.mode = 5; // Values exceeding 3 bits are truncated.
```

---
### **6. Common Pitfalls**

#### **Pitfall 1: Operator Precedence**:
```c
if (x & 1 == 0) { /* Equivalent to x & (1 == 0) → Always false! */ }
// Fix: if ((x & 1) == 0)
```

#### **Pitfall 2: Shifting Beyond Bit Width**:
```c
int x = 1;
x = x << 33; // Undefined behavior for 32-bit int.
```

#### **Pitfall 3: Signed Right Shifts**:
```c
int x = -8;
x = x >> 1; // Result is implementation-defined.
```

---
### **7. Tricky Code Snippets**

> [!question]- **Q1: What does this code do?**
> - Clears the least significant set bit.
>     
> - Result: `1010 & 1001 = 1000 (8)`.

```c
int x = 10; // 1010
x = x & (x - 1);
```

---

> [!question]- **Q2: Predict the output**:
> - **Output**: `55` (01010101 in hex). 

```c
int x = 0xFF ^ 0xAA;
printf("%x", x); // 0xFF = 11111111, 0xAA = 10101010
```

---

> [!question]- **Q3: Identify the bug**:
> - **Bug**: Signed integer overflow (UB). Use `unsigned int` for bit 31.

```c
int x = 1 << 31; // For 32-bit int
printf("%d", x);
```

---
### **8. Applications of Bitwise Operations**

1. **Flags/Options**:
```c
#define READ_ONLY  (1 << 0)
#define HIDDEN     (1 << 1)

int file_perms = READ_ONLY | HIDDEN;        
```

2. **Efficient Storage** (e.g., RGB colors):
```c
unsigned int rgb = (r << 16) | (g << 8) | b;
```    
    
3. **Swapping Variables**:
```c
a ^= b;
b ^= a;
a ^= b;    
```    

---
### **9. Key Takeaways**
1. **Operator Precedence**: Use parentheses to avoid errors.
2. **Unsigned vs. Signed**: Prefer `unsigned` for bitwise operations.
3. **Shifting**: Left shifts multiply, right shifts divide (for non-negative numbers).
4. **Bit Fields**: Use for compact storage but avoid platform-dependent code.

---

**Final Exam-Style Questions**:

**Q1**: What is the output?
```c
int x = 0x0F; // 0000 1111
x = x | (x << 4);
printf("%x", x);
```

**Q2**: Fix this code:
```c
int x = 5;
if (x & 1) {
    printf("Odd"); // Should print for odd numbers
}
```

**Q3**: Predict the output:
```c
unsigned int x = 0x1;
printf("%d", x << 31);
```

> [!question]- Answers
> - **A1**: `0xFF` (00001111 | 11110000 = 11111111).
> - **A2**: `if (x & 1)` is correct (no fix needed).
> - **A3**: `2147483648` (2<sup>31</sup>, assuming 32-bit `unsigned int`).
