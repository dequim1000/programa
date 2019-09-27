//Criando uma agenda usando ponteiros
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct cadastro {
    char nome [50];
    char telefone [11];
    char email [50];
    char idade[3];
    struct cadastro *prox;
    struct cadastro *ant;
};

typedef struct cadastro agenda;

void inicializar(agenda *nodo);

/** Funções para o CRUD (cadastro, recuperação, atualização e deleção). */
int inserir_fim(agenda *inicio);
int inserir_inicio(agenda *inicio);
void consultar_todos(agenda *inicio);
int consultar_especifico(agenda *inicio);
int remover(agenda *inicio);
int editar(agenda *inicio);
int inserir_fim_inicio(agenda *inicio);

int main(){

    agenda *inicio = (agenda *)malloc(sizeof(agenda));
    inicializar(inicio);

    if(!inicio){
        printf("Erro ao criar estrutura de dados!\n\n");
        system("pause");
        return 1;
    }

	int opcao = 0;

	do{
        system ("cls");
        printf("|------------------------------------------------- |\n");
        printf("|[1] - Inserir contato no fim                      |\n");
        printf("|[2] - Inserir contato no inicio prioridade maxima |\n");
        printf("|[3] - Consultar todos os contatos                 |\n");
        printf("|[4] - Consultar um contato especifico             |\n");
        printf("|[5] - Editar contato                              |\n");
        printf("|[6] - Remover contato                             |\n");
        printf("|[7] - Inserir contato no inicio prioridade minima |\n");
        printf("|[0] - Sair                                        |\n");
        printf("|--------------------------------------------------|\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        switch (opcao){

            case  0:{
                printf("\nSaindo...\n\n");
                system ("pause");
            }
            break;

            case 1:{
                printf("\nInserir contato no fim:\n\n");
                if(inserir_fim(inicio)){
                    printf("\nContato inserido com sucesso!\n\n");
                }else{
                    printf("\nErro ao inserir contato!\n\n");
                }
                system("pause");
            }
            break;

            case  2:{
                printf("\nInserir contato no inicio:\n\n");
                if(inserir_inicio(inicio)){
                    printf("\nContato inserido com sucesso!\n\n");
                }else{
                    printf("\nErro ao inserir contato!\n\n");
                }
                system("pause");
            }
            break;

            case  3:{
                printf("\nConsultar todos os contatos cadastrados:\n\n");
                consultar_todos(inicio);
                system("pause");
            }
            break;

            case  4:{
                printf("\nConsultar um contato especifico:\n\n");
                if(!consultar_especifico(inicio)){
                    printf("\nContato nao encontrado!\n\n");
                }
                system("pause");
            }
            break;

            case  5:{
                printf("\nEditar contato:\n\n");
                if(editar(inicio)){
                    printf("\nContato editado com sucesso!\n\n");
                }else{
                    printf("\nErro ao editar contato ou contato nao encontrado!\n\n");
                }
                system("pause");
            }
            break;

            case 6:{
                printf("\nExcluir contato:\n\n");
                if(remover(inicio)){
                    printf("\nContato removido com sucesso!\n\n");
                }else{
                    printf("\nErro ao remover contato ou contato nao econtrado!\n\n");
                }
                system("pause");
            }
            case 7:{
                printf("\nPrioritario Minimo:\n\n");
                if (inserir_fim_inicio(inicio)){
                    printf("\nContato incluido com sucesso!\n\n");
                }else{
                    printf("\nErro ao inserir contato !\n\n");
                }
            }
            break;

            default :{
                printf("\nEscolha uma opcao valida!\n\n");
                system ("pause");
            }

        }

    }while(opcao != 0);

    return 0;

}

void inicializar(agenda *nodo){
    strcpy(nodo -> nome, "");
    strcpy(nodo -> telefone, "");
    strcpy(nodo -> email, "");
    strcpy(nodo -> idade, "");
    nodo -> prox = (agenda *)NULL;
    nodo -> ant = (agenda *)NULL;
}

int inserir_fim(agenda *inicio){
    agenda *novo = (agenda *)malloc(sizeof(agenda));

    if(!novo){
        return 0;
    }else{
        inicializar(novo);

        printf("Escreva o nome: ");
        fflush(stdin);
        gets(novo -> nome);
        printf("\nEscreva o telefone: ");
        fflush(stdin);
        gets(novo -> telefone);
        printf("\nEscreva o email: ");
        fflush(stdin);
        gets(novo -> email);
        printf("\nEscreva a idade: ");
        fflush (stdin);
        gets(novo -> idade );

        if(inicio -> prox == (agenda *)NULL){
            inicio -> prox = novo;
            novo -> ant = inicio;
            return 1;
        }else{
            agenda *temp = inicio;
            while(temp -> prox != (agenda *)NULL){
                temp = temp -> prox;
            }
            temp -> prox = novo;
            novo -> ant = temp;
            return 1;
        }

    }

    return 0;

}

int inserir_inicio(agenda *inicio){

	agenda *novo = (agenda *)malloc(sizeof(agenda));

    if(!novo){
        return 0;
    }else{
        inicializar(novo);

        printf("Escreva o nome: ");
        fflush(stdin);
        gets(novo -> nome);
        printf("\nEscreva o telefone: ");
        fflush(stdin);
        gets(novo -> telefone);
        printf("\nEscreva o email: ");
        fflush(stdin);
        gets(novo -> email);
        printf("\nEscreva a idade: ");
        fflush (stdin);
        gets(novo -> idade );

        novo -> prox = inicio -> prox;
        novo -> ant = inicio;
        inicio -> prox -> ant = novo;
        inicio -> prox = novo;

        return 1;

    }

    return 0;

}

void consultar_todos(agenda *inicio){
	agenda *temp = inicio -> prox;

    while (temp != (agenda *)NULL){
        printf("Nome: %s\n", temp -> nome );
        printf("Telefone: %s\n", temp -> telefone);
        printf("Email: %s\n", temp -> email);
        printf("Idade: %s\n\n", temp -> idade);
        temp = temp -> prox;
    }

}

int consultar_especifico(agenda *inicio){
	char nome_consulta[50];

	unsigned char retorno = 0;

	printf("\nEscolha o nome que deseja consultar: ");
	fflush(stdin);
	gets(nome_consulta);

	agenda *temp = inicio -> prox;

	while(temp != (agenda *)NULL){
        if (strcmp(nome_consulta, temp -> nome) == 0){
            printf("Nome: %s\n", temp -> nome );
            printf("Telefone: %s\n", temp -> telefone);
            printf("Email: %s\n", temp -> email);
            printf("Idade: %s\n\n", temp -> idade);
            temp = temp -> prox; /** Exibir contatos com o mesmo nome. */
            retorno = 1;
        }else{
            temp = temp -> prox;
        }
    }

    return retorno;

}
int remover(agenda *inicio){


	char nome_remover[50];

	printf("\nEscolha o nome que deseja remover: ");
	fflush(stdin);
	gets(nome_remover);

	agenda *temp = inicio -> prox;

	while(temp != (agenda *)NULL){
        if(strcmp(nome_remover, temp -> nome) == 0){

            temp -> prox -> ant = temp -> ant;
            temp -> ant -> prox = temp -> prox;

            free(temp);

            return 1;
        }else{
            temp = temp -> prox;
        }
	}

	return 0;

}

int editar(agenda *inicio){

    agenda *temp;
	char nome_consulta[50];

	printf("Escolha o nome que deseja editar: ");
	fflush(stdin);
	gets(nome_consulta);

	temp = inicio -> prox;

	while(temp != (agenda *)NULL){
        if (strcmp(nome_consulta, temp -> nome) == 0){
            printf("\nEscreva o nome: ");
            fflush(stdin);
            gets(temp -> nome);
            printf("\nEscreva o telefone: ");
            fflush(stdin);
            gets(temp -> telefone);
            printf("\nEscreva o email: ");
            fflush(stdin);
            gets(temp -> email);
            printf("\nEscreva a idade: ");
            fflush (stdin);
            gets(temp -> idade);

            return 1;
        }else{
            temp = temp -> prox;
        }
    }

    return 0;

}
int inserir_fim_inicio(agenda *inicio){
    agenda *novo = (agenda *)malloc(sizeof(agenda));
    agenda *temp =  inicio -> prox;
    if(!novo){
        return 0;
    }else{

        inicializar(novo);

        printf("Escreva o nome: ");
        fflush(stdin);
        gets(novo -> nome);
        printf("\nEscreva o telefone: ");
        fflush(stdin);
        gets(novo -> telefone);
        printf("\nEscreva o email: ");
        fflush(stdin);
        gets(novo -> email);
        printf("\nEscreva a idade: ");
        fflush (stdin);
        gets(novo -> idade );

        if(temp != (agenda *)NULL){
        novo->prox = temp->prox;
        novo->ant = temp;
        temp->prox->ant=novo;
        temp->prox=novo;
        return 1;
        }else{
        inicio->prox = novo;
        novo->ant = inicio;
        return 1;
        }
}
    return 0;
}
