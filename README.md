# Library-Stock-Manager
An application in the C programming language to manage a library's book stock
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Definirea structurii conform cerintei
struct Carti {
    char titlu[50];
    char autor[50];
    char domeniu[100];
    int id_carte;
};

// Prototipurile functiilor
void introducere(struct Carti *c);
struct Carti* adaugare(struct Carti *biblioteca, int *n);
void afisare(struct Carti *biblioteca, int n);
void cautare(struct Carti *biblioteca, int n);

int main() {
    struct Carti *biblioteca = NULL;
    int n = 0; // Numarul de carti actuale
    int optiune;

    do {
        printf("\n--- MENIU BIBLIOTECA ---\n");
        printf("1. Adaugare carte noua\n");
        printf("2. Afisare toate cartile\n");
        printf("3. Cautare carte dupa ID\n");
        printf("0. Iesire\n");
        printf("Alegeti optiunea: ");
        scanf("%d", &optiune);
        getchar(); // Consumă newline-ul rămas în buffer

        switch (optiune) {
            case 1:
                biblioteca = adaugare(biblioteca, &n);
                break;
            case 2:
                afisare(biblioteca, n);
                break;
            case 3:
                cautare(biblioteca, n);
                break;
            case 0:
                printf("Inchidere program...\n");
                break;
            default:
                printf("Optiune invalida!\n");
        }
    } while (optiune != 0);

    free(biblioteca); // Eliberarea memoriei la final
    return 0;
}

// Functie pentru citirea datelor unei singure carti
void introducere(struct Carti *c) {
    printf("Titlu: ");
    fgets(c->titlu, 50, stdin);
    c->titlu[strcspn(c->titlu, "\n")] = 0; // Elimina newline-ul de la fgets

    printf("Autor: ");
    fgets(c->autor, 50, stdin);
    c->autor[strcspn(c->autor, "\n")] = 0;

    printf("Domeniu: ");
    fgets(c->domeniu, 100, stdin);
    c->domeniu[strcspn(c->domeniu, "\n")] = 0;

    printf("ID Carte: ");
    scanf("%d", &c->id_carte);
    getchar(); 
}

// Functie pentru adaugarea unei carti in vectorul dinamic
struct Carti* adaugare(struct Carti *biblioteca, int *n) {
    (*n)++;
    biblioteca = (struct Carti*) realloc(biblioteca, (*n) * sizeof(struct Carti));
    
    if (biblioteca == NULL) {
        printf("Eroare la alocarea memoriei!\n");
        exit(1);
    }

    printf("\nIntroduceti datele pentru cartea %d:\n", *n);
    introducere(&biblioteca[*n - 1]);
    
    return biblioteca;
}

// Functie pentru afisarea intregii colectii
void afisare(struct Carti *biblioteca, int n) {
    if (n == 0) {
        printf("\nBiblioteca este goala.\n");
        return;
    }

    printf("\n--- LISTA CARTI ---\n");
    for (int i = 0; i < n; i++) {
        printf("ID: %d | Titlu: %s | Autor: %s | Domeniu: %s\n",
               biblioteca[i].id_carte, biblioteca[i].titlu, 
               biblioteca[i].autor, biblioteca[i].domeniu);
    }
}

// Functie pentru cautarea unei carti dupa ID
void cautare(struct Carti *biblioteca, int n) {
    int id_cautat, gasit = 0;
    printf("\nIntroduceti ID-ul cartii cautate: ");
    scanf("%d", &id_cautat);

    for (int i = 0; i < n; i++) {
        if (biblioteca[i].id_carte == id_cautat) {
            printf("Carte gasita! Titlu: %s, Autor: %s\n", 
                   biblioteca[i].titlu, biblioteca[i].autor);
            gasit = 1;
            break;
        }
    }

    if (!gasit) {
        printf("Cartea cu ID-ul %d nu a fost gasita.\n", id_cautat);
    }
}
