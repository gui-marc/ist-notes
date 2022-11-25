#SO #ficheiros

Ver [[Sistema de Ficheiros]] e [[Funções de Sistema]]

Em linguagens mais antigas é uma funcionalidade proporcianada por bibliotecas explicitamente incluidas no programa

```c
#include <stdio.h>
#include <stdlib.h> // <--- here 

FILE *fp;

fp = fopen("file.txt", "r");
```

> [!hint] Programas gerlamente retornam -1 quando ha erro e 0 quando status é OK

## Funções relacionadas ao [[Sistema de Ficheiros]]

Podemos dividir estas funções em 6 grupos:
- Abertura, criação e fecho
- Operação entre ficheiros
- Operações complexas
- Op sobre diretorios
- Op de gestao do sistema de ficheiros
- ==Acesso a ficheiros mapeados em memória==

> [!info] Lazyness
> O SO geralmente so faz as coisas quando precisa. Por exemplo, só salva um ficheiro quando é pedido. A parte boa é que tudo anda mais rápido, mas caso haja algum erro, tudo pode ser perdido facilmente.
> Existem algumas funções que forçam o SO a salvar a mudar esse comportamento como por exemplo o `fflush()`

## Abertura, criação e fecho

Abrir e Criar em Linux é a mesma função `open`

Por cada `processo` é mantida uma tabela de ficheiros abertos. (Tabela de Ficheiros Aberto). Como *Everything is a File*, podem estar nessa tabela ficheiros, sockets, diretorios, etc...

### Abrir um ficheiro

__1. Pesquisar o Diretório__: visualiza na Tabela de Ficheiros abertos o diretorio. (por cada diretorio)
__2. Verificar se o processo tem permissões__
__3. Copiar o *inode* (metadata) para a memoria__
**4. Devolve ao utilizador um identificador:**

> [!tip] Processo -> instancia de um programa em execução

> [!info] iNode
> Estrutura que contem os metadados de uma file (existe em praticamente tudo pq EIF)

## Autenticação de Procesos

Cada processo corre em nome de um utilizador (UID) ou grupo (GID).

### Matriz de permissões

É uma matriz em que existem os utilizadores e os objetos e suas permissões, ex:

| Utilizadores | Objeto 1 | Objeto 2 | Objeto 3 |
| -------- | -------- | --------- | -------- |
| A | ler | . | escrever | 
| B | - | ler/escrever |

### Grupos

Ao inves de guardar uma lista com todos os utilizadores, guardam uma lista com grupos (e.g sudoers), com os 3 bits de permissoes read, write, execute.

### ACL dos ficheiros em Unix

Nos SGF utiliza-se normalmente listas de controle de acesso ACL que são guardadas na metadata dos ficheiros.

| user | group | others |
| ----- | ----- | ---- |
| rw- | r-- | r-- |

```bash
ls -l
total 7
-rw-r--r-- l user 12301 113 nov 20  2020 file.txt
```

> [!info] Group
> Cada utilizador so pode estar em 1 grupo (??) e possui todos os provilégios deste grupo

### Cursor

Para qualquer ficheiro aberto, o core mantem um cursor

> [!info] Cursor
> Indice que indica a posição no ficheiro onde a proxima operação de `read()` ou `write()` se executará.

O cursor pode ser alterado explicitamente pelo programa com o comando:

`int lseek(int fd, off_t offset, int whence)`

__Offset__: posição do cursor em relação ao inicio
__Whence__: posição de referência -> o offset conta a partir daqui.

Por exemplo, em uma memoria com 100 posicoes, se o whence for 10 e o offset for 20, o cursor começa em 30 (analogia).

## Diretorios e Nomes de Ficheors

Pode-se denominar o mesmo ficheiro com vários nomes ==links==

A maneira mais simples de fazer isso é: 2 inodes de um diretório apontam para o mesmo ficheiro

> [!example] Hardlink
> Existem 2 links: /a/link_1 e /b/link_2 que apontam para o mesmo *inode* no disco (é uma cópia)

Se apagarmos um ficheiros com varios ==hardlinks== ele na realidade não é removido. Apenas o seu link é removidos.

> [!info] Hardlink
> Como o que liga o ficheiro ao disco é o *inode*, o hard link apenas faz varios ficheiros apontarem para o mesmo inode. Ou seja, quando um link é removido, o inode ainda está "vivo" em outros links e portanto o ficheiro ainda continua existente.

> [!info] Softlink
> No softlink liga-se apenas ao diretorio do ficheiro, ou seja, se ele for removido, o inode é removido mas a ligação continua a apontar para um local "vazio"
