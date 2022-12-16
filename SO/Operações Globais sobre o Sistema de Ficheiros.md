#SO #system-call #ficheiros 


## Montar um volume na hierarquia de diretorios
`mount`

Nos permite fazer a relação entre o que a maquina conhece como sistema de ficheiros e o sistem apropriamente dito

> [!example] Mount
> Pega numa hierarquia de ficheiros e mapeia para outro lugar, como se fosse o mesmo sistema de ficheiros.
> O utilizador nao percebe que na realidade existem varios discos compartilhando o sistema de ficheiros que ele esta a utilizar
> 
> `mount -t <filesystem> /dev/hd1 /b`


> [!tip] É possivel listar o que esta montado no sistemas, com o comando `mount` sem parâmetros

## Organização do Disco

É feita em blocos de tamanho fixo, para otimizar o acesso ao disco

> [!info] Bloco
> Unidade minima que pode ser indexada diretamente pelo sistema operativo.
> - Quanto maior for o bloco, maior a taxa de transferencia bruta -> taxa de transferencia efetiva depende do numero de bytes dentro do bloco com informação util.
> - Se um bloco estiver metade cheio sua transferencia efetiva +e metade da transferencia bruta. Ou seja, quanto maior o bloco, maior a diferença da taxa de transferencia bruta e efetiva.
>   
> - Se um bloco for pequeno demais, o sistema necessita de muitos indices para mapear os blocos. (tambem é ruim).
>    
> **Tamanho tipico: `4kb`**

### Partição

Subdivizão logica de um dispositivo fisico, dividido em blocos de tamanho fixo.

É vista pelo SO como um vetor de blocos organizados a partir do numero zero.

Cada partição é um contexto *independente* das outras, nao existindo ficheiros repartidos por partições diferentes. Ou seja, um ficheiro ==nunca== pode estar em mais que uma partição ao mesmo tempo.

> É como se tivesse feito uma divisao fisica no disco e tivesse naquela divisao uma partição.

![](https://www.researchgate.net/publication/281843624/figure/fig2/AS:669967733227532@1536744163505/Example-hard-disk-volume-organized-into-three-partitions.ppm)


## Organização dos ficheiros

Dada esta organização, um probelma interessante é como organizar os ficheiros.

### Organização em Lista
*Solução #1*

![[Pasted image 20221129160215.png]]


> [!warning] Limitações
> - É necessario percorrer todos os ficheiros comparando os nomes um a um
> - Impossível reaproveitar totalmente o espaço previamente ocupado por ficheiros grandes
> - A extensão de um ficheiro é limitada (pois 'bateria' no inicio do proximo ficheiro)

### Tabela de Diretorios
*Solução #2*

![[Pasted image 20221129160613.png]]

> [!success] Pros
> - Resolve o problema da pesquisa de nomes. Agora possui O(1)
> - Resolve o problema da fragmentação externa e do crescimento dos ficheiros. (um ficheiro so utiliza os blocos necessarios por ele)

> [!warning] Contras
> - Ou as tabelas sao muito grandes para guardarem muitos indices, ou os blocos sao muito grandes para os indices caberem na tabela.


### File Allocation Table (FAT)
*Solução #3*

![[Pasted image 20221129161409.png]]

A tabela de alocação possui uma estrutura de indices, com os indices dos blocos.

Primeiro ia para o diretorio descobrir o nome, depois ia com o nome para a tabela e descobria os blocos associados ao ficheiro com o dado nome.

> [!success] Pros
> - Possuía dimensao suficiente para caber na memoria `RAM`.
> - Tem tantas entradas quanto o numero de blocos de dados na partição do disco.

> [!warning] Contras
> Funciona muito bem quando a tabela tem tamanho para ser mantida permanentemente na memoria RAM. Se a FAT precisar ser armazenada em disco, o sistema perde muito desempenho.

### Referenciação dos Blocos do Ficheiro em Unix

Quando o ficheiro é muito pequeno, vai direto ao bloco do ficheiro (iNode)

![[Pasted image 20221129162146.png]]

Se o bloco for um pouco maior, ele vai ao primeiro bloco de indice e busca os indices dos blocos do ficheiro.

Se for maior ainda, vai ao segundo bloco de indicese faz o mesmo.

### iNode (index node)

C