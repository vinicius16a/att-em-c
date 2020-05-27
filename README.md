//att-em-c
 //verify that all open scopes have been closed
//   objetivo "AA1"
//   autor "Vinicius Bruno Lima Dias"


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define X 70


//*** Declaracoes de tipos *****************************************************
struct tControle {
	char escopo;
};

struct tNo {
	struct tControle dado;
	struct tNo *prox;
};
//variavel global
int val = 0;

//*** Prototipos de funcoes ****************************************************
int menu(void);
void inicializar(struct tNo**);
void push(struct tNo**, struct tNo *);
struct tNo* pop(struct tNo**);
void destruir(struct tNo**);
void resultado();

//*** Bloco Principal **********************************************************
int main(void) {
	struct tNo *pilha, *p;
	int opcao;
	char express[X], valida;
	int  i,tam;
	
	inicializar(&pilha);
	do {
        opcao = menu();
        switch (opcao) {
              case 1:
              		inicializar(&pilha);
					printf("\ndigite a expressao matematica\n");
					fflush(stdin);
					gets(express);
					fflush(stdin);
					p = malloc(sizeof(struct tNo));
					tam = strlen(express);
					val=0;
					for(i=0;i<=tam;i++){
						if ((express[i] == '(') ){
							valida = express[i];
							val++;
							p->dado.escopo = valida;
							push(&pilha, p);
						}
						if ((express[i] == ')') ){
							if(val==0){
								printf("\n string invalida pois ocorre de um fechamento de escopo sem uma abertura \n");
								
							}
							else{
								if(valida != '('){
									printf("\n stirng invalida: os () nao se correspondem \n");
								}
								p = pop(&pilha);
								free(p);
							}
					
						}
					}
					resultado();
					destruir(&pilha);
					val = 0;
              	break;
               case 2:
               		inicializar(&pilha);
					printf("\ndigite a expressao matematica\n");
					fflush(stdin);
					gets(express);
					fflush(stdin);
					p = malloc(sizeof(struct tNo));
					tam = strlen(express);
					val=0;
					for(i=0;i<=tam;i++){
						if ((express[i] == '(') || (express[i] == '[') || (express[i] == '{')){
							valida = express[i];
							val++;
							p->dado.escopo = valida;
							push(&pilha, p);
						}
						if ((express[i] == ')') || (express[i] == ']') || (express[i] == '}')){
						
							if(val=0){
						
								printf("\n string invalida pois ocorre de um fechamento de escopo sem uma abertura \n");
							}
							else{
							
								switch(express[i]){
								
									
									case ')':
										
										if(valida != '('){
											
											printf("\n stirng invalida: os () nao se correspondem \n");
										}
										p = pop(&pilha);
										free(p);
									break;
									case ']':
										
										if(valida != '['){
											printf("\n stirng invalida: os [] nao se correspondem\n");
										}
										p = pop(&pilha);
										free(p);
									break;
									case '}':
										
										if(valida != '{'){
											printf("\n stirng invalida: as {} nao se correspondem \n");
										}
										p = pop(&pilha);
										free(p);
									break;
								}
							}
						}		
				
					}
					resultado();
					destruir(&pilha);
					val = 0;
			        
        }
    } while (opcao != 0);
	return 0;
}

//*** Menu *********************************************************************
int menu(void) {
     int op;
    printf("\n\n*** MENU ***\n\n");
    printf("1. Parte 1\n");
    printf("2. Parte 2\n");
    printf("0. Sair\n\n");
    printf("Escolha sua opcao: ");
    scanf("%d", &op);
    return op;
}

//Inicializar 
void inicializar(struct tNo **topo) {
	(*topo) = NULL;
}

// Push 
void push(struct tNo **topo, struct tNo *novo) {
	novo->prox = (*topo);
	(*topo) = novo;
}

// Pop 
struct tNo *pop(struct tNo **topo) {

	struct tNo *p = (*topo);
	if (p != NULL)
		(*topo) = p->prox;
	val--;	
	return p;
}

// verifica se no final a lista está ou nao vazia
void resultado(){
	if(val>0){
		printf("\n string invalida: ocorreu a abertura de um escopo mas nao o fechamento\n");
	
	}
	else{
		if(val == 0)
			printf("\n STRING VALIDA \n");
	}
}

// Destruir 
void destruir(struct tNo **topo) {
	struct tNo *p = (*topo), *q;	
	while (p != NULL) {
		q = p;
		p = p->prox;
		free(q);
	}
	inicializar(topo);
}

