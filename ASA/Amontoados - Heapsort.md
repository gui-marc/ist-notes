#ASA 

Vetor de valores interpretado como uma arvore binaria (essencialmente completa)

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/MinHeapAndMaxHeap.png)

## Árvore binaria completa

Cada no tem exatamente 0 ou 2 filhos

Cada folha tem a mesma profundidade *d*

![](https://cdn.programiz.com/sites/tutorial2program/files/comparison-4.png)

## Árvore essencialmente completa

Os elementos podem ter somente 1 filho
- Se um no tiver apenas um filho, sera o fiho do lado esquerdo
Cada folha pode ter profundidade d ou d - 1
- Qualquer no interno a profundidade d - 1, posicionado a esquerda de qualquer no folha a mesma profundidade

> [!info] Profundidade
> Quantos niveis abaixo de uma determinada folha

## Amontoado - Min-heap

O valor do no e sempre $\leq$ ao valor dos nos descendentes

__Aplicação__: Usado na implementação de filas de prioridade

## Amontoado - Max-heap

| 16 | 14 | 10 | 8 | 7 | 9 | 3 | 2 | 4 | 1 |
| -- | - | - |- | - | -| -| - | - | - |

O valor do no e sempre $\geq$ ao valor dos nos descendentes

### Operações (Análogo para o min heapx)

- Heap-Maximum(A) -> devolve o valor maximo do amontoado A
- Max-Heapify(A, i) -> corrige uma violação da invariante em $i$
- Build-Max-Heap(A) -> constroi um max-heap a partir de um vetor arbitrario

```lua
Max-Heapify(A,i)
	l = left(i)
	r = right(i)
	largest <- i
	if l <= heapsize(A) and A[l] > A[i] then
		largest = l
	end if
	if r <= heapsize(A) and A[r] > A[largest] then
		largest = r
	end if
	if largest != i then
		exhange A[i] <-> A[largest]
		Max-heapify(A, largest)
	end if
```

- Altura do amontoado: h = (log n)
- Complexidade de Max-Heapify: $\mathcal{O}(\log n)$

Numero de elementos de um amontoado = $2^h - 1$

