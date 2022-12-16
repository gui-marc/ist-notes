#ASA 

## Maior sub-sequência palíndromo

Dada uma sequência X = ($x_1,...,x_n$), calcular a __maior subsequência__ Z = $(z_1,..., z_n)$ de X tal que Z seja um palíndromo.

### Formulação

`l[i, j]`: tamanho da maior sub-sequencia palindromo de X entre os indices i e j tal que j >= i

```lua
l(i,j)
	if i == j
		return 1
	if xi = xj
		return l(i + 1, j - 1) + 2
	else
		return max(l(i, j - 1), l(i + 1, j))
end
```

Quando os caracteres são iguais

## Algoritmos greedy (ganaciosos)

Não é preciso calcular a solução de varios problemas, somente a solução otima.

A cada passo de eecução do algoritmo escolher opção que localmente se afigura como a mlheor para encontrar __solução optima__

>Estratégia permite obter a solução optima?

### Problema 1) Seleção de Atividades

Seja S = $\{1,2,...,n\}$ um conjunto de atividades que pretendem utilizar um dado recurso.

Apenas uma atividade pode utilizar o recurso de cada vez

Cada atividade $i$:
- Tempo de inicio $s_i$
- Tempode fim $f_i$
- Execução da atividade durante $[s_i,f_i[$

Atividades $i$ e $j$ compativeis apenas se  $[s_i,f_i[$ e  $[s_j,f_j[$ não se intersectam.

__Objetivo__: encontrar conjunto maximo de atividades mutuamente compativeis

#### Solução

Admitir ordenação $f_1 \leq f_2 \leq ... \leq f_n$

Qual a escolha greedy?

> Escolher atividade com o menor tempo de fim

#### Exemplo

| i | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| - | - | - | - | - | - | - | - | - | - | - | - | 
| $s_i$ | 1 | 3 | 0 | 5 | 3 | 5 | 6 | 8 | 8 | 2 | 12 |
| $j_i$ | 4 | 5 | 6 | 7 | 9 | 9 | 10 | 11 | 12 | 14 | 16 |

__Atividades Compativeis__

- {$a_3,a_9,a_11$}
- {$a_1,a_4,a_8,a_11$} (maior subconjunto)
- {$a_2,a_4,a_9,a_11$} (maior subconjunto)
- ...

$s$ - vector com os tempos de inicio
$f$ - vector com os tempos de fim

```lua
selecionar_atividades_greedy(s, f)
	a = s.length
	A = {1}
	j = 1
	for i = 2 to n do
		if si >= fj then
			A = A U {i}
			j = i
		end if
	end for
	return A
end
```

