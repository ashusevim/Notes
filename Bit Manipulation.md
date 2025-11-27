# Bit Manipulation

![Basics 1.png](Bit%20Manipulation/Basics_1.png)

![Basics 2.svg](Bit%20Manipulation/Basics_2.svg)

![Basics 3.svg](Bit%20Manipulation/Basics_3.svg)

## Bitwise Complement operator

- it is basically a unary operation that works only one operand
- it converts the binary digits from 1 to 0 and 0 to 1
- **bitwise complement** of any integer **N** is equal to **(N + 1)**.

# why it works?

- The bitwise complement of any integer N is equal to −(N+1) due to the way computers represent **signed integers** using a system called **two's complement**. This representation allows for efficient arithmetic operations, particularly subtraction, to be performed using the same circuitry as addition.

## What is two's complement?

Two's complement is the standard method for representing signed integers (positive, negative, and zero) in binary. A number's two's complement is the value that, when added to the original number, results in zero.

Let's illustrate with a 4-bit integer.

- The positive number 5 is represented as 01012.
- To get the two's complement of 5, we first perform a **bitwise complement** (inverting all the bits), which gives us 10102.
- We then add 1 to the result: 10102+12=10112.
- The binary value 10112 represents -5 in two's complement.

## Why the formula works

The bitwise complement of a number N, often denoted by ∼N, is equivalent to the negation of N in a system where all bit positions are flipped.

Let's consider the number N in binary.

- The **bitwise complement** ∼N is obtained by flipping every bit of N.
- When we add ∼N to N, every bit position becomes 1. For example, 01012+10102=11112.
- A sequence of all 1's in a two's complement system represents -1. This is because adding 1 to it overflows and results in 0 (e.g., 11112+12=100002, which, in a 4-bit system, is 0 after the overflow bit is discarded). So, 11112=−1.
- Thus, we have the relationship: N+∼N=−1.
- Rearranging this equation gives us ∼N=−1−N, which can be rewritten as **∼N=−(N+1)**.

![Operators.svg](Bit%20Manipulation/Operators.svg)