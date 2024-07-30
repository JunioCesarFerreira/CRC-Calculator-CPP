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

### O que é CRC?

CRC é um código de detecção de erros popular utilizado em redes digitais e dispositivos de armazenamento. Ele funciona anexando uma sequência de bits redundantes (bits CRC) ao final do bloco de dados, formando uma palavra-código. Quando a palavra-código é recebida ou lida, o mesmo algoritmo de CRC é utilizado para calcular a soma de verificação. Se a soma de verificação calculada coincidir com os bits CRC anexados, os dados são considerados intactos.

### Como o CRC Funciona

1. **Representação dos Dados**: Os dados a serem transmitidos são representados como um polinômio binário.
2. **Divisão Polinomial**: O polinômio dos dados é dividido por um polinômio predeterminado (polinômio divisor) usando divisão binária.
3. **Cálculo do Resto**: O resto da operação de divisão é o valor CRC.
4. **Transmissão**: O valor CRC é anexado aos dados e enviado como uma palavra-código.
5. **Verificação**: No destino, o mesmo processo de divisão é realizado na palavra-código recebida. Se o resto for zero, os dados são considerados válidos.

### Reflexão

Reflexão refere-se a inverter a ordem dos bits dos bytes dos dados antes do cálculo do CRC. Isso é frequentemente usado para simplificar a implementação em hardware.

### Detalhes Teóricos

O CRC é baseado em operações aritméticas em um corpo finito, mais especificamente no corpo de Galois, denotado $GF(2)$. A divisão polinomial usada no CRC opera com coeficientes em $\mathbb{Z}_2=\{0,1\}$, o que simplifica o cálculo.

#### Polinômio Gerador

O polinômio gerador é um polinômio binário fixo usado como divisor na operação de divisão polinomial. Ele é representado na forma:

$$
G(x) = x^n + g_{n-1}x^{n-1} + \ldots + g_1x + g_0
$$

onde $n$ é o grau do polinômio e $g_i$ são os coeficientes binários.

#### Cálculo do CRC

1. **Representação dos Dados**: Os dados $D(x)$ são representados como um polinômio binário.

2. **Multiplicação por $x^n$**: O polinômio dos dados é multiplicado por $x^n$ (equivalente a adicionar $n$ zeros ao final dos dados).

3. **Divisão Polinomial**: O polinômio dos dados multiplicado por $x^n$ é dividido pelo polinômio gerador $G(x)$. O resto $R(x)$ da divisão é o valor CRC.

4. **Formação da Palavra-Código**: A palavra-código é formada concatenando o polinômio dos dados com o resto $R(x)$.

O processo pode ser descrito como:

$$
D(x) \cdot x^n = Q(x) \cdot G(x) + R(x)
$$

onde $Q(x)$ é o quociente da divisão e $R(x)$ é o resto, que representa o valor CRC.

#### Reflexão

Refletir os bits de um valor envolve inverter a ordem dos bits. Isso é usado para compatibilidade com certas implementações de hardware que processam bits em ordem inversa.

## Uso

### Definição da Classe

A classe template `CrcClass` é usada para calcular valores CRC. A classe suporta diferentes tamanhos de dados e configurações.

```cpp
template<class data_t>
class CrcClass
{
  // Definição da classe conforme fornecida no código-fonte
};
```

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

## Testes

O código fornecido inclui um programa de testes abrangente que valida a implementação do CRC contra valores conhecidos.

## Contribuições

Correções e melhorias são sempre bem vindas!

## Licença

Este projeto está licenciado sob a Licença MIT. Consulte o arquivo [LICENSE](./LICENSE) para obter detalhes.
