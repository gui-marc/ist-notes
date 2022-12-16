#SO 

[[Multiprogramação e Programação com processos]] - Processos permitem executar programas em paralelo.

## Tarefa

Fluxos independentes de execução, mas que partilham um contexto comum: as `variaveis globais`, o `heap` e os ficheiros abertos.

![[Pasted image 20221209124707.png]]

## Programação paralela

Permite explorar de forma mais eficiente os processadores actuais com multiplos cores.

![[Pasted image 20221209124851.png]]


> [!info] Definição de tarefa
> Uma tarefa é um fluxo de actividade que executa, no âmbito de um processo já existente, uma função do respectivo programa e partilha o mesmo espaço de endereçamento e os mesmos recursos do processo.

### O que __não__ é partilhado entre tarefas do mesmo processo:

- Pilha (stack)
- Estados dos registros do processador
- Atributos específicos da tarefa

### Um processo Unix

![[Pasted image 20221209125142.png]]

### Especificação Posix de tarefas

Tentativa de unificar a interface das tarefas no Unix. Antigamente, haviam diversos _flavors_ de tarefas nas diferentes distros do Unix.

__Funções Posix__
| Tarefas | Criar | Sincronizar | Transferir Controlo | Adormecer | Terminar |
| - | - | - | - | - | - |
| POSIX | `pthread_create` | `pthread_join` | `pthread_yield` | `sleep` | `pthread_exit` |

### Criar tarefa

```c
int pthread_create(pthread_t *thread, attr *attr, void *(*start), void *arg) 
```

- A tarefa começa chamando a função `start` com o argumento `arg`

### Terminar Tarefa

```c
int pthread_exit(void *value_ptr)
```

- Tarefa termina
- Retorna ponteiro para resultado da função
- semelhante a fazer `return()`.

### Sincronizar tarefa

```c
int pthread_join(pthread_t thread, void **value_ptr)
```

- Tarefa espera ate a tarefa indicada ser terminada
- O retorno é colocado na variavel `value_ptr`

## 