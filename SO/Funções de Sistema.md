#SO #system-call 

## Timer

Define um tempo em que cada 'utilizador' pode se beneficiar da execução do processador

Funciona com poucas aplicações, mas com algumas centenas de programas: 
- Consome muita memória ( Cada programa é armazenado integralmente na memoria )

## As funções Sistema
System Calls

> [!info] O que são
> Funcões para fazer *tudo* no Sistema

As maiores diferenças entre o sistema Unix tradicional e os SOs atuais sao a interface, o restante das operações são semelhantes.

> [!info] Everything is a file
> Ao invés de criar novas interfaces, os desenvolvedores preferiram reutilizar a interface dos ficheiros. Simplifica tanto a interface (reuzabilidade) e as system calls (uma system call pode funcionar para várias coisas)

> [!example] Exemplo
> No linux, o gestor de dispositivos e de ficheiros é praticamente igual, é como se fosse uma interface de ficheiros (e.g. Dolphin)

## Exemplo em C

Aplicação que lê um ficheiro e o escreve em outro com command line, exemplo:
`app ficheiro-entrada ficheiro-saida`

```c
// argc: numero de argumentos
// argv: lista de argumentos onde o primeiro é o nome do programa
// fopen: função do c
// open: system call
int main(int argc, char *argv[]) {
	...
	
	int inputFd = open(argv[1], O_RDONLY);
	// as system calls muitas das vezes falham
	// o 'inputFd' é um file descriptor
	// - identificador que permite continuar a trabalhar com o ficheiro
	
	// é necessário verificar por erros pos as system calls não os tratam
	// geralmente quando as system calls retornam um numero negativo, é um erro
	if (inputFd == -1) errExit("opening file %s", argv[1]);

	// O pipe | serve para concatenar essas mascaras de bits utilizadas como flags
	openFlags = O_CREAT | O_WRONLY | O_TRUNC;
	filePErms = S_IRUSR | s_IWUSR | ...
	

	// A system call 'open' também pode ser usada para criar ficheiros
	// truncar -> apagar e começar do início
	int outPutFd = open(argv[2], openFlags, filePerms);

	...
}

```

> [!info] Exemplo de Everything is a file neste programa
> Ao passar como argumento `./copy teste /dev/tty` a máquina *escreve* o conteudo de teste no stout, como se fosse um ficheiro.
> 
> O mesmo acontece se utilizarmos o stin como input em `./copy /dev/tty teste` basicamente vai copiar tudo do stdin até chegar em um EOF (^C) e copia tudo para o ficheiro de teste.

### Método ==read== #system-call 

```c
int numRead = read(inputFile, buffer, BUF_SIZE);
// A system call 'read' lê o conteudo de um ficheiro e retorna (em Bytes):
// -1 -> quando ha um erro no buffer
// 0 -> caso seja o fim do ficheiro
// BUF_SIZE -> caso tenha mais coisas para ler (lê um BUF_SIZE por vez)
// 0 < i < BUF_SIZE -> caso não exista um BUF_SIZE inteiro
```

### Método ==write== #system-call 

```c
int numWrote = write(outputFile, buffer, numRead);
// A system call 'write' escreve em um ficheiro
// numRead -> caso tenha ocorrido a escrita de forma correta
// n != numRead -> caso não tenha conseguido escrever todo o buffer (e.g falta HD)
```

## Como pode o SO garantir a segurança ?

> [!info] Sistema de autorizações
> Uma parte da memória que está protegida e não pode ser alterada.

Exemplo do [Método open]:

o metodo `open` chama a rotina `open_syscall` que chama a real `sys_open` do SO.

Isso tudo ocorre em uma tabela de interrupções que pega a exceção lançada pelo `open_syscall` e muda de ==Modo de Utilizador== para ==Modo Núcleo== como se fosse um `super` ou `root`.

> [!example] Como os hackers burlam isso - Buffer overflow
> Antigamente utilizavam um input com um número muito grande, que ultrapassava o buffer. A parte de excedia era passada ao modo núcleo e assim poderiam fazer alterações nocivas ao computador do utilizador.


> [!info] Modo Núcleo - Tratamento de Exceções
> No modo núcleo, exceções simples como numero de argumentos em uma função não são tratados.