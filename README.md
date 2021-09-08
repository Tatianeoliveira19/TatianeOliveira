#Tatiane Oliveira 

#include <stdio.h>
#include <stdlib.h>
#include <windows.h>
#include <time.h>
#include "pessoal.h"

int xc, yc; // coordenadas da posição da "comida" - necessário global para função snake()
int pontos=0; // +500 quando a cobra se alimenta, -1 por "casa" andada

void comida(void) // põe a "comida" para a cobra
{
xc=(rand()%60)+3;
yc=(rand()%24)+2;
gotoxy(xc,yc);
textcolor(WHITE);
printf("%c", 4);
}

void quadro(void) // desenha o quadro do jogo:
{
int n;
textcolor(LIGHTRED);
gotoxy(1,1);
for (n=0; n<62; n++) printf("%c",219); // linha horizontal superior
for (n=0; n<=25; n++) // linha vertical direita
{
gotoxy(63,n);
printf("%c%c",219,219);
}
gotoxy(1,1);
for (n=0; n<25; n++) printf("%c%c\n",219, 219); // linha vertical esquerda
for (n=0; n<64; n++) printf("%c",219); // linha horizontal inferior
}

void snake(void) // TEM MUITO PARA FAZER!!!
{
int vidas=3; // jogador tem três vidas e vai diminuindo...
int tam=4; // tamanho inicial é 4 e aumenta...
int i=0; // contador para desenhar a cobra
int x=3, y=2; // coordenadas da posição atual da cabeça da cobra
char a=77; // pega as setas e lembra a direção a seguir

/** AGORA COMEÇA REALMENTE O JOGO **/
for (vidas=3;vidas>=0;vidas--)
{
comida(); // põe a comida
for (i=0;i==0;) // i será igual a zero até a cobra bater
{
int pos[2][tam+1]; // para lembrar as posições x,y de cada ponto da cobra e +1 para apagar onde passou
pos[0][0]=x; // posição atual
pos[1][0]=y; // ... da cabeça
gotoxy(5,30);
textcolor(YELLOW);
printf("Vidas restantes: %d\tPontos: %6d",vidas, pontos);
textcolor(LIGHTGREEN); // cor da snake
gotoxy(x,y);
printf("%c",15);
fflush(stdin);
if (kbhit()) // se alguma tecla for pressionada
{
a=getch(); // pega mais uma...
switch (a)
{
case 72: y--; break; // seta p/ cima
case 80: y++; break; // seta p/ baixo
case 77: x++; break; // seta direita
case 75: x--; break; // seta esquerda
}
Sleep(10); // espera um tempo menor para uma resposta mais rápida
}
else
{
switch (a)
{
case 72: y--; break; // p/ cima
case 80: y++; break; // p/ baixo
case 77: x++; break; // direita
case 75: x--; break; // esquerda
}
Sleep(120); // espera um certo tempo para o novo movimento
}

if (x==xc && y==yc) // se a cobra comeu a "comida"
{
tam++; // cresce
comida(); // mostra mais
textcolor(LIGHTGREEN); // volta a cor da cobra
pontos+=500; // e ganha 500 pontos!!
}
pontos-=1;
if (y==1 || y==26 || x==2 || x==63) i=1; // se bate em cima, embaixo, esquerda ou direita
}
x=3; // volta para o começo sempre que perde vida
y=2; // volta para o começo sempre que perde vida
a=77; // sempre começa indo para a direita
system("cls"); // limpa toda a tela quando a cobre morre
quadro(); // redesenha o quadro

/**
* - A cobra já está se movendo, até sozinha, mas não fica com tamanho "tam"
* - E ainda perder uma vida quando bate nela mesma (nas paredes já tá certo)...
* Alguma sugestão para isso???
*/

} // sai desse loop sem vida
}

int main (void)
{
char a;
system("title Jogo Snake - não deixe a cobra bater nas paredes nem nela mesma");
system("cls");
srand(time(NULL)); // para randomizar as coordenadas da "comida" da cobra
quadro(); // desenha o quadro do jogo
snake(); // o jogo em si
system("cls");
textcolor(YELLOW);
printf("\n\n\n\n\n\t\t\tFIM DE JOGO!!\n\n\t\t\tPontos: %d\n\n\t\t\tESC para sair...", pontos);
for(a=0;a!=27;a=getch());
return(0);
}
/**

//pessoal.h

#include <windows.h>

typedef enum{BLACK,BLUE,GREEN,CYAN,RED,MAGENTA,BROWN,LIGHTGRAY,DARKGRAY,
LIGHTBLUE,LIGHTGREEN,LIGHTCYAN,LIGHTRED,LIGHTMAGENTA,YELLOW,WHITE} COLORS;

static int __BACKGROUND = BLACK;
static int __FOREGROUND = LIGHTGRAY;

void textcolor (int color)
{
__FOREGROUND = color;
SetConsoleTextAttribute (GetStdHandle (STD_OUTPUT_HANDLE),
color + (__BACKGROUND << 4));
}

void textbackground (int color)
{
__BACKGROUND = color;
SetConsoleTextAttribute (GetStdHandle (STD_OUTPUT_HANDLE),
__FOREGROUND + (color << 4));
}

int gotoxy(DWORD x,DWORD y)
{
COORD Coordenadas;
Coordenadas.X = (x - 1);
Coordenadas.Y = (y - 1);
return (SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),Coordenadas));
/*se por algum erro não mudar o cursor retorna false.*/
}
