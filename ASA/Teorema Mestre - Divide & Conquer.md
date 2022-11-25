 #ASA

## Exemplo 4 - Multiplicação de Matrizes

$C = A \times B$

Posso multiplicar as matrizes em blocos (sub-matrizes)

$$\begin{vmatrix}c11 & c12\\
c21 & c22
\end{vmatrix}= \begin{vmatrix}a11 & a12\\ 
a21 & a22
\end{vmatrix} \times \begin{vmatrix}b11 & b12\\
b21 & b22
\end{vmatrix}$$

Algoritmo recursivo:
- Partir de cada matriz *n $\times$ n* em 4 matrizes, cada uma com dimensões $n/2 \times n/2$ , até as matrizes terem a dimensão $n = 1$
- Efetuar *bottom-up* a multiplicação de 2 matrizes $n \times n$ através de 8 multiplicações de matrizes $n/2 \times n/2$ (e mais 4 somas)

$T(n) = 8T(n/2) + \Theta (n^2)$
- $a = 8, b = 2, d = 2$
- $d = 2 < \log2(8)$

### Algoritmo de Strasen

Permite reduzir o numero de multiplicações de 8 para 7, à custa de introduzir um numero constante de adições.

Com $\Theta (n^{\log[b](a)}) = \Theta (n^{2.81})$
