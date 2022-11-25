#SO #ficheiros 

> [!info] O que é um ficheiro
> É um conjunto de bytes que persiste em um HD e possui alguns **metadados**:
> - Titulo
> - Data de criação
> - Caminho
> - ... etc

## Informação persistente

É a informação que não se perde quando é removida a energia. Inicialmente era mantida em um disco magnético. Geralmente são guardados em objetos fisicos que independem da energia, como os CDS (escreve nas linhas do cd), HDs (gravado magneticamente).

Trabalham em blocos: ao invés de ler e escrever um bit de cada vez, leem varios e enviam varios de uma vez

> [!info] RAM vs Memoria Persistente
> Na memoria ram pode se aceder a um bite aleatorio, em HDs e SSDs isso não é possível.
> A memoria ram é resetada quando desligada, as memorias persistentes não.

## Hierarquia de Memória

Representa a relação entre a memoria de acesso rapido (e de custo elevado) e as de acesso lento (e de custo reduzido).

[Latência de Memórias](https://gist.github.com/jboner/2841832)

## Objeto Ficheiro

A estrutura de dados é normalmente um ==vetor de bytes==

Tem um ==nome==:
- Nome para os utilizados
- Nome para uso interno (id: int)

```c
// Diretório
typedef struct dir {
	string name;
	ficheiro * identificador;
} Directory;
```

```c
// Ficheiro
typedef struct file {
	Metadata meta;
	Bytes ** info; // apontadores para blocos com informações
} File;
```

Para simplificar a organização, os diretórios estao normalmente relacionados de uma forma hierarquica

> [!info] Diretórios
> São ficheiros que apresentam uma lista de atributos associados (outros ficheiros) 
> 
> Todo diretorio ja vem automaticamente com os apontadores dos diretorios '.' e '..' (para poder navegar para trás).

## Ficheiros em Unix

Tipos:
- Normais - sequencia de bytes sem uma organização
- Especiais - perifericos de E/S, pipes, FIFOS, sockets
- Diretório

Quando um processo é iniciado ele inicia 3 ficheiros especiais:
- stdin - *input* para o processo (fd - 0)
- stdout - *output* para o processo (fd - 1)
- stderr - *saida* para assinalar erros (fd - 2)

> [!info] Virtual File System - Unix
> Interface unica para diferenciar diferentes file systems (EXT3, VFAT, EXT4). Virtualiza tudo em uma interface, e o nucleo depois decide a implementação conforme o file system utilizado.
