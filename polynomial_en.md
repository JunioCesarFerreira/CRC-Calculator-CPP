## Introduction to Polynomials

A polynomial is a mathematical expression composed of terms consisting of variables, coefficients, and operations of addition, subtraction, and multiplication. Formally, a polynomial in a variable $x$ can be written as:

$$
P(x) = a_n x^n + a_{n-1} x^{n-1} + \cdots + a_1 x + a_0
$$

where $a_n, a_{n-1}, \ldots, a_1, a_0$ are coefficients belonging to a ring $K$, and $a_n \neq 0$. The highest exponent $n$ for which the coefficient $a_n \neq 0$ is called the degree of the polynomial, denoted by $\deg(P(x))=n$.

Polynomials can be manipulated similarly to numbers, with operations of addition, subtraction, multiplication, and, in some cases, division.

## Ring Structure of Polynomials

The algebraic structure of polynomials has properties that classify them as a ring. A ring is a structure $(R, +, \cdot)$ where $R$ is a set equipped with two binary operations: addition ($+$) and multiplication $(\cdot)$.

To show that the set of polynomials with coefficients in a field $K$ forms a ring, it is necessary to verify that the operations of polynomial addition and multiplication satisfy the following properties:

1. **Closure**: For any polynomials $P(x), Q(x) \in K[x]$:
   - The sum $P(x) + Q(x)$ and the product $P(x) \cdot Q(x)$ are also in $K[x]$.

2. **Associativity**: For any polynomials $P(x), Q(x), R(x) \in K[x]$:
   - $(P(x) + Q(x)) + R(x) = P(x) + (Q(x) + R(x))$
   - $(P(x) \cdot Q(x)) \cdot R(x) = P(x) \cdot (Q(x) \cdot R(x))$

3. **Commutativity of addition**: For any polynomials $P(x), Q(x) \in K[x]$:
   - $P(x) + Q(x) = Q(x) + P(x)$

4. **Additive identity**: There exists a zero polynomial $0(x) \in K[x]$ such that for any polynomial $P(x)$:
   - $P(x) + 0(x) = P(x)$

5. **Additive inverse**: For any polynomial $P(x) \in K[x]$, there exists a polynomial \(-P(x)\) such that:
   - $P(x) + (-P(x)) = 0(x)$

6. **Distributivity**: For any polynomials $P(x), Q(x), R(x) \in K[x]$:
   - $P(x) \cdot (Q(x) + R(x)) = P(x) \cdot Q(x) + P(x) \cdot R(x)$
   - $(P(x) + Q(x)) \cdot R(x) = P(x) \cdot R(x) + Q(x) \cdot R(x)$

## Polynomial Division

Polynomial division is a more complex operation than addition, subtraction, and multiplication. When dividing a polynomial $P(x)$ by a polynomial $D(x)$, the result can be expressed in the form:

$$
P(x) = Q(x) \cdot D(x) + R(x)
$$

where $Q(x)$ is the quotient and $R(x)$ is the remainder, and $\deg(R(x)) < \deg(D(x))$.

### Polynomial Division Algorithm

1. **Initialization**: Let $P(x) = a_n x^n + \cdots + a_0$ and $D(x) = b_m x^m + \cdots + b_0$, where $n \geq m$.

2. **Quotient**: Initially, $Q(x) = 0$ and $R(x) = P(x)$.

3. **Algorithm steps**:
   - While the degree of $R(x)$ is greater than or equal to the degree of $D(x)$, do:
     - Calculate the term of the quotient: $t(x) = \frac{\text{leading term of } R(x)}{\text{leading term of } D(x)}$.
     - Update the quotient: $Q(x) = Q(x) + t(x)$.
     - Update the remainder: $R(x) = R(x) - t(x) \cdot D(x)$.

4. **Result**: The process ends when the degree of $R(x)$ is less than the degree of $D(x)$, resulting in the polynomials $Q(x)$ and $R(x)$.

### Example

Consider $P(x) = 2x^3 + 3x^2 + x + 5$ and $D(x) = x^2 + 1$.

1. **First term of the quotient**: \(\frac{2x^3}{x^2} = 2x\).
   - $Q(x) = 2x$
   - $R(x) = (2x^3 + 3x^2 + x + 5) - 2x(x^2 + 1) = x^2 + x + 5$

2. **Second term of the quotient**: \(\frac{x^2}{x^2} = 1\).
   - $Q(x) = 2x + 1$
   - $R(x) = (x^2 + x + 5) - (x^2 + 1) = x + 4$

Thus, we have $P(x) = (2x + 1)(x^2 + 1) + (x + 4)$.

This process demonstrates that although polynomials can be divided, the result usually includes a remainder, similar to the integer division of numbers.

---

## Correspondence between Polynomials in $\mathbb{Z}_2$  and Binary Sequences

1. **Polynomials as Binary Sequences**:
   - Each term $x^i$ in a polynomial can be mapped to the bit $b_i$ in a binary sequence, where $b_i$ is 0 or 1. The polynomial $P(x) = a_n x^n + \cdots + a_1 x + a_0$ is represented by the binary sequence $a_n a_{n-1} \cdots a_1 a_0$.

2. **Addition and XOR Operations**:
   - The addition of polynomials in $\mathbb{Z}_2$ corresponds to the XOR operation in binary sequences. Thus, the sum $a_i + b_i \pmod{2}$ of two polynomial coefficients is equivalent to the XOR operation ($\oplus$) of the corresponding bits.

### Example of Correspondence

Let's take the previous example of polynomials:

- **Polynomials**:
  - $P(x) = x^4 + x^3 + x^2 + 1$
  - $D(x) = x^2 + x + 1$

- **Binary Sequences**:
  - $P(x)$ corresponds to $11001$
  - $D(x)$ corresponds to $111$

## Division of Binary Sequences using XOR

The division of polynomials in binary form follows the same logic as the division of binary numbers using XOR.

### Steps of Calculation with XOR

1. **Binary Sequence of $P(x)$**: $11001$

2. **Binary Sequence of $D(x)$**: $111$

3. **Division using XOR**:

   - **First Step**:
      - Divide $110$ (the first three bits of $11001$) by $111$ using XOR:

$$
110 \oplus 111 = 001
$$
   - Partial remainder: $001$

   - **Second Step**:
     - Append the next bit of $P(x)$, which is $0$, to the remainder: $0010$.
     - Divide $0010$ by $111$ using XOR:

$$
0010 \oplus 1110 = 1100
$$
   - Partial remainder: $1100$

   - **Third Step**:
     - Append the last bit of $P(x)$, which is $1$, to the remainder: $11001$.
     - Divide $1100$ by $111$ using XOR:

$$
1100 \oplus 1110 = 0101
$$

### Result

The quotient $Q(x)$ is $100$ (corresponding to $x^2$), and the remainder $R(x)$ is $01$ (corresponding to 1).

### Conclusion

The division operation of polynomials in $\mathbb{Z}_2$ is isomorphic to the calculation with binary sequences using XOR. Each term of the polynomial is mapped to a bit, and the addition of polynomials (mod 2) is mapped to the XOR operation, demonstrating that the algebraic structure and operations between polynomials and binary sequences are structurally equivalent.

## Isomorphism

An isomorphism between two sets $A$ and $B$ is a bijection $f: A \to B$ that preserves operations, i.e., for any $a_1, a_2 \in A$, we have:

$$
f(a_1 + a_2) = f(a_1) \oplus f(a_2)
$$

### Polynomials

 and Binary Sequences

- **Polynomial in $\mathbb{Z}_2$**: A polynomial $P(x) = a_n x^n + \cdots + a_1 x + a_0$ is represented by a binary sequence $a_n a_{n-1} \cdots a_1 a_0$.

- **Operations**:
  - **Addition** of polynomials in $\mathbb{Z}_2$ corresponds to performing the sum of coefficients modulo 2:

$$
(a_i + b_i) \pmod{2}
$$
  - **XOR** of bits in binary sequences:

$$
a_i \oplus b_i
$$

### Proof of Isomorphism

To show that the addition of polynomials is isomorphic to the XOR operation in binary sequences, we need to verify that the algebraic structure is preserved.

1. **Closure**:
   - Both the addition of polynomials in $\mathbb{Z}_2$ and the XOR of binary sequences result in a new sequence in the same structure.

2. **Associativity**:
   - The addition of polynomials is associative:

$$
(P(x) + Q(x)) + R(x) = P(x) + (Q(x) + R(x))
$$
   - The XOR of binary sequences is associative:

$$
(a \oplus b) \oplus c = a \oplus (b \oplus c)
$$

3. **Identity Element**:
   - The zero polynomial $0(x)$ corresponds to the binary sequence $0 \cdots 0$, and:

$$
P(x) + 0(x) = P(x)
$$
   - The sequence of zeros is the identity element for XOR:

$$
a \oplus 0 = a
$$

4. **Inverse Element**:
   - Each polynomial $P(x)$ has an inverse in $\mathbb{Z}_2$ that is itself:

$$
P(x) + P(x) = 0(x)
$$
   - For XOR, each bit has an inverse that is itself:

$$
a \oplus a = 0
$$

5. **Distributivity**:
   - The multiplication of polynomials over addition in $\mathbb{Z}_2$ is distributive, as is the combination of AND with XOR in binary sequences:

$$
a \cdot (b \oplus c) = (a \cdot b) \oplus (a \cdot c)
$$

### Correspondence of Division

In the division of polynomials in $\mathbb{Z}_2$, the steps of the division algorithm correspond directly to binary subtraction operations using XOR:

- **Polynomial**: $P(x) = x^4 + x^3 + x^2 + 1$ (in binary: 11001).
- **Divisor**: $D(x) = x^2 + x + 1$ (in binary: 111).

1. **First Step**:
   - Subtract $111$ from $110$ (XOR):

$$
110 \oplus 111 = 001
$$

2. **Second Step**:
   - Append the next bit of $P(x)$ and continue the operation:

$$
0010 \oplus 1110 = 1100
$$

3. **Third Step**:
   - Continue with the binary subtraction operation:

$$
1100 \oplus 1110 = 0101
$$

These XOR steps correspond exactly to the polynomial division operations, illustrating that the polynomial division operation in binary form is isomorphic to binary division using XOR. The quotient and remainder obtained are consistent in both representations.

### Conclusion

The algebraic structure and operations between polynomials and binary sequences are structurally equivalent in $\mathbb{Z}_2$. Each term of the polynomial is mapped to a bit, and the addition of polynomials (mod 2) is mapped to the XOR operation, preserving the algebraic structure and demonstrating the isomorphism between these operations.