#include <stdio.h>
#include <stdlib.h>
#include <locale.h>

//jogo da velha
//matriz global X O " "
char jogo[3][3];
int vitX = 0;
int vitO = 0;
int empat=0;
int l,c;
// procedimento para inicializar todas as posiçoes da matriz

void matrizinicial(){
    for(l=0;l<3;l++){
        for(c=0;c<3;c++){
             jogo[l][c] = ' ';

        }
    }

}
//função para imprimir o jogo

void imprimirjogo(){
    printf("\n\n\t 0   1   2\n\n");
    for(l=0;l<3;l++){
        for(c=0;c<3;c++){
            if(c == 0){
                printf("\t");
            }
            printf(" %c ",jogo[l][c]);

             if(c < 2){
                printf("|");
            }
            if(c == 2){
                printf("   %d",l);
            }

        }
        printf("\n");
        if(l < 2){
            printf("\t-----------\n");
        }
    }

}
/*função para verificar vitoria por linha
   1- ganhou
   0-não ganhou */

int ganhouLinha(int l, char a){

    if(jogo[l][0] == a && jogo[l][1] == a && jogo[l][2] == a){
        return 1;
    }
    else
        return 0;


}
int ganhouLinhas(char a){
    int ganhou= 0;
    for(l=0; l<3; l++){
        ganhou+=ganhouLinha(l,a);
    }
    return ganhou;

}
/*função para verificar vitoria por coluna
   1- ganhou
   0-não ganhou */
int ganhouColuna(int c,char b){

    if(jogo[0][c] == b && jogo[1][c] == b && jogo[2][c] == b){
            return 1;
        }
        else
            return 0;

}
int ganhouColunas(char b){
    int ganhou= 0;
    for(c=0; c<3; c++){
        ganhou+=ganhouColuna(c,b);
    }
    return ganhou;

}
/*função para verificar vitoria na diagonal principal
   1- ganhou
   0-não ganhou */

int ganhouDiagonalPrin(char f){

    if(jogo[0][0] == f && jogo[1][1] == f && jogo[2][2] == f){
        return 1;
    }
    else
        return 0;
}
/*função para verificar vitoria na diagonal secundaria
   1- ganhou
   0-não ganhou */

int ganhouDiagonalSec(char f){

    if(jogo[0][2] == f && jogo[1][1] == f && jogo[2][0] == f){
        return 1;
    }
    else
        return 0;
}
/* função para verificar coordenadas
1- valida
0- não é valida
*/
int coordenada(int l, int c){
    if(l >= 0 && l <3 && c >= 0 && c < 3 && jogo[l][c] == ' '){
        return 1;
    }
    else
        return 0;
}
//Função para ler as coordenadas
void lercoordenada(char k){
    int linha=0,coluna=0;
    printf("\n\n  Digite a linha e a coluna: ");
    scanf("%d%d",&linha,&coluna);

    while(coordenada(linha,coluna)== 0){
        printf("\n Coordenada invalida. Digite a linha e coluna: ");
        scanf("%d%d",&linha,&coluna);
    }
    //para salvar o que o jogador jogou seja x ou o
    jogo[linha][coluna] = k;
}
//Função se deu velha(Empate)
int velha(){
     int quant =0;
     for(l=0;l<3;l++){
        for(c=0;c<3;c++){
             if(jogo[l][c] == ' '){
                quant++;
             }

        }
    }
    return quant;

}
//Função de jogo em loop
void jogar(){
    int jogador = 1, vitoriaX = 0, vitoriaO = 0;
       do{
            system("CLS");
            printf("\n ********* JOGO DA VELHA *********");
            printf("\n\n  REGRAS:\n  1 - O primeiro a jogar sempre sera o 'X' e o segundo a 'O'.\n  2 - Para jogar coloque o número da linha primeiro e depois da coluna.\n");
            printf("\n");
            imprimirjogo();
           if(jogador == 1){
                lercoordenada('X');
                jogador++;
                vitoriaX+= ganhouLinhas('X');
                vitoriaX+= ganhouColunas('X');
                vitoriaX+= ganhouDiagonalPrin('X');
                vitoriaX+= ganhouDiagonalSec('X');
           }
           else{
                lercoordenada('O');
                jogador =1;
                vitoriaO+= ganhouLinhas('O');
                vitoriaO+= ganhouColunas('O');
                vitoriaO+= ganhouDiagonalPrin('O');
                vitoriaO+= ganhouDiagonalSec('O');
           }
        }while(vitoriaX == 0 && vitoriaO == 0 && velha() > 0);
        imprimirjogo();
        if(vitoriaX == 1){
            printf("\n\n #### JOGADOR 1 VENCEU! ####");
            vitX++;
        }
         else if(vitoriaO == 1){
            printf("\n\n#### JOGADOR 2 VENCEU! ####");
            vitO++;
        } else{
            printf("\n\n#### QUE PENA EMPATOU! DEU VELHA! ####");
            empat++;
        }
}

int main()

{
    int cont=0;
    setlocale(LC_ALL,"portuguese");
    do{

        system("CLS");

        matrizinicial();
        jogar();
        printf("\n\nJogador 1 (X) venceu %d vezes.", vitX);
        printf("\nJogador 2 (O) venceu %d vezes.", vitO);
        printf("\nEmpatou %d vezes",empat);

        printf("\n\n");
        printf("\nDeseja continuar jogando? 1-SIM ou clique em qualquer tecla para NÃO: ");
        scanf("%d",&cont);

    }while(cont ==1);

    return 0;
}
