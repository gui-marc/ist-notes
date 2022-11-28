#ASA 

> Creditos ao [Gonçalo](https://github.com/GSAprod) pelos apontamentos

>[!info] Livro CLRS - Capítulo 15

A propagação dinâmica é parecida com o método divide-and-conquer, no entanto, quando se faz a divisão do problema, os sub-problemas **NÃO** são independentes entre si. 

Os passos para a realização de um algoritmo baseado em programação dinâmica são os seguintes:
1. Caracterizar **estrutura** de uma solução óptima;
2. Definir **recursivamente** o valor de uma solução óptima;
3. Calcular valor da solução óptima utilizando a abordagem ***bottom-up***;
4. Construir a **solução** a partir da informação obtida;

### Exemplo: Sequência de Fibonacci
Calcule o n-ésimo elemento da sequência de Fibonacci.
A sequência de Fibonacci é definida desta forma:
$\text{fib}(n)=\begin{cases} 0 & \text{se }n = 0 \\ 1 & \text{se }n = 1 \\ \text{fib}(n-1) + \text{fib}(n-2) & \text{se }n > 1 \end{cases}$

**Solução recursiva**
```lua
fib(n)
	if n <= 1 then
		return n
	else
		return fib(n-1) + fib(n-2)
	end if
```
Complexidade: $O(2^n)$

**Solução de programação dinâmica**
```lua
fib(n)
	if(n <= 1) then
		return n
	else
		"Alocar vetor f com n+1 posições"
		f[0] = 0
		f[1] = 1
		i = 2
		while i <= n do
			f[i] = f[i - 1] + f[i - 2]
			i = i + 1
		end while
		return f[n]
	end if
```
**Complexidade:** $O(n)$

### Exemplo: Número de combinações
Calcular as combinações de n, k a k:
$C^n_k = \frac{n!}{k!(n-k!)}$

C^n_k = 1 se k=0 ou k=n
C^n_k = C^{n-1}\_{k-1} + C^{n-1}\_{k} se 0 < k < n
C^n_k = 0 caso contrário

**Solução recursiva**
```lua
C(n,k)
	if k = 0 or k = n then
		return 1
	else
		return C(n-1,k-1) + C(n-1,k)
	end if
```

**Solução de Programação Dinâmica**
*Bottom-up* - preencher tabela $(n \times k)$ (triângulo de Pascal)
![[triangulopascal4.webp|300]]
![[organizacao-triangulo-de-pascal.jpg|300]]

# Características da programação dinâmica
(não consegui passar a tempo... eis o que eu percebi)
Consiste principalmente em, ao invés de chamar recursivamente a mesma função, guardar os valores de cálculos anteriores numa estrutura de dados e usá-la para fazer os cálculos seguintes até se chegar à solução final.

**PRÓS**
- Evita cálculos repetidos
- Menor complexidade temporal

**CONTRAS**
- Geralmente maior complexidade espacial

### Exemplo: Problema da mochila
**Definição do problema**
- n objetos (1 -> n) e uma mochila
- Cada objeto tem um valor $v_i$ e um peso $w_i$
- Peso transportado pela mochila não pode exceder $W$
- **Objetivo:** Maximizar o valor transportado pela mochila e respeitar a restrição de peso

**Formalização**
Define-se $x_i = 1$ caso o objeto $i$ está incluído na mochila, e 0 caso contrário.
O objetivo é maximizar $\sum_{i = 1}^{n}(v_ix_i)$
Esta operação está sujeito à soma $\sum_{i = 1}^{n}(w_ix_i) \leq W$, com $x_i \in \{0,1\}$.

**Ideia**
Algoritmo que a cada passo escolhe objeto com o maior valor de $v_i/w_i$.

>[!fail] Problema
>Para este conjunto de objetos:
>1. $v_1 = 8$, $w_1 = 6$;
>2. $v_2 = 5$, $w_2 = 5$;
>3. $v_3 = 5$, $w_3 = 5$
>
>Restrição: $W = 10$
>O primeiro objeto selecionado (objeto 1) impede encontrar solução ótima (com os objetos 2 e 3).

**Solução**
- $\text{v}[i,j]$ = valor máximo que é possível transportar se:
	- o peso limite é $j$, com $j \leq W$;
	- apenas podem ser escolhidos os objetos numerados de 1 a i.
- Solução óptima encontra-se em $\text{v}[n, W]$.

Definição:
- $\text{v}[i, j] = \text{max}(\text{v}[i-1, j], \text{v}[i-1, j-w_i] + v_i)$`
- $v[0, j] = 0, j \geq 0$
- $\text{v}[i, j] = -\infty, j < 0$;
- $\text{v}[i, 0] = 0$

>[!example]
>Usando os objetos:
>- $v_1 = 8$, $w_1 = 6$;
>- $v_2 = 5$, $w_2 = 5$;
>- $v_3 = 5$, $w_3 = 5$;
>
>e a restrição $W = 10$, então a tabela $\text{v}[i, j]$ terá estes valores:
>
> | i\\j | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  |
> | ---- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
> | 0    | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
> |  1    | 0   | 0   | 0   | 0   | 0   | 0   | 8   | 8   | 8   | 8   | 8   | 8   |
> | 2    | 0   | 0   | 0   | 0   | 0   | 5   | 8   | 8   | 8   | 8   | 8   | 13  |
> | 3    | 0   | 0   | 0   | 0   | 0   | 5   | 8   | 8   | 8   | 8   | 10  | 13  | 

**Observações**
A solução recursiva tem tempo de execução exponencial em n e W.
A solução de programação dinâmica tem tempo de execução polinomial em n e W.

### Exemplo: Problema da Mochila (com repetição)
Este problema é parecido com o problema anterior, com a diferença de se poder ter vários objetos do mesmo tipo.

**Solução**
- $\text{v}[j]$ - máximo valor que é possível transportar se o peso limite é $j$, com $j \leq W$
- Solução ótima encontra-se em $\text{v}[W]$

Definição:
- $\text{v}[j] = \text{max}_{1 \leq i \leq n}(\text{v}[j - w_i] + \text{v}[i]), j \leq W$
- $\text{v}[0] = 0$
- $\text{v}[j] = -\infty , j < 0$

>[!example]
>Com os objetos:
>- $v_1 = 3, w_1 = 2$
>- $v_2 = 4, w_2 = 3$
>- $v_3 = 20, w_3 = 7$
>
>e a restrição $W = 10$ (?), então o vetor $\text{v}[j]$ vai ter estes valores:
>
>| j             | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
>| ------------- | --- | --- | --- | --- | --- | --- | --- | --- |
>| $\text{v}[j]$ | 0   | 0   | 3   | 4   | 6   | 7   | 9   | 20    |

### Exemplo: Problema dos trocos
**Definição**
- Dado um conjunto de moedas, denominadas 1,...,n, com valores d_1,...,d_n calcular o ==menor número de moedas== cuja soma de valores é T.

>[!info] 
>Existe um número ilimitado de moedas de cada tipo 1,...n

**Formulação**
- $\text{c}[i, j]$: menor número de moedas necessárias para pagar j unidades, $0 \leq j \leq T$, utilizando apenas moedas com denominação entre 1 e i, $1 \leq i \leq n$
- **Objetivo:** calcular o valor de $\text{c}[n, T]$

Definição:
$\text{c}[i, j] = \begin{cases} 0 & \text{se } i > 0, j = 0 \\ +\infty & \text{se } i = 0 \text{ ou } j < 0 \\ \text{min}(\text{c}[i-1, j], \text{c}[i, j-d_i] + 1) & \text{caso contrário} \end{cases}$

Tempo de execução: $O(n \times T)$

>[!example]
>Com estas moedas:
>
>| Denominação | Valor |
>| --- | --- |
>| 1   | 1c  |
>| 2   | 2c  |
>| 3   | 3c  |
>| 4   | 5c  | 
>
>Obtemos os seguintes valores para $\text{c}[i, j]$:
>
>| i\\j | 0      | 1      | 2      | 3      | 4      | 5      | 6      | 7      | 8      | 9      | 10     |
>| ---- | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
>| 0    | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ | $\inf$ |
>| 1    | 0      | 1      | 2      | 3      | 4      | 5      | 6      | 7      | 8      | 9      | 10     |
>| 2    | 0      | 1      | 1      | 2      | 2      | 3      | ...    |        |        |        |        |
>| 3    | 0      | 1      | 1      | 1      | ...    |        |        |        |        |        |        |
>| 4    | 0      | ...    |        |        |        |        |        |        |        |        |        |
