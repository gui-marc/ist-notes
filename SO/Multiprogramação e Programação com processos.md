#SO #ficheiros 

## Multiprogramação

Execução de varios programas na mesma maquina. Cada instancia de um programa se denomina **processo**

Parece que há varios programas a correr ao mesmo tempo, mas na realidade é só 1 de cada vez com interrupções

![[Pasted image 20221206153714.png]]

Se for multicore, pode existir mesmo paralelismo real. O restante, é o SO que gere.

![[Pasted image 20221206153939.png]]

> [!tip] Processo
> Entidade activa controlada por um programa que necessita de um processador para se executar.

### Processo x Programa

Programa é uma coisa estatica que esta no disco. O programa não corre. Para ele correr, tem de estar dentro de um processo.

O processo é o que tem a capacidade de executar as funções.

Um processo pode executar diversos programas.

## Exemplo: Unix

```bash
# Lista os processos do utilizador atual
ps -al
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 R  1000   261     9  0  80   0 -  2630 -      pts/0    00:00:00 ps
```

__PID__: Identificador do processo
__TTY__: terminais a que ele esta associado
__UID__: Identificador do utilizador q iniciou o processo

```bash
# Listar TODOS os processos que estao a correr no pc
ps -Al
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0     1     0  0  80   0 -   568 -      ?        00:00:00 init
0 S     0     4     1  0  80   0 -   568 -      ?        00:00:00 init
5 S     0     7     1  0  80   0 -   570 -      ?        00:00:00 init
5 R     0     8     7  0  80   0 -   574 -      ?        00:00:00 init
4 S  1000     9     8  0  80   0 - 59240 futex_ pts/0    00:00:00 fish
0 R  1000   304     9  0  80   0 -  2630 -      pts/0    00:00:00 ps
```

## Processo como uma Maquina Virtual

![[Pasted image 20221206155112.png]]

#### Espaço de Endereçamento

Conjunto de posicoes de memoria acessiveis. Codigo, dados e pilha. A dimensao destas regioes de memoria pode variar ao longo da execução.

#### Reportório de instruções

As instruções executaveis em modo utilizador e as funções do SO.

#### Contexto de execução (estado interno)

Contem toda a informação necessaria para retomar a execução do processo. tem de ser memorizado quando o processo é retirado de execução.

### Espaço de Endereçamento de um Processo

![[Pasted image 20221206155522.png]]

__Heap__: Zona que o programa aloca para variaveis dinamicas (malloc)
```c
node * node_ptr = malloc(sizeof(node));
```

__Pilha__: Zona para variaveis estaticas
```c
int a = 2;
```

## Contexto de Segurança

Cada processo tem associado um Owner que utilizado como responsavel pelas ações do processo.

O dono é atribuido no login e depois gera um identificador (UID) que é utilizado para validar se o usuário tem ou nao permissao para executar as operações de um processo sob o SO.

## Hierarquia de Processos

Um processo pode criar outros processos (subprocessos) os quais herdam o contexto do processo pai. Os processos mantém esta noção de hierarquia geracional

## Objeto "processo"

__Propriedades__:
- Identificador
- Dono
- Programa
- Espaço de endereçamento
- Prioridade
- Processo pai
- Ficheiros abertos
- Quotas de utilização de dados
- Contexto de segurança

__Operações__ sob processos:
- Criar
- Eliminar
- Esperar pelo termino


## Criação de um Processo

```c
int id = fork();
```

Em unix esta primitiva de criação de processos é mais simples do que em Windows, etc. Na realidade, faz um __clone__ do processo que está a correr. Ou seja, cria 2 processos com a mesma coisa a correr.

No subprocesso, é copiado o espaço de endereçamento e o contexto de execução do pai.

A função retorna o PID do processo. Assumindo diferentes valores consoante o processo em que se efetua o retorno:
- ao processo pai é devolvido o "pid" do filho
- ao processo filho é devolvido 0
- -1 em caso de erro

Ou seja, quando eu chamo a função, é criado um outro processo e, portanto, o metodo corre nos 2 processos. No processo pai, retorna o PID do filho e no filho, retorna 0.

```c
// exemplo de fork
main () {
	int pid = fork();
	
	if (pid == -1) // tratar o erro
	if (pid == 0) {
		// codigo do processo filho
	} else {
		// codigo do processo pai
	}

	// pipipi popopo
}
```

> Depois do `fork()` cada processo modifica as regiões de memória autonomamente

## Terminação de um Processo

```c
void exit(int status);
```

Termina o processo, libertando todos os recursos detidos pelo processo. Assinala ao processo pai a terminação (mecanismo de `signal`).

Em programas c, o compilador assegura que a função main chame o `exit()`;

```c
int main_aux(argc, argv) {
	int s = main(argc, argv);
	exit(s);
}
```

