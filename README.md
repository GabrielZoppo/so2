# Sistemas Operacionais II
## Plano de Ensino Disponivel Abaixo:
* [Plano de Ensino](http://olaria.ucpel.edu.br/soii/lib/exe/fetch.php?media=plano-soii-2021-1.pdf)

## Encontros: 
* terça Feira das 19h15 às 20h30min
* Google Meet

## Primeira Atividade - Programando com PThreads
### Fundamentação Técnico-Científica:
* <a href="https://computing.llnl.gov/tutorials/pthreads/" target="_blank">Pthreads</a>
* <a href="http://pages.cs.wisc.edu/~travitch/pthreads_primer.html" target="_blank">Pthreads</a>
### Atividade Prática:
* <a href="/soii/lib/exe/fetch.php?media=pt_join.zip" download>Código</a>
* Para compilar utilizar:
* gcc -g -Wall -pthread pt_join.c -lpthread -o pt_join
### Discussão em Aula:**
* Data: 09/03/2020
### Metodologia: 
* explicar o código personalizado para os colegas; 
* empregar um repositório no GitHub para disponibilização dos códigos; A personalização consiste em passar nomes de funções para Português (ou Inglês mais legível), nomes de variáveis, etc.;
* avaliar o tempo de execução do programa variando o número de threads;
* ajustar o número de iterações para que o tempo de execução seja oportuno para uma avaliação, considerando o poder do equipamento utilizado.

### Código Módificado:
~~~C
// declarando as bibliotecas usadas
#include <unistd.h>
#include <stdlib.h>
#include <pthread.h>
#include <math.h>
#include <stdio.h>
#include <time.h>

// definindo uma variavel global de controle
#define Numero_THREADS     4

// declarando variaves globais
int vg;
pthread_mutex_t trava;

// criação de uma função para realizar o calculo pedido
void *GeraThreads(void *threadid)
{
   long k, Numero_Pontos;
   float resultado;
   long tid;
   tid = (long)threadid;


   printf("\nIniciou, thread #%ld!\n", tid);
   
   Numero_Pontos=1000000000/Numero_THREADS; 

   for (k=0; k<Numero_Pontos; k++){
   resultado = sin(1.77)*cos(.99);
   }
    
/* Testa o mutex. Se liberado ativa o mutex e avanca */

    pthread_mutex_lock(&trava);


   printf("Valor vg = %d - Thread #%ld\n", vg,tid);
   vg = vg +1;

   printf("Terminou, thread #%ld!\n", tid);

/* libera o mutex */

   pthread_mutex_unlock(&trava); 

   pthread_exit(NULL);
}

//função main para executar as funções declaradas anteriormente
int main (int argc, char *argv[])
{
//declaração das variaveis necessárias
    pthread_t threads[Numero_THREADS];
    int codigo_Retorno;
    long t;
    long ti, tempog;
    void *status;
    pthread_attr_t attr;
    time_t inicio, fim, reghora;
    struct tm *tmptr;
    
// Rotina para o testar se o programa iniciou e começar a contar o tempo para termino da tarefa
    printf("\n\nPrograma principal iniciou\n\n");
    
    reghora = time(NULL);
    tmptr = localtime(&reghora);
    printf("horario inicio: %s\n", asctime(tmptr));
    
    
   inicio= time(NULL);

    if (pthread_mutex_init(&trava, NULL) != 0)
    {
        printf("\n mutex init failed\n");
        return 1;
    }


// Bloqueia o inicio das threads ateh que todas sejam criadas 

    pthread_mutex_lock(&trava);


// cria as threads 

   vg = 0;

  pthread_attr_init(&attr);
   pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_JOINABLE);


   for(t=0; t<Numero_THREADS; t++){
      printf("In main: creating thread %ld\n", t);
      codigo_Retorno = pthread_create(&threads[t], &attr, GeraThreads, (void *)t);
      if (codigo_Retorno){
         printf("ERROR; return code from pthread_create() is %d\n", codigo_Retorno);
         exit(-1);
      }
   }
    

// liberas as threads para iniciarem 
 
    pthread_mutex_unlock(&trava);


// Rotina para esperar que todas threads terminem 

 	pthread_attr_destroy(&attr);

	for(t=0; t<Numero_THREADS; t++){
	    pthread_join(threads[t], &status);
	}


    fim=time(NULL);

// Rotina para calcular o tempo total de execução do programa
    reghora = time(NULL);
    tmptr = localtime(&reghora);
    printf("\nhorario fim: %s", asctime(tmptr));
    
    fprintf(stdout, "Tempo Gasto = %f\n\n", difftime(fim, inicio));

// fim do programa
    pthread_exit(NULL);
    pthread_mutex_destroy(&trava);
}

~~~

## Segunda Atividade - Explorando conceitos introdutórios em Sistemas Embarcados
### Responder as perguntas abaixo, considerando o texto de Edna Barros e Sérgio Cavalcante da UFPE <a href="http://www.cin.ufpe.br/~vba/periodos/8th/s.e/aulas/STP%20-%20Intro%20Sist%20Embarcados.pdf" target="_blank">Introdução aos Sistemas Embarcados</a> 
* Data da discussão em aula: 16/03/2021
#### Perguntas:
* Caracterize a diferença entre sistemas computacionais convencionais e os sistemas computacionais embarcados.
* Resuma as características dos sistemas embarcados.
* Porque em sistemas embarcados é possível ter algumas funções implementadas diretamente em hardware?
* Explique a razão pela qual não se emprega processadores de propósito geral no desenvolvimento de sistemas embarcados.
* Resuma as diferenças entre os microcontroladores e os processadores de propósito geral (item 2.1.2).
