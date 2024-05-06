 #include <iostream>
#include <conio.h>
#include <windows.h>
#include <vector>

// Definiciones de tama�o de pantalla
#define SCREEN_WIDTH 30 
#define SCREEN_HEIGHT 10

// Definiciones de personajes
#define DINO 'A'
#define OBSTACULO 'X'
#define TIERRA '_'

int dinoPosY;
bool estaSaltando;
int obstaculoPos;
int puntuacion;                                      

void gotoxy(int x, int y) {
    COORD coord;
    coord.X = x;
    coord.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}

void dibujarPantalla() {
    for (int y = 0; y < SCREEN_HEIGHT; y++) {
        for (int x = 0; x < SCREEN_WIDTH; x++) {
            if (y == dinoPosY && x == 10) {
                std::cout << DINO;
            } else if (y == SCREEN_HEIGHT) {
                std::cout << TIERRA;
            } else if (x == obstaculoPos && y == 9) {
                std::cout << OBSTACULO;
            } else {
                std::cout << ' ';
            }
        }
        std::cout << std::endl;
    }
}

int main() {
    dinoPosY = SCREEN_HEIGHT - 1;
    estaSaltando = false;
    obstaculoPos = SCREEN_WIDTH - 1;
    puntuacion = 0;

    while (true) {
        if (_kbhit()) {
            char tecla = _getch();
            if (tecla == ' ' && !estaSaltando) {
                estaSaltando = true;
            }
        }

        if (estaSaltando) {
            dinoPosY--;
            if (dinoPosY == SCREEN_HEIGHT - 5) {
                estaSaltando = false;
            }
        } else if (dinoPosY < SCREEN_HEIGHT - 1) {
            dinoPosY++;
        }

        obstaculoPos--;

        if (obstaculoPos == 0) {
            obstaculoPos = SCREEN_WIDTH - 1;
            puntuacion++;
        }

        if (obstaculoPos == 10 && dinoPosY == SCREEN_HEIGHT - 1) {
            break;
        }

        system("cls");
        dibujarPantalla();
        std::cout << "Puntuacion: " << puntuacion << std::endl;

        Sleep(100);
    }

    system("cls");
    std::cout << "Juego terminado. Tu puntuacion: " << puntuacion << std::endl;

    return 0;
}
