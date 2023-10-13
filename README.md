#include <stdio.h>
#include <stdlib.h>

//Estrutura do N�
typedef struct no {
    int valor;
    struct no *proximo;
}No;

//Estrutura da Lista
typedef struct {
    No *inicio;
    int tam;
}Lista;
 
//Fun�ao para criar a Lista
void criar_lista(Lista *lista) {
    lista->inicio = NULL;
    lista->tam = 0;
}

//Fun��o de inser��o no in�cio da lista
void inserir_no_inicio(Lista *lista, int num) {
    //Alocar espa�o de mem�ria para o item
    No *novo = malloc(sizeof(No));

    //Se for um item vazio
    if(novo) {
        //Insere o valor que o usuario enviou no N�
        novo->valor = num;
        //Item novo aponta para o inicio ANTIGO da lista
        novo->proximo = lista->inicio;
        //Inicio da lista NOVO recebe o novo n�
        lista->inicio = novo;
        //Incremento o tamanho da lista
        lista->tam++;

        printf("\nItem inserido com sucesso!\n\n");

    } else {
        printf("Erro ao alocar memoria!");
    }

    getch();

}

//Fun��o para inserir o item no FIM da lista
void inserir_no_fim(Lista *lista, int num) {
    //Alocar espa�o de mem�ria para o item NOVO e para o ponteiro Auxiliar
    No *novo = malloc(sizeof(No));
    No *aux = malloc(sizeof(No));

    //Se for um item vazio
    if (novo) {
        //Insere o valor que o usuario enviou no N�
        novo->valor = num;
        //Se ele � o ultimo item, ele aponta para NULL
        novo->proximo = NULL;

        //Ele � o primeiro item? SIM!
        if (lista->inicio == NULL) {
            lista->inicio = novo;
        //Ele � o primeiro item? N�O!
        } else {
            aux = lista->inicio;
            //Encontrar o ultimo item da lista
            while (aux->proximo) {
                aux = aux->proximo;
            }
            //Inserir o item no fim da lista
            aux->proximo = novo;
        }

        lista->tam++;

        printf("\nItem inserido com sucesso!\n\n");
    } else {
        printf("Erro ao alocar memoria!");
    }

    getch();
}

//Fun��o para inserir de forma ordenada na lista
void inserir_ordenado(Lista *lista, int num) {
    //Alocar espa�o de mem�ria para o item NOVO e para o ponteiro Auxiliar
    No *novo = malloc(sizeof(No));
    No *aux = malloc(sizeof(No));

    if (novo) {
        //Insere o valor que o usuario enviou no N�
        novo->valor = num;
        //Se a minha lista est� vazia
        if (lista->inicio == NULL) {
            novo->proximo = NULL;
            lista->inicio = novo;
        //Se o primeiro item da lista for maior que o item novo
        } else if (novo->valor < lista->inicio->valor) {
            novo->proximo = lista->inicio;
            lista->inicio = novo;
        //Se o primeiro item da lista for menor ou igual
        } else {
            //Auxiliar recebe o inicio da lista
            aux = lista->inicio;
            //Enquanto o numero do usu�rio for maior que o item auxiliar, o auxiliar anda para o pr�ximo item
            while (aux->proximo && novo->valor > aux->proximo->valor) {
                    aux = aux->proximo;
            }
            //Se chegou aqui � porque o auxiliar encontrou um numero maior que o novo, ou seja, o local onde eu tenho que colocar o novo item
            novo->proximo = aux->proximo;
            aux->proximo = novo;
        }

        lista->tam++;

        printf("\nItem inserido com sucesso!\n\n");

    } else {
        printf("Erro ao alocar memoria!");
    }

    getch();

}

//Fun��o para excluir um item da lista
No* remover(Lista *lista, int num) {
    No *aux = NULL;
    No *remover = NULL;

    if (lista->inicio) {
        //Se o numero a ser removido for o inicio da lista
        if (lista->inicio->valor == num) {
            remover = lista->inicio;
            lista->inicio = remover->proximo;
            lista->tam--;
        //Se o numero a ser removido N�O for o inicio da lista
        } else {
            aux = lista->inicio;

            while (aux->proximo && aux->proximo->valor != num) {
                aux = aux->proximo;
            }
            if (aux->proximo){
                //Encontrei o item
                remover = aux->proximo;
                //Organizando os ponteiros
                aux->proximo = remover->proximo;
                lista->tam--;
            }
        }
    }
    return remover;
}

//Fun��o para buscar na lista
No* buscar(Lista *lista, int num) {
    No *aux = NULL;
    No *no = NULL;

    aux = lista->inicio;

    while (aux && aux->valor != num) {
        aux = aux->proximo;
    }

    if (aux) {
        no = aux;
    }

    return no;
}
//Fun��o para mostrar a lista
void imprimir_lista(Lista lista) {
    //Recebendo o inicio da lista
    No *no = lista.inicio;
    printf("\nLista de tamanho %d: ", lista.tam);

    //La�o para mostrar os itens
    while (no) {
        printf("%d ", no->valor);
        no = no->proximo;
    }

    printf("\n\n");

    getch();
}



int main () {
    int opcao, valor;
    No *noAux;
    Lista lista;

    criar_lista(&lista);

    do {
        system("cls");
        printf("---------- LISTA SIMPLESMENTE ENCADEADA ----------");
        printf("\n\n1 - Inserir no inicio da lista;");
        printf("\n2 - Inserir no final da lista;");
        printf("\n3 - Inserir de forma ordenada na lista;");
        printf("\n4 - Remover item;");
        printf("\n5 - Imprimir lista;");
        printf("\n6 - Buscar na lista;");
        printf("\n7 - Sair;");
        printf("\n\nInforme a opcao desejada: ");
        scanf("%d", &opcao);

        switch (opcao) {
        case 1:
            //Inserindo no come�o da lista
            printf("\nDigite um valor: ");
            scanf("%d", &valor);

            //Enviando o valor para a fun��o de inserir no inicio
            inserir_no_inicio(&lista, valor);
            break;

        case 2:
            //Inserindo no come�o da lista
            printf("\nDigite um valor: ");
            scanf("%d", &valor);

            //Enviando o valor para a fun��o de inserir no inicio
            inserir_no_fim(&lista, valor);
            break;


        case 3:
            //Inserindo na ordem da lista
            printf("\nDigite um valor: ");
            scanf("%d", &valor);

            //Enviando o valor para a fun��o de inserir no inicio
            inserir_ordenado(&lista, valor);
            break;

        case 4:
            //Remove um item da lista
            printf("\nDigite um valor a ser removido: ");
            scanf("%d", &valor);
            //A fun��o vai encontrar o item que ser� removido e ser� colocado na variavel "removido"
            noAux = remover(&lista, valor);

            if(noAux) {
                printf("\nElemento a ser removido: %d\n", noAux->valor);
                free(noAux);
            } else {
                printf("\nO elemento nao existe na lista!\n");
            }

            getch();

            break;
        case 5:
            //Mostrar lista na tela
            imprimir_lista(lista);
            break;

        case 6:
            printf("\nInsira o valor a ser buscado: ");
            scanf("%d", &valor);
            noAux = buscar(&lista, valor);

            if(noAux) {
                printf("\nValor encontrado: %d.\n", noAux->valor);
            } else {
                printf("\nValor nao encontrado!\n\n");
            }

            getch();

            break;
        default:
            printf("\nEscolha uma opcao valida!\n\n");

        }
    }while(opcao != 0);

    return 0;

}

