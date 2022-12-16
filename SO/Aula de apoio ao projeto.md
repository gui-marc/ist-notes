#SO 

## 1.1 Cópia de um sistema de ficheiros externo

```c
tfs_copy_from_external_fs(char const * source_path, char const * dest_path);
```

Devolve 0 em caso de sucesso, -1 em caso de erro.

Caso o ficheiro de destino nao exista, deve ser criado. Caso contrario, o conteudo previamente existente deve ser totalmente substituido pelo novo conteúdo.

> Programa de teste que demonstre e verifique a correção da solução proposta.

> [!info] Qualquer coisa que der errado (inesperadamente) deve lançar um `panic`.
> É preferivel imprimir uma mensagem de erro do que ignorar o problema. Senao depois o problema fica muito mais dificil de resolver

## Nomes alternativos e Atalhos

Para criar `hard` ou `soft` links deverao ser implementadas as seguintes funções

```c
int tfs_link(char const * target_file,char const * source_file); // hard link
int tfs_sys_link(char const * target_file, char const * source_file); // soft link
```

Função que permite apagar ficheiros e links

```c
int tfs_unlink(char const * target);
```

> O codigo que veio no inicio pode mudar. Ou seja, é suposto que as funções do codigo inicial sejam extendidas.

## 1.3 Chamadas concorrentes por cliente multitarefa

Permitir multiplas tarefas concorrentes `thread-safe`

Devem ser utilizados trincos logicos ==mutext== ou trincos de leitura-escrita ==rwlock==.

> [!warning] A solução deve maximizar o paralelismo
> Uma solução que impessa esse tipo de concorrência será fortemente penalizada. Só quando temos 2 threads a aceder o mesmo ficheiro ao mesmo tempo é que deveremos intervir, as restantes ocasiões deveriam funcionar sem problemas.

## Submissão e avaliação

Até dia ==19 de dezembro== às __17:00__

Deverá ser entregue um arquivo `.zip` com o codigo e o ficheiro `Makefile`.
- Não incluir outros ficheiros
- `make clean`

>__Recomendação__: verificar se o projeto compila e corre corretamente no __ambiente de referência__.

