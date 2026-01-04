/******************************************************************************

Welcome to GDB Online.
GDB online is an online compiler and debugger tool for C, C++, Python, Java, PHP, Ruby, Perl,
C#, OCaml, VB, Swift, Pascal, Fortran, Haskell, Objective-C, Assembly, HTML, CSS, JS, SQLite, Prolog.
Code, Compile, Run and Debug online from anywhere in world.

*******************************************************************************/
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

#define MAX 36
#define PLAYERS 2
#define LEADERBOARD_MAX 5

/* ================= STRUCT ================= */

typedef struct {
    char name[20];
    int tries;
    double timeUsed;
} Player;

typedef struct {
    char name[20];
    int tries;
    double timeUsed;
} Leaderboard;

/* ================= UTILITY ================= */

void shuffle(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        int r = rand() % size;
        int temp = arr[i];
        arr[i] = arr[r];
        arr[r] = temp;
    }
}

void printBoard(int cards[], int revealed[], int rows, int cols) {
    int index = 0;
    printf("\n   ");
    for (int c = 0; c < cols; c++)
        printf(" %d ", c);
    printf("\n-----------------------\n");

    for (int r = 0; r < rows; r++) {
        printf("%d |", r);
        for (int c = 0; c < cols; c++) {
            if (revealed[index])
                printf(" %d ", cards[index]);
            else
                printf(" * ");
            index++;
        }
        printf("\n");
    }
}

/* ================= FILE HANDLING ================= */

void saveScore(Player p) {
    FILE *fp = fopen("leaderboard.txt", "a");
    if (fp == NULL) return;

    fprintf(fp, "%s %d %.0f\n", p.name, p.tries, p.timeUsed);
    fclose(fp);
}

int loadLeaderboard(Leaderboard lb[]) {
    FILE *fp = fopen("leaderboard.txt", "r");
    int count = 0;
    if (fp == NULL) return 0;

    while (fscanf(fp, "%s %d %lf",
           lb[count].name,
           &lb[count].tries,
           &lb[count].timeUsed) != EOF && count < LEADERBOARD_MAX) {
        count++;
    }
    fclose(fp);
    return count;
}

/* ================= SORTING ================= */

void sortLeaderboard(Leaderboard lb[], int n) {
    Leaderboard temp;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (lb[j].tries > lb[j + 1].tries ||
               (lb[j].tries == lb[j + 1].tries &&
                lb[j].timeUsed > lb[j + 1].timeUsed)) {
                temp = lb[j];
                lb[j] = lb[j + 1];
                lb[j + 1] = temp;
            }
        }
    }
}

/* ================= REPORT ================= */

void showReport() {
    Leaderboard lb[LEADERBOARD_MAX];
    int n = loadLeaderboard(lb);

    if (n == 0) {
        printf("\nBelum ada leaderboard.\n");
        return;
    }

    sortLeaderboard(lb, n);

    printf("\n===== LEADERBOARD =====\n");
    for (int i = 0; i < n; i++) {
        printf("%d. %s | Tries: %d | Time: %.0f sec\n",
               i + 1, lb[i].name, lb[i].tries, lb[i].timeUsed);
    }
}

/* ================= GAME ================= */

void playGame(Player *p, int rows, int cols) {
    int size = rows * cols;
    int pairs = size / 2;
    int cards[MAX];
    int revealed[MAX] = {0};
    int found = 0;

    int idx = 0;
    for (int i = 1; i <= pairs; i++) {
        cards[idx++] = i;
        cards[idx++] = i;
    }

    shuffle(cards, size);

    printf("\n=== Giliran %s ===\n", p->name);
    printf("Tekan ENTER untuk mulai...");
    getchar(); getchar();

    time_t start = time(NULL);

    while (found < pairs) {
        printBoard(cards, revealed, rows, cols);

        int r1, c1, r2, c2;
        printf("Pilih kartu pertama (row col): ");
        scanf("%d %d", &r1, &c1);
        printf("Pilih kartu kedua (row col): ");
        scanf("%d %d", &r2, &c2);

        int i1 = r1 * cols + c1;
        int i2 = r2 * cols + c2;

        if (i1 < 0 || i1 >= size || i2 < 0 || i2 >= size || i1 == i2) {
            printf("Input tidak valid!\n");
            continue;
        }

        p->tries++;

        printf("(%d,%d) = %d\n", r1, c1, cards[i1]);
        printf("(%d,%d) = %d\n", r2, c2, cards[i2]);

        if (cards[i1] == cards[i2]) {
            printf("Cocok!\n");
            revealed[i1] = revealed[i2] = 1;
            found++;
        } else {
            printf("Tidak cocok!\n");
        }
    }

    time_t end = time(NULL);
    p->timeUsed = difftime(end, start);

    printf("\nSelesai %s\n", p->name);
    printf("Percobaan : %d\n", p->tries);
    printf("Waktu     : %.0f detik\n", p->timeUsed);

    saveScore(*p);
}

/* ================= MAIN ================= */

int main() {
    srand(time(NULL));

    int level, rows, cols;
    Player players[PLAYERS];

    printf("Pilih Difficulty Level:\n");
    printf("1. Easy (4x4)\n");
    printf("2. Medium (4x5)\n");
    printf("3. Hard (6x6)\n");
    printf("Pilihan: ");
    scanf("%d", &level);

    switch (level) {
        case 1: rows = 4; cols = 4; break;
        case 2: rows = 4; cols = 5; break;
        case 3: rows = 6; cols = 6; break;
        default:
            printf("Level tidak valid!\n");
            return 0;
    }

    for (int i = 0; i < PLAYERS; i++) {
        printf("Masukkan nama Player %d: ", i + 1);
        scanf("%s", players[i].name);
        players[i].tries = 0;
        players[i].timeUsed = 0;
    }

    for (int i = 0; i < PLAYERS; i++) {
        playGame(&players[i], rows, cols);
    }

    printf("\n===== HASIL AKHIR =====\n");
    for (int i = 0; i < PLAYERS; i++) {
        printf("%s | Time: %.0f sec | Tries: %d\n",
               players[i].name,
               players[i].timeUsed,
               players[i].tries);
    }

    printf("\nPEMENANG: ");
    if (players[0].timeUsed < players[1].timeUsed ||
       (players[0].timeUsed == players[1].timeUsed &&
        players[0].tries < players[1].tries))
        printf("%s\n", players[0].name);
    else
        printf("%s\n", players[1].name);

    showReport();
    return 0;
}
