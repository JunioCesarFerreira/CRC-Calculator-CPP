# CRC-Calculator-CPP

🌍 *[Português](README.md) ∙ [English](README_en.md)*

This repository contains a C++ implementation to calculate CRC (Cyclic Redundancy Check) for various data sizes and configurations. The implementation is generic, allowing different divisor polynomials, initial values, and reflection settings.

## Overview

CRC is a method used to detect errors in digital data. It uses polynomial division to calculate a checksum (CRC value) for the data. The calculated checksum can then be used to verify the integrity of the data during transmission or storage.

## Features

- Generic CRC calculation for data sizes of 8, 16, 32, and 64 bits.
- Configurable divisor polynomial and initial value.
- Support for input and output reflection.
- Comprehensive test program to validate the implementation.

## CRC Theory

### Error Detection Code

CRC is a popular error detection code used in digital networks and storage devices. It works by appending a sequence of redundant bits (CRC bits) to the end of the data block, forming a codeword. When the codeword is received or read, the same CRC algorithm is used to calculate the checksum. If the calculated checksum matches the appended CRC bits, the data is considered intact.

#### How CRC Works

1. **Data Representation**: The data to be transmitted is represented as a binary polynomial.
2. **Polynomial Division**: The data polynomial is divided by a predetermined polynomial (divisor polynomial) using binary division.
3. **Remainder Calculation**: The remainder of the division operation is the CRC value.
4. **Transmission**: The CRC value is appended to the data and sent as a codeword.
5. **Verification**: At the destination, the same division process is performed on the received codeword. If the remainder is zero, the data is considered valid.

#### Reflection

Reflection refers to reversing the order of the bits of the data bytes before the CRC calculation. This is used for compatibility with certain hardware implementations that process bits in reverse order.

### Theoretical Details

CRC is based on arithmetic operations in a finite field, specifically the Galois field, denoted $GF(2)$. The polynomial division used in CRC operates with coefficients in $\mathbb{Z}_2=\{0,1\}$, which simplifies the calculation.

#### Generator Polynomial

The generator polynomial is a fixed binary polynomial used as the divisor in the polynomial division operation. It is represented in the form:

$$
G(x) = x^n + g_{n-1}x^{n-1} + \ldots + g_1x + g_0
$$

where $n$ is the degree of the polynomial and $g_i$ are the binary coefficients.

#### CRC Calculation

1. **Data Representation**: The data $D(x)$ is represented as a binary polynomial. For example, a bit sequence `110100011010` can be represented as

$$
D(x)=x^{11}+x^{10}+x^8+x^4+x^3+x^1
$$

2. **Multiplication by $x^n$**: The data polynomial is multiplied by $x^n$, where $n$ is the degree of the generator polynomial. In binary, this is equivalent to appending $n$ zeros to the end of the sequence, as follows:

$$
x^n \cdot D(x)=x^{11+n}+x^{10+n}+x^{8+n}+x^{4+n}+x^{3+n}+x^{1+n}
$$

let's say $n=4$, then the resulting bit sequence is `1101000110100000`.

3. **Polynomial Division**: The data polynomial multiplied by $x^n$ is divided by the generator polynomial $G(x)$. The remainder $R(x)$ of the division is the CRC value. This is done by the following algorithm:

    - Align the multiplied polynomial with the generator polynomial and perform division using XOR for subtraction.

    - The process is repeated until the number of remaining bits is less than the number of bits in the generator polynomial.

    - The remaining bits correspond to $R(x)$ as follows:
    
$$
    D(x) \cdot x^n = Q(x) \cdot G(x) + R(x)
$$

where $Q(x)$ is the quotient of the division and $R(x)$ is the remainder, representing the CRC value.

5. **Forming the Codeword**: The codeword is formed by concatenating the data polynomial with the remainder $R(x)$.

In the file [`polynomial`](polynomial_en.md) we present some more theoretical details about polynomials and the equivalence of polynomials over $\mathbb{Z}_2$ and binary sequences (or binary words).

## Usage

### Class Definition

The `CrcClass` template class is used to calculate CRC values. The class supports different data sizes and configurations.

### Example Usage

```cpp
#include "CrcClass.h"  // Include the header for the CRC class

int main()
{
    char testArray[14] = { 128, '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'A', 129 };
    
    // Example for CRC-8
    CrcClass<uint8_t> crc8(0x7, 0, false, false);
    uint8_t crc8_result = crc8.calc(testArray, 14);
    
    // Example for CRC-16
    CrcClass<uint16_t> crc16(0x1021, 0xFFFF, false, false);
    uint16_t crc16_result = crc16.calc(testArray, 14);
    
    // Example for CRC-32
    CrcClass<uint32_t> crc32(0x04C11DB7, 0xFFFFFFFF, false, false);
    uint32_t crc32_result = crc32.calc(testArray, 14);
    
    return 0;
}
```


## Compilation

### Compilation on Windows

1. Install [MINGW-w64](http://mingw-w64.org/) and add it to your PATH.
2. Open the MINGW64 terminal and navigate to the project directory.
3. Compile the code with the command:

    ```sh
    g++ -o test CrcCalculator.cpp
    ```

4. Run the program with:

    ```sh
    ./test
    ```

**Note**: Another option is to install the g++ compiler and set it up with VS Code.

### Compilation on Linux

1. Ensure that `g++` is installed on your system. If not, you can install it by running:

    ```sh
    sudo apt update
    sudo apt install g++
    ```

2. Navigate to the project directory.
3. Compile the code with the command:

    ```sh
    g++ -o test CrcCalculator.cpp
    ```

4. Run the program with:

    ```sh
    ./test
    ```

Running `test` should return:

```
CRC-8
result crc: f1  checked value: f1
result crc: 8f  checked value: 8f
result crc: c8  checked value: c8
result crc: 13  checked value: 13

CRC-16
result crc: 6755        checked value: 6755
result crc: aae6        checked value: aae6
result crc: 3100        checked value: 3100
result crc:   8c        checked value:   8c

CRC-32
result crc: b8b77646    checked value: b8b77646
result crc: 626eed1d    checked value: 626eed1d
result crc: d26e6139    checked value: d26e6139
result crc: 9c86764b    checked value: 9c86764b
```

## Contributions

Corrections and improvements are always welcome!

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
