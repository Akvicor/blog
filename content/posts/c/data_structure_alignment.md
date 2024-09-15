---
title: "Data Structure Alignment"
date: 2023-02-27 07:14:03 +08:00
draft: false
categories: [C]
tags: []
card: false
weight: 0
---

The CPU in modern computer hardware performs reads and writes to memory most efficiently when the data is naturally aligned, which generally means that the data's memory address is a multiple of the data size. For instance, in a 32-bit architecture, the data may be aligned if the data is stored in four consecutive bytes and the first byte lies on a 4-byte boundary.

If the highest and lowest bytes in a datum are not within the same memory word the computer must split the datum access into multiple memory accesses. This requires a lot of complex circuitry to generate the memory accesses and coordinate them. To handle the case where the memory words are in different memory pages the processor must either verify that both pages are present before executing the instruction or be able to handle a TLB miss or a page fault on any memory access during the instruction execution.

```c
#include <stdio.h>

typedef struct structa_tag { // A
  char        c;
  short int   s;
} structa_t;

typedef struct structb_tag { // B
  short int   s;
  char        c;
  int         i;
} structb_t;

typedef struct structc_tag { // C
  char        c;
  double      d;
  int         s;
} structc_t;

typedef struct structd_tag { // D
  double      d;
  int         s;
  char        c;
} structd_t;

int main() {
  printf("sizeof(structa_t) = %d\n", sizeof(structa_t));
  printf("sizeof(structb_t) = %d\n", sizeof(structb_t));
  printf("sizeof(structc_t) = %d\n", sizeof(structc_t));
  printf("sizeof(structd_t) = %d\n", sizeof(structd_t));

  return 0;
}

/** OUTPUT
sizeof(structa_t) = 4
sizeof(structb_t) = 8
sizeof(structc_t) = 24
sizeof(structd_t) = 16
**/

// Alignment requirements
// (typical 64 bit machine)

// char         1 byte
// short int    2 bytes
// int          4 bytes
// double       8 bytes
```

***Output of Above Program:***

For the sake of convenience, assume every structure type variable is allocated on 4 byte boundary (say 0x0000), i.e. the base address of structure is multiple of 4 (need not necessary always, see explanation of structc_t).

***structure A***

The structa_t first element is char which is one byte aligned, followed by short int. short int is 2 byte aligned. If the the short int element is immediately allocated after the char element, it will start at an odd address boundary. The compiler will insert a padding byte after the char to ensure short int will have an address multiple of 2 (i.e. 2 byte aligned). The total size of structa_t will be sizeof(char) + 1 (padding) + sizeof(short), 1 + 1 + 2 = 4 bytes.

***structure B***

The first member of structb_t is short int followed by char. Since char can be on any byte boundary no padding required in between short int and char, on total they occupy 3 bytes. The next member is int. If the int is allocated immediately, it will start at an odd byte boundary. We need 1 byte padding after the char member to make the address of next int member is 4 byte aligned. On total, the structb_t requires 2 + 1 + 1 (padding) + 4 = 8 bytes.

***structure C – Every structure will also have alignment requirements***

Applying same analysis, structc_t needs sizeof(char) + 7 byte padding + sizeof(double) + sizeof(int) = 1 + 7 + 8 + 4 = 20 bytes. However, the sizeof(structc_t) will be 24 bytes. It is because, along with structure members, structure type variables will also have natural alignment. Let us understand it by an example. Say, we declared an array of structc_t as shown below

```c
structc_t structc_array[3];
```

Assume, the base address of structc_array is 0x0000 for easy calculations. If the structc_t occupies 20 (0x14) bytes as we calculated, the second structc_t array element (indexed at 1) will be at 0x0000 + 0x0014 = 0x0014. It is the start address of index 1 element of array. The double member of this structc_t will be allocated on 0x0014 + 0x1 + 0x7 = 0x001C (decimal 28) which is not multiple of 8 and conflicting with the alignment requirements of double. As we mentioned on the top, the alignment requirement of double is 8 bytes.

Inorder to avoid such misalignment, compiler will introduce alignment requirement to every structure. It will be as that of the largest member of the structure. In our case alignment of structa_t is 2, structb_t is 4 and structc_t is 8. If we need nested structures, the size of largest inner structure will be the alignment of immediate larger structure.

In structc_t of the above program, there will be padding of 4 bytes after int member to make the structure size multiple of its alignment. Thus the sizeof (structc_t) is 24 bytes. It guarantees correct alignment even in arrays. You can cross check.

***structure D – How to Reduce Padding?***

By now, it may be clear that padding is unavoidable. There is a way to minimize padding. The programmer should declare the structure members in their increasing/decreasing order of size. An example is structd_t given in our code, whose size is 16 bytes in lieu of 24 bytes of structc_t.

***Typical Alignments***

The following typical alignments are valid for compilers from Microsoft (Visual C++), Borland/CodeGear (C++Builder), Digital Mars (DMC), and GNU (GCC) when compiling for 32-bit x86:

- A **char** (one byte) will be 1-byte aligned.
- A **short** (two bytes) will be 2-byte aligned.
- An **int** (four bytes) will be 4-byte aligned.
- A **long** (four bytes) will be 4-byte aligned.
- A **float** (four bytes) will be 4-byte aligned.
- A **double** (eight bytes) will be 8-byte aligned on Windows and 4-byte aligned on Linux (8-byte with -malign-double compile time option).
- A **long long** (eight bytes) will be 4-byte aligned.
- A **long double** (ten bytes with C++Builder and DMC, eight bytes with Visual C++, twelve bytes with GCC) will be 8-byte aligned with C++Builder, 2-byte aligned with DMC, 8-byte aligned with Visual C++, and 4-byte aligned with GCC.
- Any **pointer** (four bytes) will be 4-byte aligned. (e.g.: char*, int*)

The only notable differences in alignment for an LP64 64-bit system when compared to a 32-bit system are:

- A **long** (eight bytes) will be 8-byte aligned.
- A **double** (eight bytes) will be 8-byte aligned.
- A **long long** (eight bytes) will be 8-byte aligned.
- A **long double** (eight bytes with Visual C++, sixteen bytes with GCC) will be 8-byte aligned with Visual C++ and 16-byte aligned with GCC.
- Any **pointer** (eight bytes) will be 8-byte aligned.


