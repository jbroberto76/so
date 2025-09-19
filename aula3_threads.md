Sistemas Operacionais

1

Threads
Prof. José Roberto Bezerra

Agenda

 O que são threads?
 Por que utilizar threads?
 Exemplos de uso
 Aplicação online
 Pthreads

2

Threads

3

O que são threads?

 Programas “normais” (single thread),

tem um único fluxo de execução
 Um Thread executa dentro de um

processo

 Também chamados lightweight process

(processos leves)
 São mais fáceis de criar e destruir, pois
praticamente não possuem recursos
associados

 No SO Solaris, estima-se que a criação de
um processo é 30 vezes mais lenta do que
de uma thread

4

O que são threads?

 São úteis em sistemas com múltiplas
CPUs, nas quais o paralelismo real é
possível
 A medida que é possível dividir uma tarefa
em subtarefas, o uso de threads se torna
mais interessante

 É um fluxo único de controle que
compartilha o contexto com um
processo

5

Threads não são processos

 Processos são usados para agrupar

recursos

 Threads são entidades escalonadas para

execução sobre a CPU

 Múltiplos threads executando em

paralelo em um processo é análogo a
múltiplos processos executando em
paralelo em um computador

6

Threads não são processos

 Um thread é semelhante a um processo

 Compartilha o mesmo "espaço de

endereçamento"
 Arquivos abertos
 Processos filhos
 Sinais e alarmes

 O chaveamento entre threads é similar

ao de processos
 Porém mais rápido e menos custoso para
CPU que o chaveamento entre processos

 Cada thread possui um contador de

programa e uma pilha próprios

7

Por que threads?

 São fáceis de criar e destruir

 Praticamente não possuem recursos

associados

 Criar um thread pode ser até cem vezes
mais rápido que criar um novo processo
 Permite que múltiplas execuções de

código ocorram no mesmo ambiente do
processo

 Quando existe uma mistura entre
threads orientados a CPU e E/S há
ganho de desempenho

8

Multithreading

 Suporte a nível de usuário e kernel

 Threads de usuário são permitidas acima do
kernel e não possuem gerenciamento do
kernel
Já as threads de kernel são gerenciadas
pelo próprio SO



 Presente nos SO atuais

 Linux
 Windows XP, 7
 Mac OS
 Solaris
 Tru64

9

Exemplos de uso

 Editor de textos
 Planilha eletrônica
 Servidor web

10

Editor de textos convencional

 Thread única



Interação com usuário (mouse/teclado) e

Formatação

 A remoção de um parágrafo na página 1 terá
impacto na formatação de todas as 800
páginas

 Após o comando a interação com o

documento pode tornar-se lenta, pois o
programa passará a executar a formatação
“deixando” de atender requisições dos
dispositivos e a visualização do documento

11

Editor de textos
multithreaded

 Multithread

 Thread para interação com o usuário
 Thread para formatação do texto

 Exemplo anterior

 Ao remover texto da página 1, o thread

interativo solicita ao thread de formatação
para reformatar o arquivo

 Assim, o thread interativo continua a

interação normal com o usuário
(mouse/teclado/tela)

 Segundo thread continua processamento
nos momentos de inatividade do usuário
 Acrescentando um thread para salvamento
automático teríamos um esquema mais
completo

12

Editor de textos
multithreaded

13

Um esquema de processador de textos com
três processos também funcionaria
adequadamente?

14

Não. Pois, os três threads devem precisam
operar sobre os mesmos dados (documento)
que devem estar compartilhados. Com três
processos teríamos três espaços de
endereçamento distintos, dificultando
operações com o mesmo conjunto de dados.

15

Planilha eletrônica

 Permite manter uma matriz com dados

passados pelo usuário e aplicar fórmulas
complexas obtendo resultados
 A alteração de um dado em uma

planilha traz impacto em diversas
planilhas, necessitando recálculo

 Multithread



Interação

 Cálculos
 Backups automáticos

16

Servidor Web

 Requisições de páginas são feitas a um

servidor web que deve enviá-las de
volta aos clientes

 Algumas páginas são mais demandadas
 Tais páginas são armazenadas na memória

reduzindo o uso do disco (cache)

17

Arquitetura

 Despachante



lê as requisições que
chegam pela rede

 Examina a requisição
e escolhe um operário
para responder

 Operário

 É desbloqueado para
atender a requisição
 Verifica se a página
requisitada está em
cache

 Caso contrário,

18

executa read para ler
o disco (bloqueado)

Arquitetura

 Permite que o

servidor web seja
escrito como uma
coleção de threads
sequenciais



19

Compartilhamento
entre threads

 Cada thread

possui seu próprio
 Contador de
Programa

 Registradores
 Pilha
 Estado

 Todas as threads
de um processo
compartilham
entre si:
 Espaço de

endereçamento
 Variáveis Globais
 Processos Filhos
 Alarmes

pendentes

 Sinais
 Arquivos abertos

20

Compartilhamento
entre Threads

Código
Dados
Arquivos
Registradores
Pilha

Código
Dados
Arquivos
Registradores Registradores Registradores

Pilha

Pilha

Pilha

Thread

Thread 1 Thread 2

Thread 3

21

Proteção

 Todos os threads têm exatamente o
mesmo espaço de endereçamento
 Cada thread pode ter acesso a qualquer

endereço de memória do processo

 Não há proteção de memória entre threads
 Um thread pode ler, alterar e até apagar a

pilha de outro thread

22

Aplicação online

 Exemplo de aplicação

 www.dsc.ufcg.edu.br/~jacques/cursos/map/html/threads/threads1.html

23

POSIX Threads

24

Threads POSIX

 IEEE 1003.1c
 Possibilita a portabilidade em threads
 Define o pacote chamado Pthreads
 Suportado pelos sistemas UNIX
 Possuem propriedades básicas:

○ Identificador
○ Registros
○ Contador de programas

 Utiliza chamadas de sistema

25

Chamadas Pthreads

Chamada
Phtread_create
Phtread_exit
Phtread_join

Phtread_yield

Phtread_attr_init

Phtread_attr_destroy

Descrição

Cria nova thread

Conclui a chamada de thread

Espera que um thread específico
seja abandonado

Libera a CPU para que outra
thread seja executado

Cria e inicializa uma estrutura de
atributos da thread

Remove uma estrutura de
atributos da thread

26

Exemplos

27

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *print_message_function(void *ptr);

void *print_message_function(void *ptr)
{

char *message;

    message = (char *) ptr;
    printf("%s \n", message);
}

28

main()
{
     pthread_t thread1, thread2;
     char *message1 = "T1";
     char *message2 = "T2";
     int  t1, t2;

    /* Cria threads e aponta para suas respectivas funcoes */

     t1 = pthread_create( &thread1, NULL, print_message_function,
(void*) message1);
     t2 = pthread_create( &thread2, NULL, print_message_function,
(void*) message2);

     printf("Thread 1 returns: %d\n",t1);
     printf("Thread 2 returns: %d\n",t2);
     exit(0);
}

29

main()
{
     pthread_t thread1, thread2;
     char *message1 = "T1";
     char *message2 = "T2";
     int  t1, t2;

    /* Cria threads e aponta para suas respectivas funcoes */

     t1 = pthread_create( &thread1, NULL, print_message_function,
(void*) message1);
     t2 = pthread_create( &thread2, NULL, print_message_function,
(void*) message2);

     /* Espera a conclusão das threads para continuar main  */

     pthread_join( thread1, NULL);
     pthread_join( thread2, NULL);

     printf("Thread 1 returns: %d\n",t1);
     printf("Thread 2 returns: %d\n",t2);
     exit(0);
}

30

#define NUM_THREADS     5
void *PrintHello(void *threadid)
{
   long tid;
   tid = (long)threadid;
   printf("Hello World! Thread #%ld!\n", tid);
   pthread_exit(NULL);
}
int main (int argc, char *argv[])
{
   pthread_t threads[NUM_THREADS];
   int rc;
   long t;
   for(t=0; t<NUM_THREADS; t++){
      printf("Function main: criando thread %ld\n", t);
      rc = pthread_create(&threads[t], NULL, PrintHello, (void *)t);
      if (rc){
         printf("ERROR; codigo de erro de pthread_create(): %d\n",
rc);

exit(-1);

      }
   }
   pthread_exit(NULL);
}

31

Bibliografia

 Tanenbaum, Andrew S.
Sistemas Operacionais
Modernos. 3a. Ed.
Pearson, 2010

 Tanenbaum, Andrew S.
Sistemas Operacionais
Modernos. 2a. Ed.
Pearson, 2003

 Silbershatz. Sistemas

Operacionais com java.
7a. Ed, 2008

32

Dúvidas e Perguntas

33

FIM

34

