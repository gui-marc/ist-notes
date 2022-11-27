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

## Operações (Análogo para o min heapx)

- Heap-Maximum(A) -> devolve o valor maximo do amontoado A
- Max-Heapify(A, i) -> corrige uma violação da invariante em $i$
- Build-Max-Heap(A) -> constroi um max-heap a partir de um vetor arbitrario

### Max Heapify

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

$\lim[h -> \infty ] = 2 / 3$

### Build Max Heap

```python
def build_max_heap(A):
	for i in reversed(range(1, heap_size(A) / 2)):
		max_heapify(A, i)
```

> [!info] Porque começa com heap_size(A) / 2 ?
> Começa com o ultimo elemento que tem filhos

__Complexidade__:
- Análise simples:  $\mathcal{O}(n \log n)$
- Possivel provar:  $\mathcal{O}(n)$
	- O tempo de execução de Max Heapify depende da aultura do no onde esta a ser aplicado
	- A altura da maioria dos nos é peuqena, muito inferior a $\log n$

### Heapsort

```c
void heapSort(Item a[], int l, int r) {
	buildheap(a, l, r);
	while (r - l > 0) {
		exch(a[l], a[r]);
		fixDown(a, l, --r, l);
	}
}
```

__Complexidade__: $\mathcal{O}(\log n)$

## Filas de prioridade (FIFO)

Implementa um conjunto de elementos S, em que cada um dos elementos tem associada um valor / prioridade.

__Exemplos__:
- Filas nas finanças
- Escalonamento de processos num computador
- Reencaminhamento de pacotes na rede
- ...

### Operações sobre FIFOs

__MaxHeapInsert__(S, x) - insere o elemento x no conjunto S
HeapMaximum(S) - devolve o elemento de S com o valor maximo
HeapExtractMax(S) - remove e devolve o elemento de S com o valor maximo
HeapIncreaseKey(S, x, k) - incrementa o valor de x com o valor de k

> [!tip] Performance
> Para implementar essas operações de forma eficiente utilizamos um ==Heap==

### Heap Maximum

```c
Item heapMaximun(Item a[]) {
	return a[0];
}
```

__Complexidade__ ($\mathcal{O}(1)$

### Heap Extract Max

```c
Item heapExtractMax(Item a[]) {
	max = a[0];
	a[1] = a[heapSize(a) - 1];
	heapsize(a)
	...
}
```

__Complexidade__: $\mathcal{O}(\log n)$

### Heap Increase Key

```lua
heapIncreaseKey = function(a, i, key)
	if key < a[i] then
		error "new key is smaller than current key"
	end
	a[i] = key
	while i > i and a[parent(i)] < a[i] do
		a[i] = a[parent(i)]
		i = parent[i]
	end 
end 
```

__Complexidade__: $\mathcal{O}(\log n)$

### Max Heap Insert

```lua
maxHeapInsert = function(a, key)
	heapSize(a) = heapSize(a) + 1
	a[heapSize(a)] = - MAX_INTEGER
	heapIncreaseKey(a, heapSize(a), key)
end
```

__Complexidade__: $\mathcal{O}(\log n)$
