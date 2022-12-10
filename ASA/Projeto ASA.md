#ASA 
Projeto biro e gui

## Matriz ajudante - $H_{N \times M}$

A matriz $H_{N \times M}$  helper faz com que não repetimos o código (otimização).

N (linhas) se referem as possibilidades com quadrados de nxn naquela linha.
M (colunas) se referem as linhas da matriz real.

![[Pasted image 20221210195241.png]]

Note que,  a soma das linhas de uma coluna são o resultado de combinações possíveis até aquela linha.

## Algorítmo

### Primeira linha ( de baixo para cima )

Começamos pela primeira linha (de baixo para cima) e vamos até o final verificando se dá pra fazer um quadrado maior que 1x1. (a última é sempre 1). No final da contagem com esse tamanho de quadrado, colocamos na entrada (0,0) da matriz *H*.

![[Pasted image 20221210195625.png]]

### Segunda linha

Agora na segunda linha, o maior quadrado possível é 2x2, começaremos por ele. Verificando todas as hipoteses com quadrados de 2x2, colocaremos na entrada (1,1) da matriz H. Agora ao invés de testar com todos 1x1, vamos adicionar a **soma** de todas as linhas da coluna anterior da matriz H (no caso 1).

![[Pasted image 20221210195919.png]]

### Terceira Linha

Agora na terceira linha, o maior quadrado possível é 3x3, portanto começaremos com ele. Pelo que são 2 combinações possíveis:

![[Pasted image 20221210200034.png]]

Adicionaremos o valor na entrada (2,2) da nossa matriz H.

Continuaremos nossa iteração com o quadrado de tamanho 2x2, ainda na terceira linha.

As iterações possíveis serão iguais ao da linha anterior (4) quando a linha de baixo é vazia + possibilidades quando a linha de baixo não é vazia = 2:

![[Pasted image 20221210200825.png]]

Por fim, quando tentarmos fazer as combinações com todos de 1x1, adicionaremos a soma das linhas da coluna anterior da nossa matriz H:

![[Pasted image 20221210201014.png]]

### Quarta Linha

Fazemos a mesma coisa para os quadrados 4x4 (1 possiblidade) e 3x3 (2 possiblidades) e adicionamos as respetivas entradas na matriz H.

![[Pasted image 20221210210404.png]]
> [!danger] O valor na entrada `(3,4)` da matriz era para ser 2 (escrevi errado)


Porém, agora a calcular as possiblidades com quadrados de 2x2, podemos otimizar ainda mais.

A quantidade de combinações com quadrados de 2x2 ocupando as primeiras linhas vai ser a quantidade de quadrados 2x2 que calculamos na 2ª linha anterior (4) vezes todas as possiblidades da 2ª linha, dado que, para cada possiblidade na linha 1, existirão 5 possibilidades abaixo da linha 2. Dessa forma esses  $4 \times 5 = 20$ casos já estão calculados. Teremos só que calcular os casos em que existe pelo menos 1 quadrado 2x2 começando na próxima linha e um começando na primeira linha (4 casos) = 24.

> [!tip] Reutilização de valores
> Todas as vezes que vamos verificar um caso em que as linhas de baixo são todas vazias, podemos reutilizar um valor previamente calculado. Além disso, deveremos multiplicar cada caso pelo total da linha que começa a estar vazia. (Para cada caso da linha de cima, tem os outros casos que ja calculamos nas linhas vazias de baixo).

### Final

No final, teremos nossa matriz H totalmente preenchida. O resultado será a soma de todas as linhas da última coluna.

![[Pasted image 20221210211834.png]]

