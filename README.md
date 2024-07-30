# CRC-Calculator-CPP

Este repositório contém uma implementação em C++ para calcular o CRC (*Cyclic Redundancy Check*) para vários tamanhos de dados e configurações. A implementação é genérica, permitindo diferentes polinômios divisores, valores iniciais e configurações de reflexão.

## Visão Geral

O CRC é um método utilizado para detectar erros em dados digitais. Ele utiliza divisão polinomial para calcular uma soma de verificação (valor CRC) para os dados. A soma de verificação calculada pode então ser usada para verificar a integridade dos dados durante a transmissão ou armazenamento.

## Funcionalidades

- Cálculo genérico de CRC para tamanhos de dados de 8, 16, 32 e 64 bits.
- Polinômio divisor e valor inicial configuráveis.
- Suporte para reflexão de entrada e saída.
- Programa de teste abrangente para validar a implementação.

## Teoria do CRC

### Código de Detecçaõ de Erros

CRC é um código de detecção de erros popular utilizado em redes digitais e dispositivos de armazenamento. Ele funciona anexando uma sequência de bits redundantes (bits CRC) ao final do bloco de dados, formando uma palavra-código. Quando a palavra-código é recebida ou lida, o mesmo algoritmo de CRC é utilizado para calcular a soma de verificação. Se a soma de verificação calculada coincidir com os bits CRC anexados, os dados são considerados intactos.

#### Como o CRC Funciona

1. **Representação dos Dados**: Os dados a serem transmitidos são representados como um polinômio binário.
2. **Divisão Polinomial**: O polinômio dos dados é dividido por um polinômio predeterminado (polinômio divisor) usando divisão binária.
3. **Cálculo do Resto**: O resto da operação de divisão é o valor CRC.
4. **Transmissão**: O valor CRC é anexado aos dados e enviado como uma palavra-código.
5. **Verificação**: No destino, o mesmo processo de divisão é realizado na palavra-código recebida. Se o resto for zero, os dados são considerados válidos.

#### Reflexão

Reflexão refere-se a inverter a ordem dos bits dos bytes dos dados antes do cálculo do CRC. Isso é usado para compatibilidade com certas implementações de hardware que processam bits em ordem inversa.

### Detalhes Teóricos

O CRC é baseado em operações aritméticas em um corpo finito, mais especificamente no corpo de Galois, denotado $GF(2)$. A divisão polinomial usada no CRC opera com coeficientes em $\mathbb{Z}_2=\{0,1\}$, o que simplifica o cálculo.

#### Polinômio Gerador

O polinômio gerador é um polinômio binário fixo usado como divisor na operação de divisão polinomial. Ele é representado na forma:

$$
G(x) = x^n + g_{n-1}x^{n-1} + \ldots + g_1x + g_0
$$

onde $n$ é o grau do polinômio e $g_i$ são os coeficientes binários.

#### Cálculo do CRC

1. **Representação dos Dados**: Os dados $D(x)$ são representados como um polinômio binário.  Por exemplo, uma sequência de bits `110100011010` pode ser representada como
$$
D(x)=x^{11}+x^{10}+x^{8}+x^{4}+x^{3}+x^{1}
$$

2. **Multiplicação por $x^n$**: O polinômio de dados é multiplicado por $x^n$, onde $n$ é o grau do polinômio gerador. Em binário é equivalente a adicionar $n$ zeros no final da sequência, pois:
$$
x^n \cdot D(x)=x^{11+n}+x^{10+n}+x^{8+n}+x^{4+n}+x^{3+n}+x^{1+n}
$$

digamos que $n=4$ então a sequência de bits resultado é `1101000110100000`.

3. **Divisão Polinomial**: O polinômio dos dados multiplicado por $x^n$ é dividido pelo polinômio gerador $G(x)$. O resto $R(x)$ da divisão é o valor CRC. Isto é realizado pelo seguinte algoritmo:

    - Alinhe o polinômio multiplicado com o polinômio gerador e realize a divisão usando XOR para subtração.

    - O processo é repetido até que o número de bits restantes seja menor que o número de bits do polinômio gerador.

    - Os bits restantes correspondem ao $R(x)$ sendo 

    $$
    D(x) \cdot x^n = Q(x) \cdot G(x) + R(x)
    $$

    onde $Q(x)$ é o quociente da divisão e $R(x)$ é o resto, que representa o valor CRC. 

4. **Formação da Palavra-Código**: A palavra-código é formada concatenando o polinômio dos dados com o resto $R(x)$.

## Uso

### Definição da Classe

A classe template `CrcClass` é usada para calcular valores CRC. A classe suporta diferentes tamanhos de dados e configurações.

### Exemplo de Uso

```cpp
#include "CrcClass.h"  // Inclua o cabeçalho da classe CRC

int main()
{
    char testArray[14] = { 128, '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'A', 129 };
    
    // Exemplo para CRC-8
    CrcClass<uint8_t> crc8(0x7, 0, false, false);
    uint8_t crc8_result = crc8.calc(testArray, 14);
    
    // Exemplo para CRC-16
    CrcClass<uint16_t> crc16(0x1021, 0xFFFF, false, false);
    uint16_t crc16_result = crc16.calc(testArray, 14);
    
    // Exemplo para CRC-32
    CrcClass<uint32_t> crc32(0x04C11DB7, 0xFFFFFFFF, false, false);
    uint32_t crc32_result = crc32.calc(testArray, 14);
    
    return 0;
}
```

## Compilação

### Usando MINGW64 e g++

1. Instale o [MINGW-w64](http://mingw-w64.org/) e adicione-o ao seu PATH.
2. Abra o terminal MINGW64 e navegue até o diretório do projeto.
3. Compile o código com o comando:

    ```sh
    g++ -o test CrcCalculator.cpp
    ```

4. Execute o programa com:

    ```sh
    ./test
    ```

5. A execução do `test` deve retornar:
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

**Observação**: Outra opção é instalar o compilador g++ e configurar sua execução com o VS Code. 

## Contribuições

Correções e melhorias são sempre bem-vindas!

## Licença

Este projeto está licenciado sob a Licença MIT. Consulte o arquivo [LICENSE](./LICENSE) para obter detalhes.
